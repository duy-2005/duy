<!DOCTYPE html>
<html>
<head>
  <base target="_top">
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">

  <!-- Google Fonts -->
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700;800&family=Roboto:wght@400;500;700&display=swap" rel="stylesheet">

  <!-- Font Awesome Icons -->
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.1/css/all.min.css">

  <!-- Chart.js cho tab Thống kê -->
  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>

  <style>
    :root {
      --red-50:  #fef2f2;
      --red-100: #fee2e2;
      --red-200: #fecaca;
      --red-400: #f87171;
      --red-500: #ef4444;
      --red-600: #dc2626;
      --red-700: #b91c1c;
      --red-800: #991b1b;
      --ink: #1f2430;
      --ink-soft: #4b5563;
      --muted: #9aa4b2;
      --line: #ece7e7;
      --bg: #faf7f7;
      --card-bg: #ffffff;
      --shadow-sm: 0 1px 3px rgba(185,28,28,0.06);
      --shadow-md: 0 6px 18px rgba(185,28,28,0.10);
      --shadow-lg: 0 14px 36px rgba(185,28,28,0.16);
      --radius: 14px;
    }

    * { box-sizing: border-box; }
    html { scroll-behavior: smooth; }
    body {
      font-family: 'Inter', 'Roboto', 'Segoe UI', Arial, sans-serif;
      background: var(--bg);
      background-image:
        radial-gradient(circle at 100% 0%, rgba(220,38,38,0.06), transparent 40%),
        radial-gradient(circle at 0% 100%, rgba(220,38,38,0.05), transparent 35%);
      margin: 0;
      color: var(--ink);
      -webkit-font-smoothing: antialiased;
    }

    /* ============ HEADER ============ */
    header {
      background: linear-gradient(120deg, var(--red-700) 0%, var(--red-600) 55%, var(--red-500) 100%);
      color: #fff;
      padding: 22px 28px;
      box-shadow: 0 4px 20px rgba(185,28,28,0.25);
      position: sticky;
      top: 0;
      z-index: 30;
      display: flex;
      align-items: center;
      gap: 14px;
    }
    header .header-icon {
      width: 42px; height: 42px;
      background: rgba(255,255,255,0.18);
      border-radius: 12px;
      display: flex; align-items: center; justify-content: center;
      font-size: 19px;
      flex-shrink: 0;
    }
    header h1 {
      margin: 0;
      font-size: 21px;
      font-weight: 800;
      letter-spacing: 0.2px;
    }
    header .header-sub {
      font-size: 12.5px;
      font-weight: 500;
      opacity: 0.85;
      margin-top: 2px;
    }

    /* ============ TABS ============ */
    .tabs {
      display: flex;
      background: #fff;
      border-bottom: 1px solid var(--line);
      padding: 0 20px;
      position: sticky;
      top: 78px;
      z-index: 25;
      gap: 4px;
    }
    .tab-btn {
      display: flex; align-items: center; gap: 8px;
      padding: 15px 20px;
      cursor: pointer;
      border: none;
      background: none;
      font-family: inherit;
      font-size: 14.5px;
      font-weight: 600;
      color: var(--muted);
      border-bottom: 3px solid transparent;
      transition: color 0.15s, border-color 0.15s;
    }
    .tab-btn i { font-size: 15px; }
    .tab-btn:hover { color: var(--red-600); }
    .tab-btn.active {
      color: var(--red-600);
      border-bottom: 3px solid var(--red-600);
    }

    /* ============ TAB THỐNG KÊ ============ */
    .stat-row {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(190px, 1fr));
      gap: 14px;
      margin-bottom: 22px;
    }
    .stat-card {
      background: #fff;
      border-radius: 14px;
      padding: 18px 20px;
      border: 1px solid var(--line);
      box-shadow: var(--shadow-sm);
      display: flex;
      align-items: center;
      gap: 14px;
    }
    .stat-card .icon {
      width: 42px; height: 42px;
      border-radius: 11px;
      background: var(--red-50);
      color: var(--red-600);
      display: flex; align-items: center; justify-content: center;
      font-size: 18px;
      flex-shrink: 0;
    }
    .stat-card .num { font-size: 24px; font-weight: 800; color: var(--ink); line-height: 1.1; }
    .stat-card .label { font-size: 12px; color: var(--muted); font-weight: 500; margin-top: 2px; }

    .chart-grid {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(380px, 1fr));
      gap: 18px;
    }
    .chart-card {
      background: #fff;
      border-radius: 14px;
      padding: 20px;
      border: 1px solid var(--line);
      box-shadow: var(--shadow-sm);
    }
    .chart-card.full { grid-column: 1 / -1; }
    .chart-card h3 {
      margin: 0 0 14px;
      font-size: 14px;
      font-weight: 700;
      color: var(--ink);
      display: flex;
      align-items: center;
      gap: 8px;
    }
    .chart-card h3 i { color: var(--red-500); font-size: 14px; }
    .chart-wrap { position: relative; height: 280px; }

    @media (max-width: 780px) {
      .chart-grid { grid-template-columns: 1fr; }
      .chart-wrap { height: 240px; }
    }

    .container { padding: 24px 22px 60px; max-width: 1200px; margin: 0 auto; }
    .tab-content { display: none; }
    .tab-content.active { display: block; animation: fadeIn 0.25s ease; }
    @keyframes fadeIn { from { opacity: 0; transform: translateY(4px); } to { opacity: 1; transform: translateY(0); } }

    .card {
      background: var(--card-bg);
      border-radius: var(--radius);
      padding: 26px;
      box-shadow: var(--shadow-sm);
      border: 1px solid var(--line);
    }

    /* ============ FORM ============ */
    fieldset {
      border: 1px solid var(--line);
      border-radius: 12px;
      margin-bottom: 20px;
      padding: 18px 20px 20px;
      background: linear-gradient(180deg, #fffefe 0%, #fffafa 100%);
    }
    legend {
      font-weight: 700;
      color: var(--red-700);
      padding: 2px 10px;
      font-size: 14.5px;
      background: var(--red-50);
      border-radius: 8px;
      display: flex;
      align-items: center;
      gap: 8px;
    }
    legend i { color: var(--red-500); }

    .form-grid {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
      gap: 16px 18px;
      margin-top: 6px;
    }
    label {
      display: block;
      font-size: 12.5px;
      font-weight: 600;
      margin-bottom: 6px;
      color: var(--ink-soft);
      letter-spacing: 0.1px;
    }
    input, select, textarea {
      width: 100%;
      padding: 10px 13px;
      border: 1.5px solid #e2dede;
      border-radius: 9px;
      font-size: 14px;
      font-family: inherit;
      color: var(--ink);
      background: #fff;
      transition: border-color 0.15s, box-shadow 0.15s, transform 0.05s;
    }
    input:focus, select:focus, textarea:focus {
      outline: none;
      border-color: var(--red-500);
      box-shadow: 0 0 0 4px rgba(239,68,68,0.14);
    }
    input:hover, select:hover, textarea:hover { border-color: #d9d0d0; }
    textarea { resize: vertical; min-height: 56px; }

    .btn {
      background: linear-gradient(135deg, var(--red-600), var(--red-500));
      color: #fff;
      border: none;
      padding: 15px 38px;
      border-radius: 12px;
      font-size: 15.5px;
      font-weight: 700;
      font-family: inherit;
      cursor: pointer;
      margin-top: 8px;
      box-shadow: 0 8px 20px rgba(220,38,38,0.30);
      display: inline-flex;
      align-items: center;
      gap: 10px;
      transition: transform 0.12s ease, box-shadow 0.12s ease, background 0.12s ease;
      letter-spacing: 0.2px;
    }
    .btn:hover {
      background: linear-gradient(135deg, var(--red-700), var(--red-600));
      transform: translateY(-2px);
      box-shadow: 0 12px 26px rgba(220,38,38,0.38);
    }
    .btn:active { transform: translateY(0); }
    .btn:disabled {
      background: #e8b4b4;
      cursor: not-allowed;
      box-shadow: none;
      transform: none;
    }

    #formMsg {
      margin-top: 16px;
      padding: 13px 16px;
      border-radius: 10px;
      font-size: 14px;
      font-weight: 500;
      display: none;
      align-items: center;
      gap: 8px;
    }
    #formMsg.ok { background: #ecfdf3; color: #15803d; display: flex; border: 1px solid #bbf7d0; }
    #formMsg.err { background: var(--red-50); color: var(--red-700); display: flex; border: 1px solid var(--red-200); }

    /* ============ LOADING SPINNER ============ */
    .spinner-wrap {
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      gap: 14px;
      padding: 46px 20px;
      color: var(--muted);
      font-size: 14px;
      font-weight: 500;
    }
    .spinner {
      width: 42px; height: 42px;
      border-radius: 50%;
      border: 4px solid var(--red-100);
      border-top-color: var(--red-600);
      animation: spin 0.8s linear infinite;
    }
    @keyframes spin { to { transform: rotate(360deg); } }

    #emptyMsg { padding: 40px 20px; color: var(--muted); text-align: center; font-size: 14px; }
    #emptyMsg i { font-size: 34px; color: var(--red-200); display: block; margin-bottom: 10px; }

    /* ============ Ô TÌM KIẾM ============ */
    .search-wrap { position: relative; margin-bottom: 8px; }
    .search-wrap i {
      position: absolute; left: 16px; top: 50%; transform: translateY(-50%);
      color: var(--red-400); font-size: 15px;
    }
    #searchBox {
      width: 100%;
      padding: 13px 16px 13px 42px;
      border: 1.5px solid #e2dede;
      border-radius: 12px;
      font-size: 14.5px;
      font-family: inherit;
      transition: border-color 0.15s, box-shadow 0.15s;
      background: #fff;
    }
    #searchBox:focus {
      outline: none;
      border-color: var(--red-500);
      box-shadow: 0 0 0 4px rgba(239,68,68,0.13);
    }
    #resultCount {
      font-size: 12.5px;
      color: var(--muted);
      margin: 10px 2px 16px;
      font-weight: 500;
    }

    /* ============ DANH SÁCH DẠNG THẺ (kiểu "bảng hiện đại") ============ */
    .list-headbar {
      display: grid;
      grid-template-columns: 22px minmax(150px, 1.2fr) minmax(120px, 0.9fr) minmax(160px, 1.2fr) minmax(220px, 2fr);
      gap: 10px 20px;
      padding: 10px 18px;
      font-size: 10.5px;
      font-weight: 700;
      text-transform: uppercase;
      letter-spacing: 0.5px;
      color: #fff;
      background: linear-gradient(120deg, var(--red-700), var(--red-600));
      border-radius: 10px 10px 0 0;
      position: sticky;
      top: 132px;
      z-index: 10;
    }
    .list-headbar > div:first-child { visibility: hidden; }

    #cardList { border-radius: 0 0 10px 10px; overflow: hidden; border: 1px solid var(--line); border-top: none; }
    #cardList:empty { border: none; }

    .adr-card {
      border-bottom: 1px solid var(--line);
      overflow: hidden;
      transition: box-shadow 0.15s, background 0.15s;
      background: #fff;
    }
    .adr-card:last-child { border-bottom: none; }
    /* striped rows */
    .adr-card:nth-child(even) .adr-summary { background: #fdfafa; }
    .adr-card:nth-child(even).open .adr-summary { background: var(--red-50); }

    .adr-summary {
      display: grid;
      grid-template-columns: 22px minmax(150px, 1.2fr) minmax(120px, 0.9fr) minmax(160px, 1.2fr) minmax(220px, 2fr);
      gap: 10px 20px;
      align-items: center;
      padding: 15px 18px;
      cursor: pointer;
      background: #fff;
      transition: background 0.12s;
    }
    .adr-summary > div { min-width: 0; }
    .adr-summary:hover { background: var(--red-50); }

    .adr-card.open {
      box-shadow: inset 3px 0 0 var(--red-600);
    }
    .adr-card.open .adr-summary {
      background: var(--red-50);
    }
    .adr-card.open .adr-summary:hover { background: var(--red-100); }

    .adr-chevron {
      transition: transform 0.15s;
      color: var(--muted);
      font-size: 12px;
    }
    .adr-card.open .adr-chevron { transform: rotate(90deg); color: var(--red-600); }

    .adr-field-label {
      font-size: 10.5px;
      font-weight: 700;
      color: var(--muted);
      text-transform: uppercase;
      letter-spacing: 0.4px;
      margin-bottom: 3px;
      white-space: nowrap;
      overflow: hidden;
      text-overflow: ellipsis;
    }
    .adr-field-value {
      font-size: 14px;
      font-weight: 500;
      color: var(--ink);
      overflow: hidden;
      text-overflow: ellipsis;
      white-space: nowrap;
      letter-spacing: 0.1px;
    }

    .adr-detail {
      display: none;
      background: #fffafa;
      border-top: 1px dashed var(--red-200);
      padding: 18px 20px;
    }
    .adr-card.open .adr-detail { display: block; }

    .adr-detail-grid {
      display: grid;
      grid-template-columns: repeat(auto-fill, minmax(240px, 1fr));
      gap: 12px 22px;
    }
    .adr-detail-item {
      font-size: 13px;
      border-bottom: 1px dotted var(--line);
      padding-bottom: 7px;
    }
    .adr-detail-item .adr-field-label { margin-bottom: 3px; }
    .adr-detail-item .adr-field-value {
      white-space: normal;
      word-break: break-word;
      font-weight: 400;
    }

    /* ---- Sửa nội dung 1 vài cột trong thẻ ---- */
    .adr-detail-item.co-the-sua .adr-field-label { color: var(--red-600); }
    .adr-detail-item.co-the-sua .adr-field-label::after {
      content: " ✎";
    }
    .adr-edit-input {
      width: 100%;
      padding: 6px 9px;
      border: 1.5px solid var(--red-400);
      border-radius: 7px;
      font-size: 13px;
      font-family: inherit;
      background: #fff;
    }
    .adr-edit-input:focus {
      outline: none;
      box-shadow: 0 0 0 3px rgba(239,68,68,0.15);
    }

    .adr-edit-toolbar {
      margin-top: 14px;
      padding-top: 14px;
      border-top: 1px solid var(--line);
      display: flex;
      gap: 10px;
      flex-wrap: wrap;
    }
    .adr-btn-sm {
      display: inline-flex;
      align-items: center;
      gap: 7px;
      padding: 8px 16px;
      border-radius: 8px;
      font-size: 13px;
      font-weight: 600;
      font-family: inherit;
      cursor: pointer;
      border: none;
      transition: transform 0.1s, box-shadow 0.1s;
    }
    .adr-btn-sm:hover { transform: translateY(-1px); }
    .adr-btn-edit { background: var(--red-50); color: var(--red-600); border: 1.5px solid var(--red-200); }
    .adr-btn-edit:hover { background: var(--red-100); }
    .adr-btn-save { background: linear-gradient(135deg, var(--red-600), var(--red-500)); color: #fff; box-shadow: 0 4px 12px rgba(220,38,38,0.25); }
    .adr-btn-cancel { background: #f1f2f4; color: var(--ink-soft); }
    .adr-btn-sm:disabled { opacity: 0.6; cursor: not-allowed; transform: none; }

    .adr-save-msg {
      font-size: 12.5px;
      font-weight: 600;
      margin-left: 4px;
      align-self: center;
    }
    .adr-save-msg.ok { color: #15803d; }
    .adr-save-msg.err { color: var(--red-700); }

    /* ============ RESPONSIVE ============ */
    @media (max-width: 780px) {
      .container { padding: 16px 12px 50px; }
      header { padding: 16px 18px; }
      header h1 { font-size: 17px; }
      .tabs { top: 68px; padding: 0 8px; }
      .tab-btn { padding: 13px 14px; font-size: 13.5px; }
      .card { padding: 16px; }

      .list-headbar { display: none; }
      .adr-summary {
        grid-template-columns: 18px 1fr;
        grid-auto-rows: auto;
        row-gap: 8px;
      }
      .adr-detail-grid { grid-template-columns: 1fr; }
    }
  </style>
</head>
<body>

  <header>
    <div class="header-icon"><i class="fa-solid fa-notes-medical"></i></div>
    <div>
      <h1>Phần mềm Báo cáo ADR</h1>
      <div class="header-sub">Hệ thống ghi nhận & tra cứu phản ứng có hại của thuốc</div>
    </div>
  </header>

  <div class="tabs">
    <button class="tab-btn active" data-tab="tab-form"><i class="fa-solid fa-file-pen"></i> Nhập báo cáo</button>
    <button class="tab-btn" data-tab="tab-list"><i class="fa-solid fa-table-list"></i> Danh sách ADR</button>
    <button class="tab-btn" data-tab="tab-thongke"><i class="fa-solid fa-chart-pie"></i> Thống kê</button>
  </div>

  <div class="container">

    <!-- ===================== TAB 1: FORM NHẬP ===================== -->
    <div id="tab-form" class="tab-content active">
      <div class="card">
        <form id="adrForm">

          <fieldset>
            <legend><i class="fa-solid fa-user"></i> Thông tin bệnh nhân & người báo cáo</legend>
            <div class="form-grid">
              <div>
                <label>Họ và tên bệnh nhân</label>
                <input type="text" name="ten_benh_nhan">
              </div>
              <div>
                <label>Tuổi</label>
                <input type="number" name="tuoi" min="0">
              </div>
              <div>
                <label>Giới tính</label>
                <select name="gioi_tinh">
                  <option value="">-- Chọn --</option>
                  <option value="Nam">Nam</option>
                  <option value="Nữ">Nữ</option>
                </select>
              </div>
              <div>
                <label>Khoa phòng ghi nhận ADR</label>
                <input type="text" name="khoa_phong">
              </div>
              <div>
                <label>Họ và tên người báo cáo</label>
                <input type="text" name="ten_nguoi_bc">
              </div>
              <div>
                <label>Email người báo cáo</label>
                <input type="email" name="email_nguoi_bc">
              </div>
              <div>
                <label>Số điện thoại</label>
                <input type="tel" name="sdt">
              </div>
              <div>
                <label>Nghề nghiệp/Chức vụ</label>
                <input type="text" name="nghe_nghiep">
              </div>
            </div>
          </fieldset>

          <fieldset>
            <legend><i class="fa-solid fa-triangle-exclamation"></i> Diễn biến phản ứng ADR</legend>
            <div class="form-grid">
              <div>
                <label>Ngày xuất hiện phản ứng</label>
                <input type="date" name="ngay_xuat_hien">
              </div>
              <div>
                <label>Mức độ nghiêm trọng của phản ứng</label>
                <input type="text" name="muc_do_nghiem_trong">
              </div>
              <div style="grid-column: 1 / -1;">
                <label>Mô tả biểu hiện ADR (ngắn gọn)</label>
                <textarea name="mo_ta_adr"></textarea>
              </div>
              <div style="grid-column: 1 / -1;">
                <label>Xử trí phản ứng</label>
                <textarea name="xu_tri"></textarea>
              </div>
            </div>
          </fieldset>

          <!-- Lặp 4 nhóm thuốc nghi ngờ -->
          <fieldset>
            <legend><i class="fa-solid fa-pills"></i> Thuốc nghi ngờ 1</legend>
            <div class="form-grid">
              <div><label>Tên thuốc (biệt dược)</label><input type="text" name="ten_thuoc_1"></div>
              <div><label>Số lô thuốc</label><input type="text" name="so_lo_1"></div>
              <div><label>Liều dùng 1 lần + số lần dùng/ngày</label><input type="text" name="lieu_dung_1"></div>
              <div><label>Thời gian xuất hiện từ lần dùng cuối</label><input type="text" name="tg_xuat_hien_1"></div>
              <div><label>Sau ngừng/giảm liều, có cải thiện không</label><input type="text" name="cai_thien_1"></div>
            </div>
          </fieldset>

          <fieldset>
            <legend><i class="fa-solid fa-pills"></i> Thuốc nghi ngờ 2</legend>
            <div class="form-grid">
              <div><label>Tên thuốc (biệt dược)</label><input type="text" name="ten_thuoc_2"></div>
              <div><label>Số lô thuốc</label><input type="text" name="so_lo_2"></div>
              <div><label>Liều dùng 1 lần + số lần dùng/ngày</label><input type="text" name="lieu_dung_2"></div>
              <div><label>Thời gian xuất hiện từ lần dùng cuối</label><input type="text" name="tg_xuat_hien_2"></div>
              <div><label>Sau ngừng/giảm liều, có cải thiện không</label><input type="text" name="cai_thien_2"></div>
            </div>
          </fieldset>

          <fieldset>
            <legend><i class="fa-solid fa-pills"></i> Thuốc nghi ngờ 3</legend>
            <div class="form-grid">
              <div><label>Tên thuốc (biệt dược)</label><input type="text" name="ten_thuoc_3"></div>
              <div><label>Số lô thuốc</label><input type="text" name="so_lo_3"></div>
              <div><label>Liều dùng 1 lần + số lần dùng/ngày</label><input type="text" name="lieu_dung_3"></div>
              <div><label>Thời gian xuất hiện từ lần dùng cuối</label><input type="text" name="tg_xuat_hien_3"></div>
              <div><label>Sau ngừng/giảm liều, có cải thiện không</label><input type="text" name="cai_thien_3"></div>
            </div>
          </fieldset>

          <fieldset>
            <legend><i class="fa-solid fa-pills"></i> Thuốc nghi ngờ 4</legend>
            <div class="form-grid">
              <div><label>Tên thuốc (biệt dược)</label><input type="text" name="ten_thuoc_4"></div>
              <div><label>Số lô thuốc</label><input type="text" name="so_lo_4"></div>
              <div><label>Liều dùng 1 lần + số lần dùng/ngày</label><input type="text" name="lieu_dung_4"></div>
              <div><label>Thời gian xuất hiện từ lần dùng cuối</label><input type="text" name="tg_xuat_hien_4"></div>
              <div><label>Sau ngừng/giảm liều, có cải thiện không</label><input type="text" name="cai_thien_4"></div>
            </div>
          </fieldset>

          <fieldset>
            <legend><i class="fa-solid fa-plus"></i> Thông tin bổ sung (nếu cần)</legend>
            <div class="form-grid">
              <div><label>Cột 1</label><input type="text" name="cot_1"></div>
              <div><label>Cột 2</label><input type="text" name="cot_2"></div>
              <div><label>Cột 3</label><input type="text" name="cot_3"></div>
            </div>
          </fieldset>

          <button type="submit" class="btn" id="submitBtn"><i class="fa-solid fa-floppy-disk"></i> Lưu báo cáo</button>
        </form>

        <div id="formMsg"></div>
      </div>
    </div>

    <!-- ===================== TAB 2: DANH SÁCH ===================== -->
    <div id="tab-list" class="tab-content">
      <div class="card">
        <div class="search-wrap">
          <i class="fa-solid fa-magnifying-glass"></i>
          <input type="text" id="searchBox" placeholder="Tìm theo tên bệnh nhân, mã hồ sơ, mô tả ADR...">
        </div>
        <div id="resultCount"></div>

        <div id="loadingMsg">
          <div class="spinner-wrap">
            <div class="spinner"></div>
            <div>Đang tải dữ liệu...</div>
          </div>
        </div>

        <div id="emptyMsg" style="display:none;">
          <i class="fa-regular fa-folder-open"></i>
          Chưa có dữ liệu trong sheet "tong hop ADR".
        </div>

        <div class="list-headbar">
          <div></div>
          <div>Bệnh nhân</div>
          <div>Thời gian</div>
          <div>Thông tin NB</div>
          <div>Mô tả ADR</div>
        </div>
        <div id="cardList"></div>
      </div>
    </div>

    <!-- ===================== TAB 3: THỐNG KÊ ===================== -->
    <div id="tab-thongke" class="tab-content">
      <div id="tkLoading">
        <div class="spinner-wrap">
          <div class="spinner"></div>
          <div>Đang tính toán số liệu thống kê...</div>
        </div>
      </div>

      <div id="tkError" style="display:none;"></div>

      <div id="tkContent" style="display:none;">
        <div class="stat-row">
          <div class="stat-card">
            <div class="icon"><i class="fa-solid fa-file-medical"></i></div>
            <div>
              <div class="num" id="statTongSo">0</div>
              <div class="label">Tổng số báo cáo ADR</div>
            </div>
          </div>
          <div class="stat-card">
            <div class="icon"><i class="fa-solid fa-hospital"></i></div>
            <div>
              <div class="num" id="statSoKhoa">0</div>
              <div class="label">Khoa/phòng có báo cáo</div>
            </div>
          </div>
          <div class="stat-card">
            <div class="icon"><i class="fa-solid fa-capsules"></i></div>
            <div>
              <div class="num" id="statSoHoatChat">0</div>
              <div class="label">Hoạt chất nghi ngờ khác nhau</div>
            </div>
          </div>
          <div class="stat-card">
            <div class="icon"><i class="fa-solid fa-calendar-days"></i></div>
            <div>
              <div class="num" id="statSoThang">0</div>
              <div class="label">Tháng có dữ liệu</div>
            </div>
          </div>
        </div>

        <div class="chart-grid">
          <div class="chart-card full">
            <h3><i class="fa-solid fa-chart-line"></i> Xu hướng số ca ADR theo tháng</h3>
            <div class="chart-wrap"><canvas id="chartThang"></canvas></div>
          </div>

          <div class="chart-card">
            <h3><i class="fa-solid fa-gauge-high"></i> Theo mức độ nghiêm trọng</h3>
            <div class="chart-wrap"><canvas id="chartMucDo"></canvas></div>
          </div>

          <div class="chart-card">
            <h3><i class="fa-solid fa-venus-mars"></i> Theo giới tính</h3>
            <div class="chart-wrap"><canvas id="chartGioiTinh"></canvas></div>
          </div>

          <div class="chart-card">
            <h3><i class="fa-solid fa-hospital"></i> Top 10 khoa nhiều báo cáo nhất</h3>
            <div class="chart-wrap"><canvas id="chartKhoa"></canvas></div>
          </div>

          <div class="chart-card">
            <h3><i class="fa-solid fa-capsules"></i> Top 10 hoạt chất nghi ngờ nhiều nhất</h3>
            <div class="chart-wrap"><canvas id="chartHoatChat"></canvas></div>
          </div>
        </div>
      </div>
    </div>

  </div>

  <script>
    // ---------- CHUYỂN TAB ----------
    document.querySelectorAll('.tab-btn').forEach(function (btn) {
      btn.addEventListener('click', function () {
        document.querySelectorAll('.tab-btn').forEach(function (b) { b.classList.remove('active'); });
        document.querySelectorAll('.tab-content').forEach(function (c) { c.classList.remove('active'); });
        btn.classList.add('active');
        document.getElementById(btn.dataset.tab).classList.add('active');

        if (btn.dataset.tab === 'tab-list') {
          loadPatients();
        }
        if (btn.dataset.tab === 'tab-thongke') {
          loadThongKe();
        }
      });
    });

    // ---------- SUBMIT FORM (GHI VÀO SHEET "Copy") ----------
    document.getElementById('adrForm').addEventListener('submit', function (e) {
      e.preventDefault();

      var form = e.target;
      var submitBtn = document.getElementById('submitBtn');
      var msgBox = document.getElementById('formMsg');

      submitBtn.disabled = true;
      submitBtn.textContent = 'Đang lưu...';
      msgBox.style.display = 'none';

      // Tự gom dữ liệu form thành object thường (key-value),
      // KHÔNG dùng new FormData() trực tiếp vì google.script.run
      // đôi khi báo lỗi "illegal value in property: 0" khi serialize FormData.
      var formData = {};
      Array.prototype.forEach.call(form.elements, function (el) {
        if (el.name) {
          formData[el.name] = el.value;
        }
      });

      google.script.run
        .withSuccessHandler(function (res) {
          submitBtn.disabled = false;
          submitBtn.textContent = '💾 Lưu báo cáo';

          if (res.success) {
            msgBox.className = 'ok';
            msgBox.textContent = '✅ ' + res.message;
            form.reset();
          } else {
            msgBox.className = 'err';
            msgBox.textContent = '❌ ' + res.message;
          }
          msgBox.style.display = 'block';
        })
        .withFailureHandler(function (err) {
          submitBtn.disabled = false;
          submitBtn.textContent = '💾 Lưu báo cáo';
          msgBox.className = 'err';
          msgBox.textContent = '❌ Lỗi hệ thống: ' + err.message;
          msgBox.style.display = 'block';
        })
        .addPatient(formData);
    });

    // ---------- TẢI DANH SÁCH (ĐỌC TỪ SHEET "tong hop ADR") ----------
    var daTaiDuLieu = false;
    var TAT_CA_HEADERS = [];
    var TAT_CA_ITEMS = []; // mỗi phần tử: { row: [...], soDong: <số dòng thật trong Sheet> }

    // Các cột hiện ở phần TÓM TẮT (thu gọn), theo địa chỉ cột trên Google Sheet.
    // Muốn đổi cột tóm tắt, chỉ cần sửa 4 chữ cái này (không cần sửa gì khác).
    var COT_TOM_TAT = ['E', 'F', 'T', 'AF'];

    // Chuyển "A","B",..."Z","AA","AB"... thành số cột 0-based
    function chuCaiSangIndex(letter) {
      var result = 0;
      for (var i = 0; i < letter.length; i++) {
        result = result * 26 + (letter.charCodeAt(i) - 64);
      }
      return result - 1;
    }

    var INDEX_TOM_TAT = COT_TOM_TAT.map(chuCaiSangIndex);

    // Các cột ĐƯỢC PHÉP SỬA - lấy từ server (ChucNang_SuaBaoCao.gs) nên
    // Index.html KHÔNG BAO GIỜ cần sửa tay khi bạn thêm/bớt cột được sửa.
    var COT_DUOC_SUA = [];
    var INDEX_DUOC_SUA = [];

    function loadPatients() {
      if (daTaiDuLieu) return; // chỉ tải 1 lần khi vào tab, tránh gọi lặp
      daTaiDuLieu = true;

      document.getElementById('loadingMsg').style.display = 'block';
      document.getElementById('emptyMsg').style.display = 'none';
      document.getElementById('cardList').innerHTML = '';

      // Bước 1: lấy cấu hình cột được sửa, sau đó mới tải danh sách
      google.script.run
        .withSuccessHandler(function (cfg) {
          COT_DUOC_SUA = (cfg && cfg.success && cfg.cot) ? cfg.cot : [];
          INDEX_DUOC_SUA = COT_DUOC_SUA.map(chuCaiSangIndex);
          taiDanhSachThat();
        })
        .withFailureHandler(function () {
          // Nếu lỗi lấy cấu hình sửa, vẫn tải danh sách bình thường (chỉ là không sửa được)
          COT_DUOC_SUA = [];
          INDEX_DUOC_SUA = [];
          taiDanhSachThat();
        })
        .layDanhSachCotDuocSua();
    }

    function taiDanhSachThat() {
      google.script.run
        .withSuccessHandler(function (res) {
          document.getElementById('loadingMsg').style.display = 'none';

          if (!res.success) {
            document.getElementById('loadingMsg').style.display = 'block';
            document.getElementById('loadingMsg').textContent = '❌ ' + res.message;
            return;
          }

          TAT_CA_HEADERS = res.headers || [];
          var rows = res.rows || [];
          var soDongs = res.soDongTrongSheet || [];
          TAT_CA_ITEMS = rows.map(function (row, i) {
            return { row: row, soDong: soDongs[i] };
          });

          if (TAT_CA_ITEMS.length === 0) {
            document.getElementById('emptyMsg').style.display = 'block';
            return;
          }

          ve_danh_sach(TAT_CA_ITEMS);
        })
        .withFailureHandler(function (err) {
          document.getElementById('loadingMsg').textContent = '❌ Lỗi tải dữ liệu: ' + err.message;
        })
        .getPatients();
    }

    // Bỏ dấu tiếng Việt để tìm kiếm không phân biệt dấu
    function boDauTV(str) {
      return String(str)
        .normalize('NFD')
        .replace(/[\u0300-\u036f]/g, '')
        .replace(/đ/g, 'd').replace(/Đ/g, 'D')
        .toLowerCase();
    }

    function ve_danh_sach(items) {
      var cardList = document.getElementById('cardList');
      cardList.innerHTML = '';

      document.getElementById('resultCount').textContent =
        'Hiển thị ' + items.length + ' / ' + TAT_CA_ITEMS.length + ' báo cáo';

      if (items.length === 0) {
        cardList.innerHTML = '<div style="padding:20px; text-align:center; color:#9ca3af;">Không tìm thấy báo cáo phù hợp.</div>';
        return;
      }

      items.forEach(function (item) {
        var row = item.row;
        var soDong = item.soDong;

        var card = document.createElement('div');
        card.className = 'adr-card';

        // ----- Phần tóm tắt (luôn hiện) -----
        var summary = document.createElement('div');
        summary.className = 'adr-summary';

        var chevron = document.createElement('span');
        chevron.className = 'adr-chevron';
        chevron.textContent = '▶';
        summary.appendChild(chevron);

        INDEX_TOM_TAT.forEach(function (colIndex) {
          var wrap = document.createElement('div');
          var label = document.createElement('div');
          label.className = 'adr-field-label';
          label.textContent = TAT_CA_HEADERS[colIndex] || '';
          var value = document.createElement('div');
          value.className = 'adr-field-value';
          var raw = row[colIndex];
          value.textContent = (raw === null || raw === undefined || raw === '') ? '—' : raw;
          wrap.appendChild(label);
          wrap.appendChild(value);
          summary.appendChild(wrap);
        });

        summary.addEventListener('click', function () {
          card.classList.toggle('open');
          chevron.textContent = card.classList.contains('open') ? '▼' : '▶';
        });

        // ----- Phần chi tiết (ẩn, xổ ra khi bấm) -----
        var detail = document.createElement('div');
        detail.className = 'adr-detail';
        var grid = document.createElement('div');
        grid.className = 'adr-detail-grid';

        // Lưu các ô có thể sửa để dễ thao tác khi bấm nút Sửa/Lưu/Hủy
        var oCoTheSua = []; // { colIndex, chuCai, item(div), valueDiv, giaTriGoc }

        row.forEach(function (cell, colIndex) {
          if (INDEX_TOM_TAT.indexOf(colIndex) !== -1) return; // đã hiện ở tóm tắt

          var viTriTrongCotSua = INDEX_DUOC_SUA.indexOf(colIndex);
          var laCotDuocSua = viTriTrongCotSua !== -1;

          // Cột thường: bỏ qua nếu trống. Cột được sửa: LUÔN hiện dù đang trống,
          // để người dùng có thể điền lần đầu.
          if (!laCotDuocSua && (cell === null || cell === undefined || cell === '')) return;

          var itemEl = document.createElement('div');
          itemEl.className = 'adr-detail-item' + (laCotDuocSua ? ' co-the-sua' : '');

          var lbl = document.createElement('div');
          lbl.className = 'adr-field-label';
          lbl.textContent = TAT_CA_HEADERS[colIndex] || ('Cột ' + (colIndex + 1));

          var val = document.createElement('div');
          val.className = 'adr-field-value';
          val.textContent = (cell === null || cell === undefined || cell === '') ? '—' : cell;

          itemEl.appendChild(lbl);
          itemEl.appendChild(val);
          grid.appendChild(itemEl);

          if (laCotDuocSua) {
            oCoTheSua.push({
              colIndex: colIndex,
              chuCai: COT_DUOC_SUA[viTriTrongCotSua],
              valueDiv: val,
              giaTriGoc: (cell === null || cell === undefined) ? '' : cell
            });
          }
        });

        detail.appendChild(grid);

        // ----- Thanh nút Sửa / Lưu / Hủy (chỉ hiện nếu có ít nhất 1 cột được sửa) -----
        if (oCoTheSua.length > 0) {
          var toolbar = document.createElement('div');
          toolbar.className = 'adr-edit-toolbar';

          var btnSua = document.createElement('button');
          btnSua.className = 'adr-btn-sm adr-btn-edit';
          btnSua.innerHTML = '<i class="fa-solid fa-pen"></i> Sửa';

          var btnLuu = document.createElement('button');
          btnLuu.className = 'adr-btn-sm adr-btn-save';
          btnLuu.innerHTML = '<i class="fa-solid fa-check"></i> Lưu';
          btnLuu.style.display = 'none';

          var btnHuy = document.createElement('button');
          btnHuy.className = 'adr-btn-sm adr-btn-cancel';
          btnHuy.innerHTML = 'Hủy';
          btnHuy.style.display = 'none';

          var msgEl = document.createElement('span');
          msgEl.className = 'adr-save-msg';

          toolbar.appendChild(btnSua);
          toolbar.appendChild(btnLuu);
          toolbar.appendChild(btnHuy);
          toolbar.appendChild(msgEl);
          detail.appendChild(toolbar);

          btnSua.addEventListener('click', function (e) {
            e.stopPropagation();
            oCoTheSua.forEach(function (o) {
              var input = document.createElement('input');
              input.type = 'text';
              input.className = 'adr-edit-input';
              input.value = o.giaTriGoc;
              o.valueDiv.innerHTML = '';
              o.valueDiv.appendChild(input);
              o.inputEl = input;
            });
            btnSua.style.display = 'none';
            btnLuu.style.display = '';
            btnHuy.style.display = '';
            msgEl.textContent = '';
          });

          btnHuy.addEventListener('click', function (e) {
            e.stopPropagation();
            oCoTheSua.forEach(function (o) {
              o.valueDiv.textContent = (o.giaTriGoc === '') ? '—' : o.giaTriGoc;
            });
            btnSua.style.display = '';
            btnLuu.style.display = 'none';
            btnHuy.style.display = 'none';
            msgEl.textContent = '';
          });

          btnLuu.addEventListener('click', function (e) {
            e.stopPropagation();
            btnLuu.disabled = true;
            btnHuy.disabled = true;
            msgEl.className = 'adr-save-msg';
            msgEl.textContent = 'Đang lưu...';

            var capNhat = {};
            oCoTheSua.forEach(function (o) {
              capNhat[o.chuCai] = o.inputEl ? o.inputEl.value : o.giaTriGoc;
            });

            google.script.run
              .withSuccessHandler(function (res) {
                btnLuu.disabled = false;
                btnHuy.disabled = false;

                if (!res.success) {
                  msgEl.className = 'adr-save-msg err';
                  msgEl.textContent = '❌ ' + res.message;
                  return;
                }

                // Cập nhật lại dữ liệu trong bộ nhớ + hiển thị, để tìm kiếm vẫn đúng
                oCoTheSua.forEach(function (o) {
                  var giaTriMoi = o.inputEl ? o.inputEl.value : o.giaTriGoc;
                  o.giaTriGoc = giaTriMoi;
                  row[o.colIndex] = giaTriMoi;
                  o.valueDiv.textContent = (giaTriMoi === '') ? '—' : giaTriMoi;
                });

                btnSua.style.display = '';
                btnLuu.style.display = 'none';
                btnHuy.style.display = 'none';
                msgEl.className = 'adr-save-msg ok';
                msgEl.textContent = '✔ Đã lưu';
                setTimeout(function () { msgEl.textContent = ''; }, 2500);
              })
              .withFailureHandler(function (err) {
                btnLuu.disabled = false;
                btnHuy.disabled = false;
                msgEl.className = 'adr-save-msg err';
                msgEl.textContent = '❌ Lỗi hệ thống: ' + err.message;
              })
              .suaBaoCao(soDong, capNhat);
          });
        }

        card.appendChild(summary);
        card.appendChild(detail);
        cardList.appendChild(card);
      });
    }

    // ---------- TÌM KIẾM ----------
    document.getElementById('searchBox').addEventListener('input', function (e) {
      var tuKhoa = boDauTV(e.target.value.trim());

      if (!tuKhoa) {
        ve_danh_sach(TAT_CA_ITEMS);
        return;
      }

      var ketQua = TAT_CA_ITEMS.filter(function (item) {
        return item.row.some(function (cell) {
          if (cell === null || cell === undefined) return false;
          return boDauTV(cell).indexOf(tuKhoa) !== -1;
        });
      });

      ve_danh_sach(ketQua);
    });

    // Nút refresh thủ công (gọi lại nếu người dùng muốn F5 dữ liệu)
    function refreshPatients() {
      daTaiDuLieu = false;
      loadPatients();
    }

    // ================================================================
    // ---------- TAB 3: THỐNG KÊ (gọi thongKeADR() + vẽ Chart.js) ----------
    // ================================================================
    var daTaiThongKe = false;
    var BANG_MAU_DO = ['#dc2626', '#ef4444', '#f87171', '#fca5a5', '#b91c1c', '#991b1b', '#fecaca', '#7f1d1d', '#f43f5e', '#e11d48'];

    Chart.defaults.font.family = "'Inter', 'Segoe UI', Arial, sans-serif";
    Chart.defaults.color = '#6b7280';

    function loadThongKe() {
      if (daTaiThongKe) return; // chỉ tải 1 lần khi vào tab
      daTaiThongKe = true;

      document.getElementById('tkLoading').style.display = 'block';
      document.getElementById('tkError').style.display = 'none';
      document.getElementById('tkContent').style.display = 'none';

      google.script.run
        .withSuccessHandler(function (res) {
          document.getElementById('tkLoading').style.display = 'none';

          if (!res.success) {
            var box = document.getElementById('tkError');
            box.style.display = 'block';
            box.textContent = '❌ ' + res.message;
            return;
          }

          document.getElementById('tkContent').style.display = 'block';
          ve_thong_ke(res);
        })
        .withFailureHandler(function (err) {
          document.getElementById('tkLoading').style.display = 'none';
          var box = document.getElementById('tkError');
          box.style.display = 'block';
          box.textContent = '❌ Lỗi hệ thống: ' + err.message;
        })
        .thongKeADR();
    }

    function ve_thong_ke(res) {
      document.getElementById('statTongSo').textContent = res.tongSoBaoCao;
      document.getElementById('statSoKhoa').textContent = res.theoKhoa.labels.length;
      document.getElementById('statSoHoatChat').textContent = res.theoHoatChat.labels.length;
      document.getElementById('statSoThang').textContent = res.theoThang.labels.length;

      // ---- 1. Xu hướng theo tháng (line chart) ----
      new Chart(document.getElementById('chartThang'), {
        type: 'line',
        data: {
          labels: res.theoThang.labels,
          datasets: [{
            label: 'Số ca ADR',
            data: res.theoThang.values,
            borderColor: '#dc2626',
            backgroundColor: 'rgba(220,38,38,0.10)',
            borderWidth: 2.5,
            pointBackgroundColor: '#dc2626',
            pointRadius: 4,
            pointHoverRadius: 6,
            tension: 0.35,
            fill: true
          }]
        },
        options: {
          responsive: true,
          maintainAspectRatio: false,
          plugins: { legend: { display: false } },
          scales: {
            y: { beginAtZero: true, ticks: { precision: 0 }, grid: { color: '#f1e8e8' } },
            x: { grid: { display: false } }
          }
        }
      });

      // ---- 2. Theo mức độ nghiêm trọng (doughnut) ----
      new Chart(document.getElementById('chartMucDo'), {
        type: 'doughnut',
        data: {
          labels: res.theoMucDo.labels,
          datasets: [{ data: res.theoMucDo.values, backgroundColor: BANG_MAU_DO, borderWidth: 2, borderColor: '#fff' }]
        },
        options: {
          responsive: true,
          maintainAspectRatio: false,
          cutout: '62%',
          plugins: { legend: { position: 'bottom', labels: { boxWidth: 12, font: { size: 11.5 } } } }
        }
      });

      // ---- 3. Theo giới tính (doughnut) ----
      new Chart(document.getElementById('chartGioiTinh'), {
        type: 'doughnut',
        data: {
          labels: res.theoGioiTinh.labels,
          datasets: [{ data: res.theoGioiTinh.values, backgroundColor: ['#dc2626', '#fca5a5', '#f43f5e'], borderWidth: 2, borderColor: '#fff' }]
        },
        options: {
          responsive: true,
          maintainAspectRatio: false,
          cutout: '62%',
          plugins: { legend: { position: 'bottom', labels: { boxWidth: 12, font: { size: 11.5 } } } }
        }
      });

      // ---- 4. Top khoa (horizontal bar) ----
      new Chart(document.getElementById('chartKhoa'), {
        type: 'bar',
        data: {
          labels: res.theoKhoa.labels,
          datasets: [{ data: res.theoKhoa.values, backgroundColor: '#dc2626', borderRadius: 6, maxBarThickness: 22 }]
        },
        options: {
          indexAxis: 'y',
          responsive: true,
          maintainAspectRatio: false,
          plugins: { legend: { display: false } },
          scales: {
            x: { beginAtZero: true, ticks: { precision: 0 }, grid: { color: '#f1e8e8' } },
            y: { grid: { display: false } }
          }
        }
      });

      // ---- 5. Top hoạt chất (horizontal bar) ----
      new Chart(document.getElementById('chartHoatChat'), {
        type: 'bar',
        data: {
          labels: res.theoHoatChat.labels,
          datasets: [{ data: res.theoHoatChat.values, backgroundColor: '#b91c1c', borderRadius: 6, maxBarThickness: 22 }]
        },
        options: {
          indexAxis: 'y',
          responsive: true,
          maintainAspectRatio: false,
          plugins: { legend: { display: false } },
          scales: {
            x: { beginAtZero: true, ticks: { precision: 0 }, grid: { color: '#f1e8e8' } },
            y: { grid: { display: false } }
          }
        }
      });
    }
  </script>

</body>
</html>
