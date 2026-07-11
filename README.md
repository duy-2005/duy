function getPatients() {
  try {
    var ss = SpreadsheetApp.getActiveSpreadsheet();
    var sheet = ss.getSheetByName(TEN_SHEET_TONGHOP);

    if (!sheet) {
      throw new Error('Không tìm thấy sheet "' + TEN_SHEET_TONGHOP + '"');
    }

    var lastRow = sheet.getLastRow();
    var lastCol = sheet.getLastColumn();

    if (lastRow < 2) {
      // Chỉ có dòng tiêu đề, chưa có dữ liệu
      var headersOnly = sheet.getRange(1, 1, 1, lastCol).getValues()[0];
      return { success: true, headers: headersOnly, rows: [], soDongTrongSheet: [] };
    }

    var headers = sheet.getRange(1, 1, 1, lastCol).getValues()[0];
    var dataRange = sheet.getRange(2, 1, lastRow - 1, lastCol);
    var values = dataRange.getValues();

    // Ghi nhớ số dòng THẬT trong Sheet song song với từng dòng dữ liệu
    // (dòng đầu tiên của dữ liệu là dòng 2 trong Sheet, vì dòng 1 là tiêu đề)
    var soDongThat = values.map(function (_, i) { return i + 2; });

    // Bỏ các dòng hoàn toàn trống (nếu có) - giữ soDongThat đồng bộ theo rows
    var ghepLai = values.map(function (r, i) { return { row: r, soDong: soDongThat[i] }; });

    ghepLai = ghepLai.filter(function (item) {
      return item.row.some(function (cell) {
        return cell !== "" && cell !== null;
      });
    });

    // Chỉ giữ lại dòng nào cột I ("KHOA BÁO CÁO/ XỬ TRÍ ADR") có giá trị.
    // Đây là dấu hiệu cho biết dòng đó đã được xử lý/nhập đầy đủ ở "Tổng hợp ADR",
    // tránh hiện ra hàng loạt dòng trống do công thức kéo từ "Copy" sang.
    // Muốn đổi cột lọc, chỉ cần sửa chữ cái 'I' bên dưới.
    var COT_LOC = 'I';
    var idxCotLoc = chuCaiSangIndexHienThi(COT_LOC);
    ghepLai = ghepLai.filter(function (item) {
      var giaTri = item.row[idxCotLoc];
      return giaTri !== "" && giaTri !== null && giaTri !== undefined;
    });

    var rows = ghepLai.map(function (item) { return item.row; });
    var soDongTrongSheet = ghepLai.map(function (item) { return item.soDong; });

    // Chuẩn hóa Date -> chuỗi dễ đọc để hiển thị lên web
    // Xử lý 2 trường hợp: (1) cell là Date object thật, (2) cell là số serial
    // (VD 46046.79...) ở các cột có tên gợi ý là ngày/thời gian nhưng sheet
    // chưa format thành Date (thường do ARRAYFORMULA/VLOOKUP kéo từ Copy sang).
    var CAC_TU_KHOA_NGAY = ["ngày", "dấu thời gian", "date"];

    function laCotNgayThang(tenCot) {
      var t = String(tenCot).toLowerCase();
      return CAC_TU_KHOA_NGAY.some(function (kw) { return t.indexOf(kw) !== -1; });
    }

    rows = rows.map(function (r) {
      return r.map(function (cell, colIndex) {
        if (Object.prototype.toString.call(cell) === "[object Date]") {
          return Utilities.formatDate(cell, Session.getScriptTimeZone(), "dd/MM/yyyy HH:mm");
        }
        // Số serial ngày/giờ kiểu Google Sheets (VD 46046.79...)
        if (typeof cell === "number" && laCotNgayThang(headers[colIndex])) {
          var ngayThucTe = new Date(Math.round((cell - 25569) * 86400 * 1000));
          return Utilities.formatDate(ngayThucTe, Session.getScriptTimeZone(), "dd/MM/yyyy HH:mm");
        }
        return cell;
      });
    });

    return { success: true, headers: headers, rows: rows, soDongTrongSheet: soDongTrongSheet };

  } catch (err) {
    return { success: false, message: "Lỗi khi tải danh sách: " + err.message, headers: [], rows: [] };
  }
}

// Chuyển "A","B",...,"I",...,"Z","AA"... thành số cột 0-based
// (VD: "I" -> 8, vì I là chữ cái thứ 9 trong bảng chữ cái)
function chuCaiSangIndexHienThi(letter) {
  var result = 0;
  for (var i = 0; i < letter.length; i++) {
    result = result * 26 + (letter.charCodeAt(i) - 64);
  }
  return result - 1;
}
