/**
 * ==========================================================
 * CODE_MAIN.GS - BỘ ĐIỀU PHỐI CHÍNH (Đã tích hợp xác thực)
 * ==========================================================
 * Nhiệm vụ: Nhận request từ trình duyệt, trả về giao diện Index.html
 * và xử lý các logic đăng nhập.
 */

// ---- CẤU HÌNH CHUNG (dùng lại ở các file khác) ----
var TEN_SHEET_COPY = "Copy";
var TEN_SHEET_TONGHOP = "tong hop ADR";
var TEN_SHEET_DANHSACHKHOA = "DanhSachKhoa"; // Cấu hình thêm sheet chứa mật khẩu

/**
 * Hàm bắt buộc của Apps Script Web App.
 * Được gọi mỗi khi người dùng truy cập vào URL của Web App.
 */
function doGet(e) {
  return HtmlService.createHtmlOutputFromFile('Index')
    .setTitle('Phần mềm Báo cáo ADR')
    .setXFrameOptionsMode(HtmlService.XFrameOptionsMode.ALLOWALL)
    .addMetaTag('viewport', 'width=device-width, initial-scale=1');
}

/**
 * Hàm tiện ích: cho phép Index.html include các phần HTML/CSS/JS
 * tách riêng nếu sau này bạn muốn chia nhỏ file (không bắt buộc dùng).
 */
function include(filename) {
  return HtmlService.createHtmlOutputFromFile(filename).getContent();
}

// ==========================================================
// CÁC HÀM XỬ LÝ XÁC THỰC KHOA PHÒNG (BƯỚC 2)
// ==========================================================

/**
 * Hàm 1: Lấy danh sách các khoa gửi về cho giao diện web
 */
function getDanhSachKhoa() {
  try {
    var ss = SpreadsheetApp.getActiveSpreadsheet();
    var sheet = ss.getSheetByName(TEN_SHEET_DANHSACHKHOA);
    if (!sheet) return [];

    var data = sheet.getDataRange().getValues();
    var danhSach = [];
    
    // Quét từ dòng 2 (bỏ qua dòng tiêu đề) để lấy tên khoa ở cột A (cột số 0)
    for (var i = 1; i < data.length; i++) {
      if (data[i][0] !== "") {
        danhSach.push(data[i][0].toString().trim());
      }
    }
    return danhSach;
  } catch (error) {
    return []; // Trả về mảng rỗng nếu có lỗi
  }
}

/**
 * Hàm 2: Kiểm tra mật khẩu từ giao diện gửi lên có khớp với Google Sheets không
 */
function kiemTraMatKhau(tenKhoa, matKhau) {
  try {
    var ss = SpreadsheetApp.getActiveSpreadsheet();
    var sheet = ss.getSheetByName(TEN_SHEET_DANHSACHKHOA);
    if (!sheet) return false;

    var data = sheet.getDataRange().getValues();
    
    // Quét từ dòng 2 để đối chiếu
    for (var i = 1; i < data.length; i++) {
      var khoaTrongSheet = data[i][0].toString().trim();
      var matKhauTrongSheet = data[i][1].toString().trim();
      
      // Nếu khớp cả tên khoa và mật khẩu thì cho phép đăng nhập
      if (khoaTrongSheet === tenKhoa && matKhauTrongSheet === matKhau) {
        return true; 
      }
    }
    
    // Quét hết bảng mà không khớp dòng nào thì từ chối
    return false; 
  } catch (error) {
    return false;
  }
}
