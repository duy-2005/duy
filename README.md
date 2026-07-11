/**
 * ==========================================================
 * CHUCNANG_THONGKE.GS - THỐNG KÊ SỐ LIỆU TỪ SHEET "tong hop ADR"
 * ==========================================================
 * Khác với ChucNang_HienThi.gs (lấy nguyên bản để hiển thị danh sách),
 * file này TÍNH TOÁN / TỔNG HỢP số liệu để vẽ biểu đồ.
 *
 * Tìm cột theo TÊN tiêu đề (không theo vị trí cột) để nếu bạn
 * thêm/bớt/đổi thứ tự cột trong Sheet, hàm này vẫn chạy đúng.
 */

/**
 * Hàm chính: trả về toàn bộ số liệu thống kê cho trang Thống kê.
 * @return {Object}
 */
function thongKeADR() {
  try {
    var ss = SpreadsheetApp.getActiveSpreadsheet();
    var sheet = ss.getSheetByName(TEN_SHEET_TONGHOP);

    if (!sheet) {
      throw new Error('Không tìm thấy sheet "' + TEN_SHEET_TONGHOP + '"');
    }

    var lastRow = sheet.getLastRow();
    var lastCol = sheet.getLastColumn();

    if (lastRow < 2) {
      return {
        success: true,
        tongSoBaoCao: 0,
        theoThang: { labels: [], values: [] },
        theoMucDo: { labels: [], values: [] },
        theoKhoa: { labels: [], values: [] },
        theoHoatChat: { labels: [], values: [] },
        theoGioiTinh: { labels: [], values: [] }
      };
    }

    var headers = sheet.getRange(1, 1, 1, lastCol).getValues()[0];
    var data = sheet.getRange(2, 1, lastRow - 1, lastCol).getValues();

    var idxDauThoiGian = timCotTheoTen(headers, "Dấu thời gian");
    var idxMucDo = timCotTheoTen(headers, "Mức độ nghiêm trọng");
    var idxKhoa = timCotTheoTen(headers, "KHOA BÁO CÁO/ XỬ TRÍ ADR");
    var idxHoatChat = timCotTheoTen(headers, "HOẠT CHẤT nghi ngờ ADR");
    var idxGioiTinh = timCotTheoTen(headers, "Giới tính");

    var demTheoThang = {};
    var demTheoMucDo = {};
    var demTheoKhoa = {};
    var demTheoHoatChat = {};
    var demTheoGioiTinh = {};
    var tongSo = 0;

    data.forEach(function (row) {
      var rongHoanToan = row.every(function (c) { return c === "" || c === null; });
      if (rongHoanToan) return;
      tongSo++;

      // --- Theo tháng/năm (dựa vào Dấu thời gian) ---
      if (idxDauThoiGian !== -1) {
        var ngay = chuyenThanhNgayThongKe(row[idxDauThoiGian]);
        if (ngay) {
          var key = ('0' + (ngay.getMonth() + 1)).slice(-2) + '/' + ngay.getFullYear();
          demTheoThang[key] = (demTheoThang[key] || 0) + 1;
        }
      }

      // --- Theo mức độ nghiêm trọng ---
      if (idxMucDo !== -1) {
        var md = String(row[idxMucDo] || "").trim();
        if (md) demTheoMucDo[md] = (demTheoMucDo[md] || 0) + 1;
      }

      // --- Theo khoa báo cáo/xử trí ---
      if (idxKhoa !== -1) {
        var kh = String(row[idxKhoa] || "").trim();
        if (kh) demTheoKhoa[kh] = (demTheoKhoa[kh] || 0) + 1;
      }

      // --- Theo hoạt chất nghi ngờ (1 ô có thể chứa nhiều hoạt chất, tách bởi , ; hoặc xuống dòng) ---
      if (idxHoatChat !== -1) {
        var hc = String(row[idxHoatChat] || "").trim();
        if (hc) {
          hc.split(/[,;\n]+/).forEach(function (ten) {
            var t = ten.trim();
            if (t) demTheoHoatChat[t] = (demTheoHoatChat[t] || 0) + 1;
          });
        }
      }

      // --- Theo giới tính ---
      if (idxGioiTinh !== -1) {
        var gt = String(row[idxGioiTinh] || "").trim();
        if (gt) demTheoGioiTinh[gt] = (demTheoGioiTinh[gt] || 0) + 1;
      }
    });

    return {
      success: true,
      tongSoBaoCao: tongSo,
      theoThang: sapXepTheoThoiGian(demTheoThang),
      theoMucDo: chuyenSangLabelValue(demTheoMucDo, true, null),
      theoKhoa: chuyenSangLabelValue(demTheoKhoa, true, 10),
      theoHoatChat: chuyenSangLabelValue(demTheoHoatChat, true, 10),
      theoGioiTinh: chuyenSangLabelValue(demTheoGioiTinh, true, null)
    };

  } catch (err) {
    return { success: false, message: "Lỗi khi thống kê: " + err.message };
  }
}

// ---------- CÁC HÀM TIỆN ÍCH RIÊNG CHO THỐNG KÊ ----------

// Tìm vị trí cột (0-based) theo đúng tên tiêu đề, không phân biệt hoa/thường/khoảng trắng thừa
function timCotTheoTen(headers, tenCanTim) {
  var muc = String(tenCanTim).trim().toLowerCase();
  for (var i = 0; i < headers.length; i++) {
    if (String(headers[i]).trim().toLowerCase() === muc) return i;
  }
  return -1;
}

// Chuyển 1 ô (Date object hoặc số serial của Google Sheets) thành đối tượng Date thật
function chuyenThanhNgayThongKe(cell) {
  if (Object.prototype.toString.call(cell) === "[object Date]") return cell;
  if (typeof cell === "number") {
    return new Date(Math.round((cell - 25569) * 86400 * 1000));
  }
  return null;
}

// Sắp xếp object đếm theo tháng {"07/2026": 5, "06/2026": 3} theo thứ tự thời gian tăng dần
function sapXepTheoThoiGian(obj) {
  var keys = Object.keys(obj);
  keys.sort(function (a, b) {
    var pa = a.split('/');
    var pb = b.split('/');
    var da = new Date(parseInt(pa[1], 10), parseInt(pa[0], 10) - 1, 1);
    var db = new Date(parseInt(pb[1], 10), parseInt(pb[0], 10) - 1, 1);
    return da - db;
  });
  return { labels: keys, values: keys.map(function (k) { return obj[k]; }) };
}

// Chuyển object đếm {"Nhẹ": 10, "Nặng": 3} thành {labels:[...], values:[...]}
// sapXepGiam: true = sắp xếp giảm dần theo số lượng
// gioiHan: chỉ lấy top N mục (null = lấy hết)
function chuyenSangLabelValue(obj, sapXepGiam, gioiHan) {
  var keys = Object.keys(obj);
  if (sapXepGiam) {
    keys.sort(function (a, b) { return obj[b] - obj[a]; });
  }
  if (gioiHan) keys = keys.slice(0, gioiHan);
  return { labels: keys, values: keys.map(function (k) { return obj[k]; }) };
}
