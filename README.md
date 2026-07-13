/**
 * ==========================================================
 * CHUCNANG_HIENTHI.GS - ĐỌC DỮ LIỆU TỪ SHEET "Tổng hợp ADR"
 * ==========================================================
 * Sheet này có rất nhiều cột (~60) và cũng có tiêu đề trùng
 * (VD "TDĐT 1..8"). Vì vậy KHÔNG map thành Object theo tên cột,
 * mà trả về dạng {headers: [...], rows: [[...]]} để Frontend
 * tự vẽ bảng theo đúng thứ tự cột hiện có trong Sheet.
 * ==> Bạn thêm/bớt cột trong Sheet "Tổng hợp ADR" thoải mái,
 *     không cần sửa code này.
 */

/**
 * Lấy toàn bộ dữ liệu từ sheet "Tổng hợp ADR" để hiển thị lên web.
 * @return {Object} {success, headers: string[], rows: any[][]}
 */
// Thêm tham số tenKhoa để nhận tên khoa từ Index.html truyền xuống
function getPatients(tenKhoa) {
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
    var COT_LOC = 'I';
    var idxCotLoc = chuCaiSangIndexHienThi(COT_LOC);
    ghepLai = ghepLai.filter(function (item) {
      var giaTri = item.row[idxCotLoc];
      return giaTri !== "" && giaTri !== null && giaTri !== undefined;
    });


    // =========================================================================
    // THÊM MỚI BỞI AI: LỌC DỮ LIỆU THEO KHOA ĐĂNG NHẬP
    // =========================================================================
    // QUAN TRỌNG: Bạn hãy kiểm tra trên dòng 1 của Sheet Tổng hợp, 
    // xem tiêu đề cột chứa tên Khoa là gì thì sửa lại chuỗi bên dưới cho khớp 100%
    var TEN_COT_KHOA = "KHOA BÁO CÁO/ XỬ TRÍ ADR"; 
    var idxCotKhoa = headers.indexOf(TEN_COT_KHOA);

    // Nếu tìm thấy cột Khoa và có tên khoa đăng nhập truyền vào
    if (idxCotKhoa !== -1 && tenKhoa && tenKhoa !== "") {
      ghepLai = ghepLai.filter(function (item) {
        var khoaCuaDongNay = item.row[idxCotKhoa];
        if (khoaCuaDongNay) {
          // Xóa khoảng trắng 2 đầu và chuyển về chữ thường để so sánh tránh lỗi gõ phím
          return khoaCuaDongNay.toString().trim().toLowerCase() === tenKhoa.toString().trim().toLowerCase();
        }
        return false;
      });
    }
    // =========================================================================


    var rows = ghepLai.map(function (item) { return item.row; });
    var soDongTrongSheet = ghepLai.map(function (item) { return item.soDong; });

    // Chuẩn hóa Date -> chuỗi dễ đọc để hiển thị lên web
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

    // Tôi đã thêm lệnh .reverse() vào rows và soDongTrongSheet để các báo cáo 
    // mới nhất (nằm dưới cùng ở Sheet) sẽ tự động hiển thị lên trên cùng của Web.
    return { 
      success: true, 
      headers: headers, 
      rows: rows.reverse(), 
      soDongTrongSheet: soDongTrongSheet.reverse() 
    };

  } catch (err) {
    return { success: false, message: "Lỗi khi tải danh sách: " + err.message, headers: [], rows: [] };
  }
}

// Chuyển "A","B",...,"I",...,"Z","AA"... thành số cột 0-based
function chuCaiSangIndexHienThi(letter) {
  var result = 0;
  for (var i = 0; i < letter.length; i++) {
    result = result * 26 + (letter.charCodeAt(i) - 64);
  }
  return result - 1;
}
