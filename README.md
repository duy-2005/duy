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

    /* ============ DANH SÁCH DẠNG THẺ ============ */
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

    .adr-card.open { box-shadow: inset 3px 0 0 var(--red-600); }
    .adr-card.open .adr-summary { background: var(--red-50); }
    .adr-card.open .adr-summary:hover { background: var(--red-100); }

    .adr-chevron { transition: transform 0.15s; color: var(--muted); font-size: 12px; }
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

    .adr-detail { display: none; background: #f6f3f3; border-top: 1px dashed var(--red-200); padding: 18px 20px; }
    .adr-card.open .adr-detail { display: block; }

    .adr-detail-section-box {
      background: #fff;
      border: 1px solid var(--line);
      border-radius: 12px;
      padding: 16px 18px;
      margin-bottom: 14px;
      box-shadow: 0 1px 2px rgba(185,28,28,0.04);
    }
    .adr-detail-section-box:last-child { margin-bottom: 0; }
    .adr-section-title {
      font-size: 12px;
      font-weight: 700;
      color: var(--red-600);
      text-transform: uppercase;
      letter-spacing: 0.5px;
      margin-bottom: 12px;
      padding-bottom: 8px;
      border-bottom: 1px solid var(--red-100);
      display: flex;
      align-items: center;
      gap: 8px;
    }
    .adr-section-title i { font-size: 12px; color: var(--red-500); }

    .adr-detail-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(240px, 1fr)); gap: 12px 22px; }
    .adr-detail-item { font-size: 13px; border-bottom: 1px dotted var(--line); padding-bottom: 7px; }
    .adr-detail-item .adr-field-label { margin-bottom: 3px; }
    .adr-detail-item .adr-field-value { white-space: normal; word-break: break-word; font-weight: 400; }

    /* ============ LAYOUT 2 CỘT: DANH SÁCH TRÁI / CHI TIẾT PHẢI ============ */
    .master-detail-layout {
      display: grid;
      grid-template-columns: 380px 1fr;
      gap: 18px;
      align-items: start;
    }
    .master-detail-left { min-width: 0; }
    .master-detail-right { min-width: 0; }

    #cardList {
      max-height: calc(100vh - 260px);
      overflow-y: auto;
      margin: 4px -8px -8px;
      padding: 0 8px 8px;
    }

    /* ---- Dòng danh sách gọn bên trái ---- */
    .adr-list-row {
      border: 1px solid var(--line);
      border-left: 4px solid transparent;
      border-radius: 10px;
      padding: 12px 14px;
      margin-bottom: 8px;
      cursor: pointer;
      background: #fff;
      transition: background 0.12s, border-color 0.12s, box-shadow 0.12s;
    }
    .adr-list-row:hover { background: var(--red-50); }
    .adr-list-row.active {
      background: var(--red-50);
      border-left-color: var(--red-600);
      border-color: var(--red-200);
      box-shadow: 0 2px 10px rgba(220,38,38,0.12);
    }
    .adr-list-row .row-top {
      display: flex;
      justify-content: space-between;
      align-items: baseline;
      gap: 10px;
      margin-bottom: 4px;
    }
    .adr-list-row .row-ten {
      font-size: 14.5px;
      font-weight: 700;
      color: var(--ink);
      overflow: hidden;
      text-overflow: ellipsis;
      white-space: nowrap;
    }
    .adr-list-row.active .row-ten { color: var(--red-700); }
    .adr-list-row .row-thoigian {
      font-size: 11px;
      color: var(--muted);
      flex-shrink: 0;
      font-weight: 500;
    }
    .adr-list-row .row-nb {
      font-size: 12px;
      color: var(--ink-soft);
      margin-bottom: 4px;
      overflow: hidden;
      text-overflow: ellipsis;
      white-space: nowrap;
    }
    .adr-list-row .row-mota {
      font-size: 12.5px;
      color: #6b7280;
      display: -webkit-box;
      -webkit-line-clamp: 2;
      -webkit-box-orient: vertical;
      overflow: hidden;
    }

    /* ---- Khung chi tiết bên phải ---- */
    .detail-panel-card {
      position: sticky;
      top: 132px;
      max-height: calc(100vh - 160px);
      overflow-y: auto;
    }
    .detail-panel-empty {
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      gap: 12px;
      padding: 70px 20px;
      color: var(--muted);
      font-size: 14px;
      font-weight: 500;
      text-align: center;
    }
    .detail-panel-empty i { font-size: 34px; color: var(--red-200); }

    .detail-panel-header {
      display: flex;
      justify-content: space-between;
      align-items: flex-start;
      gap: 14px;
      padding-bottom: 16px;
      margin-bottom: 16px;
      border-bottom: 2px solid var(--red-100);
    }
    .detail-panel-header h2 {
      margin: 0 0 4px;
      font-size: 18px;
      font-weight: 800;
      color: var(--ink);
    }
    .detail-panel-header .sub {
      font-size: 12.5px;
      color: var(--muted);
      font-weight: 500;
    }

    @media (max-width: 980px) {
      .master-detail-layout { grid-template-columns: 1fr; }
      .detail-panel-card { position: static; max-height: none; }
      #cardList { max-height: none; }
    }

    .adr-detail-item.co-the-sua .adr-field-label { color: var(--red-600); }
    .adr-detail-item.co-the-sua .adr-field-label::after { content: " ✎"; }
    .adr-edit-input { width: 100%; padding: 6px 9px; border: 1.5px solid var(--red-400); border-radius: 7px; font-size: 13px; font-family: inherit; background: #fff; }
    .adr-edit-input:focus { outline: none; box-shadow: 0 0 0 3px rgba(239,68,68,0.15); }

    .adr-edit-toolbar { margin-top: 14px; padding-top: 14px; border-top: 1px solid var(--line); display: flex; gap: 10px; flex-wrap: wrap; }
    .adr-btn-sm { display: inline-flex; align-items: center; gap: 7px; padding: 8px 16px; border-radius: 8px; font-size: 13px; font-weight: 600; font-family: inherit; cursor: pointer; border: none; transition: transform 0.1s, box-shadow 0.1s; }
    .adr-btn-sm:hover { transform: translateY(-1px); }
    .adr-btn-edit { background: var(--red-50); color: var(--red-600); border: 1.5px solid var(--red-200); }
    .adr-btn-edit:hover { background: var(--red-100); }
    .adr-btn-print { background: #eff6ff; color: #1d4ed8; border: 1.5px solid #bfdbfe; }
    .adr-btn-print:hover { background: #dbeafe; }
    .adr-btn-save { background: linear-gradient(135deg, var(--red-600), var(--red-500)); color: #fff; box-shadow: 0 4px 12px rgba(220,38,38,0.25); }
    .adr-btn-cancel { background: #f1f2f4; color: var(--ink-soft); }
    .adr-btn-sm:disabled { opacity: 0.6; cursor: not-allowed; transform: none; }

    .adr-save-msg { font-size: 12.5px; font-weight: 600; margin-left: 4px; align-self: center; }
    .adr-save-msg.ok { color: #15803d; }
    .adr-save-msg.err { color: var(--red-700); }

    /* ============ MÀN HÌNH ĐĂNG NHẬP ============ */
    #login-screen {
      position: fixed; top: 0; left: 0; width: 100%; height: 100vh;
      background-color: var(--bg);
      display: flex; justify-content: center; align-items: center;
      z-index: 9999;
    }
    .login-box {
      background: var(--card-bg); padding: 36px 30px;
      border-radius: var(--radius); box-shadow: var(--shadow-lg);
      width: 100%; max-width: 400px; text-align: center;
      border: 1px solid var(--line);
    }
    .login-box i.fa-shield-halved { font-size: 46px; color: var(--red-600); margin-bottom: 16px; }
    .login-box h2 { color: var(--ink); margin-top: 0; margin-bottom: 24px; font-weight: 800; font-size: 22px; }
    .login-box label { display: block; text-align: left; margin-bottom: 6px; font-size: 13.5px; font-weight: 600; color: var(--ink-soft); }
    .login-box select, .login-box input { 
      width: 100%; padding: 10px 13px; border: 1.5px solid #e2dede; 
      border-radius: 9px; font-size: 14px; font-family: inherit; margin-bottom: 20px; 
      transition: border-color 0.15s, box-shadow 0.15s; 
    }
    .login-box select:focus, .login-box input:focus { 
      outline: none; border-color: var(--red-500); box-shadow: 0 0 0 4px rgba(239,68,68,0.14); 
    }
    .login-box .btn { width: 100%; justify-content: center; margin-top: 5px; }

    /* ============ RESPONSIVE ============ */
    @media (max-width: 780px) {
      .container { padding: 16px 12px 50px; }
      header { padding: 16px 18px; }
      header h1 { font-size: 17px; }
      .tabs { top: 68px; padding: 0 8px; }
      .tab-btn { padding: 13px 14px; font-size: 13.5px; }
      .card { padding: 16px; }
      .list-headbar { display: none; }
      .adr-summary { grid-template-columns: 18px 1fr; grid-auto-rows: auto; row-gap: 8px; }
      .adr-detail-grid { grid-template-columns: 1fr; }
    }

    /* ============ TAB KHO BÁO CÁO & SWIPE TO REVEAL ============ */
    .thang-box { 
      margin-bottom: 12px; 
      border: 1px solid var(--line); 
      border-radius: 12px; 
      background: #fff; 
      box-shadow: var(--shadow-sm); 
    }
    .thang-box summary { 
      background-color: var(--red-50); 
      padding: 14px 16px; 
      font-weight: 700; 
      cursor: pointer; 
      color: var(--red-700); 
      font-size: 14px; 
      display: flex; 
      align-items: center; 
      gap: 10px; 
      border-radius: 12px; 
      transition: background 0.15s; 
    }
    .thang-box[open] summary { 
      border-bottom-left-radius: 0; 
      border-bottom-right-radius: 0; 
      border-bottom: 1px dashed var(--red-200); 
    }
    .thang-box summary:hover { background-color: var(--red-100); }
    
    .swipe-wrapper {
      overflow-x: auto;
      scroll-snap-type: x mandatory;
      scrollbar-width: none; /* Firefox */
      -ms-overflow-style: none; /* IE */
      border-bottom: 1px solid var(--line);
    }
    .swipe-wrapper::-webkit-scrollbar { display: none; /* Chrome/Safari */ }
    
    .swipe-track {
      display: flex;
      width: max-content;
      min-width: 100%;
    }
    
    .file-item { 
      display: flex; 
      justify-content: space-between; 
      align-items: center; 
      padding: 12px 16px; 
      transition: background 0.12s; 
      flex: 1 0 100%; 
      scroll-snap-align: start;
    }
    .file-item:hover { background: var(--bg); }
    .file-item.active { background: var(--red-50); border-left: 3px solid var(--red-600); padding-left: 13px; }
    
    .file-name { 
      color: var(--ink); 
      cursor: pointer; 
      font-size: 13.5px; 
      font-weight: 600; 
      flex: 1; 
      display: flex; 
      align-items: center; 
      gap: 10px; 
      overflow: hidden; 
      text-overflow: ellipsis; 
      white-space: nowrap; 
    }
    .file-name i { color: var(--muted); font-size: 16px; }
    .file-item.active .file-name { color: var(--red-700); }
    .file-item.active .file-name i { color: var(--red-500); }
    
    .swipe-actions {
      display: flex;
      align-items: center;
      gap: 8px;
      padding: 0 16px 0 8px;
      scroll-snap-align: end;
      background: #fff; 
    }
    
    .btn-gui-mail { 
      background: #ecfdf3; 
      color: #15803d; 
      border: 1.5px solid #bbf7d0; 
      padding: 6px 12px; 
      border-radius: 8px; 
      cursor: pointer; 
      font-size: 12px; 
      font-weight: 700; 
      font-family: inherit;
      transition: transform 0.1s; 
      display: inline-flex; 
      align-items: center; 
      gap: 6px; 
    }
    .btn-gui-mail:hover { background: #dcfce7; transform: translateY(-1px); }
    .btn-gui-mail:disabled { opacity: 0.6; cursor: not-allowed; transform: none; }

    .btn-xoa-file {
      background: #fef2f2;
      color: #dc2626;
      border: 1.5px solid #fecaca;
      padding: 6px 12px;
      border-radius: 8px;
      cursor: pointer;
      font-size: 12px;
      font-weight: 700;
      font-family: inherit;
      transition: transform 0.1s, background 0.1s;
      display: inline-flex;
      align-items: center;
      gap: 6px;
    }
    .btn-xoa-file:hover { background: #fee2e2; transform: translateY(-1px); }
    .btn-xoa-file:disabled { opacity: 0.6; cursor: not-allowed; transform: none; }
  </style>
</head>
<body>

  <!-- 1. MÀN HÌNH ĐĂNG NHẬP SẼ HIỆN RA ĐẦU TIÊN -->
  <div id="login-screen">
    <div class="login-box">
      <i class="fa-solid fa-shield-halved"></i>
      <h2>ĐĂNG NHẬP HỆ THỐNG</h2>
      
      <label>Chọn Khoa/Phòng:</label>
      <select id="chonKhoa">
        <option value="">-- Đang tải danh sách khoa --</option>
      </select>

      <label>Mật khẩu Khoa/Phòng:</label>
      <input type="password" id="matKhau" placeholder="Nhập mật khẩu...">

      <button class="btn" onclick="xuLyDangNhap()">
        <i class="fa-solid fa-right-to-bracket"></i> Đăng nhập
      </button>
      <div id="login-error" style="color: var(--red-600); margin-top: 15px; font-size: 13.5px; font-weight: 600; display: none;"></div>
    </div>
  </div>

  <!-- 2. PHẦN MỀM GỐC BỊ ẨN ĐI (Sẽ mở ra khi đăng nhập đúng) -->
  <div id="main-app" style="display: none;">

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
      <button class="tab-btn" data-tab="tab-thongke" id="btnTabThongKe"><i class="fa-solid fa-chart-pie"></i> Thống kê</button>
      <!-- NÚT MỚI: KHO BÁO CÁO -->
      <button class="tab-btn" data-tab="tab-khobaocao"><i class="fa-regular fa-folder-open"></i> Kho báo cáo</button>
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
              <legend><i class="fa-solid fa-plus"></i> Thông cải bổ sung (nếu cần)</legend>
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
        <div class="master-detail-layout">

          <div class="master-detail-left">
            <div class="card" style="padding: 18px;">
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

              <div id="cardList"></div>
            </div>
          </div>

          <div class="master-detail-right">
            <div class="card detail-panel-card" id="detailPanel">
              <div class="detail-panel-empty">
                <i class="fa-solid fa-hand-pointer"></i>
                <div>Chọn 1 báo cáo bên trái để xem chi tiết</div>
              </div>
            </div>
          </div>

        </div>
      </div>

      <!-- ===================== TAB 3: THỐNG KÊ ===================== -->
      <div id="tab-thongke" class="tab-content">
        
        <!-- THÔNG BÁO TỪ CHỐI TRUY CẬP -->
        <div id="tkTuChoi" style="display:none; text-align:center; padding: 80px 20px; color: var(--ink-soft);">
           <i class="fa-solid fa-lock" style="font-size: 50px; color: var(--red-200); margin-bottom: 16px; display:block;"></i>
           <h2 style="margin: 0 0 8px; color: var(--ink);">Không có quyền truy cập</h2>
           <p>Tính năng Thống kê Biểu đồ chỉ dành cho bộ phận được phân quyền quản lý.</p>
        </div>

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

      <!-- ===================== TAB 4: KHO BÁO CÁO (KÉO SANG ĐỂ XÓA/MAIL) ===================== -->
      <div id="tab-khobaocao" class="tab-content">
        <div class="master-detail-layout">
          
          <!-- Cột trái: Danh sách file nhóm theo tháng -->
          <div class="master-detail-left">
            <div class="card" style="padding: 18px;">
              <div style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 16px; padding-bottom: 12px; border-bottom: 2px solid var(--red-100);">
                <h3 style="margin: 0; font-size: 15px; font-weight: 800; color: var(--ink);">
                  <i class="fa-brands fa-google-drive" style="color: var(--red-500); margin-right: 6px;"></i> Dữ liệu Drive
                </h3>
                <button class="adr-btn-sm adr-btn-print" onclick="refreshKhoBaoCao()" style="padding: 6px 12px;">
                  <i class="fa-solid fa-rotate-right"></i> Làm mới
                </button>
              </div>

              <!-- Trạng thái Loading -->
              <div id="khoLoadingMsg" style="display: none;">
                <div class="spinner-wrap">
                  <div class="spinner"></div>
                  <div>Đang tải file từ Drive...</div>
                </div>
              </div>

              <!-- Trạng thái Trống -->
              <div id="khoEmptyMsg" class="detail-panel-empty" style="display: none; padding: 40px 10px;">
                <i class="fa-regular fa-folder-closed"></i>
                <div>Thư mục hiện tại chưa có báo cáo nào.</div>
              </div>

              <!-- Nơi đổ dữ liệu tháng -->
              <div id="vungDanhSachKho" style="max-height: calc(100vh - 270px); overflow-y: auto; padding-right: 4px;">
              </div>
            </div>
          </div>

          <!-- Cột phải: Iframe hiển thị PDF -->
          <div class="master-detail-right">
            <div class="card detail-panel-card" style="padding: 0; overflow: hidden; height: calc(100vh - 160px); display: flex; flex-direction: column;">
              
              <div id="khoChuaChon" class="detail-panel-empty" style="height: 100%;">
                <i class="fa-regular fa-file-pdf"></i>
                <div>Chọn 1 báo cáo bên trái để xem bản in<br><span style="font-size: 12px; margin-top:8px; display:inline-block;">(Vuốt/Kéo báo cáo sang trái để Gửi mail hoặc Xóa)</span></div>
              </div>
              
              <iframe id="khoPdfViewer" width="100%" height="100%" style="border: none; display: none;" src=""></iframe>
            
            </div>
          </div>

        </div>
      </div>

    </div> <!-- Kết thúc thẻ div container -->

  </div> <!-- Kết thúc thẻ div main-app -->

  <script>
    // ================================================================
    // BIẾN TOÀN CỤC BẢO MẬT PHÂN QUYỀN
    // ================================================================
    var KHOA_DANG_NHAP = "";
    var MAT_KHAU_DANG_NHAP = ""; // Lưu tạm để gửi lên máy chủ khi thống kê
    var QUYEN_XEM_THONG_KE = false;

    // ================================================================
    // ---------- XỬ LÝ ĐĂNG NHẬP KHOA PHÒNG ----------
    // ================================================================
    document.addEventListener("DOMContentLoaded", function() {
      // Gọi lên Sheet để lấy danh sách khoa
      google.script.run
        .withSuccessHandler(function(danhSach) {
           var select = document.getElementById("chonKhoa");
           select.innerHTML = '<option value="">-- Vui lòng chọn Khoa/Phòng --</option>';
           
           danhSach.forEach(function(khoa) {
             var option = document.createElement("option");
             option.value = khoa;
             option.text = khoa;
             select.appendChild(option);
           });
        })
        .getDanhSachKhoa();
    });

    function xuLyDangNhap() {
      var khoa = document.getElementById("chonKhoa").value;
      var pass = document.getElementById("matKhau").value;
      var errorText = document.getElementById("login-error");
      var btn = document.querySelector(".login-box .btn");

      if (!khoa || !pass) {
        errorText.innerText = "Vui lòng chọn khoa và nhập mật khẩu!";
        errorText.style.display = "block";
        return;
      }

      // Đổi trạng thái nút bấm thành Loading
      btn.innerHTML = '<i class="fa-solid fa-spinner fa-spin"></i> Đang kiểm tra...';
      btn.disabled = true;

      // Gửi xuống Backend kiểm tra (Đã sửa nhận đối tượng thay vì boolean)
      google.script.run
        .withSuccessHandler(function(ketQua) {
          
          if (ketQua.hopLe === true) {
            // LƯU LẠI THÔNG TIN VÀ QUYỀN VÀO BIẾN TOÀN CỤC
            KHOA_DANG_NHAP = khoa;
            MAT_KHAU_DANG_NHAP = pass;
            QUYEN_XEM_THONG_KE = ketQua.duocXemBieuDo;

            // XỬ LÝ ẨN/HIỆN NÚT TAB THỐNG KÊ TRÊN GIAO DIỆN
            var btnThongKe = document.getElementById("btnTabThongKe");
            if (QUYEN_XEM_THONG_KE) {
              btnThongKe.style.display = 'flex'; // Cho phép thấy nút thống kê
            } else {
              btnThongKe.style.display = 'none'; // Giấu hẳn nút thống kê đi
            }

            // ĐĂNG NHẬP ĐÚNG: Tắt màn hình chờ, Mở phần mềm
            document.getElementById("login-screen").style.display = "none";
            document.getElementById("main-app").style.display = "block";
            
            // Tự động điền tên khoa vào Form nhập liệu của bạn
            var khoaInput = document.querySelector('input[name="khoa_phong"]');
            if (khoaInput) {
                khoaInput.value = khoa;
                khoaInput.readOnly = true; // Khóa ô này lại không cho sửa để tránh gian lận
            }
          } else {
            // ĐĂNG NHẬP SAI: Báo lỗi và mở lại nút bấm
            errorText.innerText = "Sai mật khẩu Khoa/Phòng!";
            errorText.style.display = "block";
            btn.innerHTML = '<i class="fa-solid fa-right-to-bracket"></i> Đăng nhập';
            btn.disabled = false;
          }
        })
        .kiemTraMatKhau(khoa, pass);
    }

    // ---------- CHUYỂN TAB ----------
    document.querySelectorAll('.tab-btn').forEach(function (btn) {
      btn.addEventListener('click', function () {
        document.querySelectorAll('.tab-btn').forEach(function (b) { b.classList.remove('active'); });
        document.querySelectorAll('.tab-content').forEach(function (c) { c.classList.remove('active'); });
        btn.classList.add('active');
        document.getElementById(btn.dataset.tab).classList.add('active');

        if (btn.dataset.tab === 'tab-list') { loadPatients(); }
        if (btn.dataset.tab === 'tab-thongke') { loadThongKe(); }
        if (btn.dataset.tab === 'tab-khobaocao') { loadKhoBaoCao(); }
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
            // Khôi phục lại tên khoa phòng sau khi reset form
            var khoaInput = document.querySelector('input[name="khoa_phong"]');
            var khoaHienTai = document.getElementById("chonKhoa").value;
            if(khoaInput) khoaInput.value = khoaHienTai;
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

    function chuCaiSangIndex(letter) {
      var result = 0;
      for (var i = 0; i < letter.length; i++) {
        result = result * 26 + (letter.charCodeAt(i) - 64);
      }
      return result - 1;
    }

    var INDEX_TOM_TAT = COT_TOM_TAT.map(chuCaiSangIndex);

    // ============================================================
    // 👉 NHÓM HIỂN THỊ Ở PHẦN CHI TIẾT (chỉ để sắp xếp cho dễ nhìn,
    //    không ảnh hưởng dữ liệu). Muốn thêm/bớt/đổi cột trong 1 nhóm,
    //    chỉ cần sửa đúng TÊN TIÊU ĐỀ trong mảng "cot" tương ứng.
    //    Cột nào không nằm trong nhóm nào sẽ tự rơi vào nhóm "Khác".
    // ============================================================
    var CAC_NHOM_CHI_TIET = [
      {
        ten: 'Thông tin báo cáo', icon: 'fa-clipboard-list',
        cot: ['SỐ BÁO CÁO ADR', 'SỐ THUỐC NGHI NGỜ ADR', 'Note ca ADR', 'MÃ HỒ SƠ', 'MÃ NGƯỜI BỆNH',
              'KHOA BÁO CÁO/ XỬ TRÍ ADR', 'NGHỀ NGHIỆP người báo cáo', 'NGƯỜI BÁO CÁO', 'ĐIỆN THOẠI',
              'Đơn vị báo cáo\n(nơi báo cáo)', 'Đơn vị báo cáo (nơi báo cáo)', 'DẠNG BÁO CÁO',
              'Ngày báo cáo\n(mục 23 trong biểu mẫu BC ADR)', 'Ngày báo cáo (mục 23 trong biểu mẫu BC ADR)']
      },
      {
        ten: 'Thông tin bệnh nhân', icon: 'fa-user',
        cot: ['Giới tính', 'Tuổi', 'Cân nặng', 'Ngày sinh', 'Tiền sử bệnh/ dị ứng']
      },
      {
        ten: 'Diễn biến phản ứng ADR', icon: 'fa-triangle-exclamation',
        cot: ['PHẢN ỨNG XUẤT HIỆN SAU BAO LÂU',
              'Ngày xuất hiện phản ứng ADR\n(mục 5 trong biểu mẫu BC ADR)',
              'Ngày xuất hiện phản ứng ADR (mục 5 trong biểu mẫu BC ADR)',
              'Mô tả biểu hiện ADR', 'Mức độ nghiêm trọng', 'CÁC XÉT NGHIỆM LIÊN QUAN',
              'CÁCH XỬ TRÍ', 'Kết quả sau khi xử trí ADR']
      },
      {
        ten: 'Thuốc nghi ngờ gây ADR', icon: 'fa-pills',
        cot: ['HOẠT CHẤT nghi ngờ ADR', 'Nhóm thuốc', 'Tên phân nhóm',
              'Thuốc (tên gốc và tên thương mại)', 'Dạng bào chế, hàm lượng', 'Nhà sản xuất',
              'Số lô.', 'Số lô', 'Liều dùng 1 lần.', 'Liều dùng 1 lần', 'SỐ LẦN DÙNG TRONG NGÀY',
              'Đường dùng.', 'Đường dùng', 'NGÀY BẮT ĐẦU ĐIỀU TRỊ', 'NGÀY KẾT THÚC ĐIỀU TRỊ',
              'LÝ DO DÙNG THUỐC',
              'SAU KHI NGỪNG/GIẢM LIỀU CỦA THUỐC BỊ NGHI NGỜ, PHẢN ỨNG CÓ ĐƯỢC CẢI THIỆN KHÔNG',
              'TÁI SỬ DỤNG THUỐC BỊ NGHI NGỜ CÓ XUẤT HIỆN LẠI PHẢN ỨNG KHÔNG']
      },
      {
        ten: 'Thuốc dùng đồng thời', icon: 'fa-capsules',
        cot: ['THUỐC DÙNG ĐỒNG THỜI 1', 'TDĐT 2', 'TDĐT 3', 'TDĐT 4', 'TDĐT 5', 'TDĐT 6', 'TDĐT 7', 'TDĐT 8']
      },
      {
        ten: 'Đánh giá chuyên môn', icon: 'fa-stethoscope',
        cot: ['ĐÁNH GIÁ MỐI LIÊN QUAN GIỮA THUỐC VÀ ADR', 'ĐƠN VỊ THẨM ĐỊNH ADR THEO THANG NÀO?',
              'Ghi chú của Dược sĩ', 'PHẦN BÌNH LUẬN CỦA CBYT', 'link PDF ADR']
      }
    ];

    function chuanHoaTenCot(s) {
      return String(s || '').replace(/[\r\n]+/g, ' ').trim().replace(/\s+/g, ' ').toLowerCase();
    }

    var COT_DUOC_SUA = [];
    var INDEX_DUOC_SUA = [];
    var TEN_COT_AN = []; // danh sách TÊN TIÊU ĐỀ bị ẩn khỏi phần chi tiết (lấy từ layDanhSachCotAn())

    function loadPatients() {
      if (daTaiDuLieu) return; 
      daTaiDuLieu = true;

      document.getElementById('loadingMsg').style.display = 'block';
      document.getElementById('emptyMsg').style.display = 'none';
      document.getElementById('cardList').innerHTML = '';

      google.script.run
        .withSuccessHandler(function (cfg) {
          COT_DUOC_SUA = (cfg && cfg.success && cfg.cot) ? cfg.cot : [];
          INDEX_DUOC_SUA = COT_DUOC_SUA.map(chuCaiSangIndex);
          taiCauHinhCotAn();
        })
        .withFailureHandler(function () {
          COT_DUOC_SUA = [];
          INDEX_DUOC_SUA = [];
          taiCauHinhCotAn();
        })
        .layDanhSachCotDuocSua();
    }

    function taiCauHinhCotAn() {
      google.script.run
        .withSuccessHandler(function (cfg) {
          TEN_COT_AN = (cfg && cfg.success && cfg.ten) ? cfg.ten : [];
          taiDanhSachThat();
        })
        .withFailureHandler(function () {
          TEN_COT_AN = [];
          taiDanhSachThat();
        })
        .layDanhSachCotAn();
    }

    // So khớp tên tiêu đề với danh sách bị ẩn (không phân biệt hoa/thường, khoảng trắng thừa)
    function coTrongDanhSachAn(tieuDe) {
      var chuan = String(tieuDe || '').trim().toLowerCase();
      return TEN_COT_AN.some(function (t) {
        return String(t).trim().toLowerCase() === chuan;
      });
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
        .getPatients(KHOA_DANG_NHAP);
    }

    function boDauTV(str) {
      return String(str)
        .normalize('NFD')
        .replace(/[\u0300-\u036f]/g, '')
        .replace(/đ/g, 'd').replace(/Đ/g, 'D')
        .toLowerCase();
    }

    var itemDangChon = null; // báo cáo đang được chọn xem chi tiết (giữ nguyên khi tìm kiếm nếu còn trong danh sách)

    function ve_danh_sach(items) {
      var cardList = document.getElementById('cardList');
      cardList.innerHTML = '';

      document.getElementById('resultCount').textContent =
        'Hiển thị ' + items.length + ' / ' + TAT_CA_ITEMS.length + ' báo cáo';

      if (items.length === 0) {
        cardList.innerHTML = '<div style="padding:20px; text-align:center; color:#9ca3af;">Không tìm thấy báo cáo phù hợp.</div>';
        if (itemDangChon) { itemDangChon = null; }
        return;
      }

      var conKhopDuocChon = false;

      items.forEach(function (item) {
        var row = item.row;

        var rowEl = document.createElement('div');
        rowEl.className = 'adr-list-row';

        var top = document.createElement('div');
        top.className = 'row-top';
        var ten = document.createElement('div');
        ten.className = 'row-ten';
        ten.textContent = row[INDEX_TOM_TAT[0]] || 'Không rõ tên';
        var thoiGian = document.createElement('div');
        thoiGian.className = 'row-thoigian';
        thoiGian.textContent = row[INDEX_TOM_TAT[1]] || '';
        top.appendChild(ten);
        top.appendChild(thoiGian);

        var nb = document.createElement('div');
        nb.className = 'row-nb';
        nb.textContent = row[INDEX_TOM_TAT[2]] || '';

        var mota = document.createElement('div');
        mota.className = 'row-mota';
        mota.textContent = row[INDEX_TOM_TAT[3]] || '';

        rowEl.appendChild(top);
        rowEl.appendChild(nb);
        rowEl.appendChild(mota);

        if (itemDangChon && itemDangChon.soDong === item.soDong) {
          rowEl.classList.add('active');
          conKhopDuocChon = true;
        }

        rowEl.addEventListener('click', function () {
          document.querySelectorAll('.adr-list-row').forEach(function (r) { r.classList.remove('active'); });
          rowEl.classList.add('active');
          itemDangChon = item;
          ve_chi_tiet(item);
        });

        cardList.appendChild(rowEl);
      });

      // Nếu báo cáo đang chọn không còn trong danh sách hiện tại (bị lọc mất do tìm kiếm) -> bỏ chọn
      if (itemDangChon && !conKhopDuocChon) {
        itemDangChon = null;
        var panel = document.getElementById('detailPanel');
        panel.innerHTML = '<div class="detail-panel-empty"><i class="fa-solid fa-hand-pointer"></i><div>Chọn 1 báo cáo bên trái để xem chi tiết</div></div>';
      }
    }

    // Vẽ toàn bộ chi tiết (nhóm theo chủ đề) + thanh Sửa/Lưu/Hủy + In PDF vào khung bên phải
    function ve_chi_tiet(item) {
      var row = item.row;
      var soDong = item.soDong;

      var panel = document.getElementById('detailPanel');
      panel.innerHTML = '';

      var header = document.createElement('div');
      header.className = 'detail-panel-header';
      var headerLeft = document.createElement('div');
      var h2 = document.createElement('h2');
      h2.textContent = row[INDEX_TOM_TAT[0]] || 'Không rõ tên';
      var sub = document.createElement('div');
      sub.className = 'sub';
      sub.innerHTML = '<i class="fa-regular fa-clock"></i> ' + (row[INDEX_TOM_TAT[1]] || '—') +
        ' &nbsp;•&nbsp; ' + (row[INDEX_TOM_TAT[2]] || '—');
      headerLeft.appendChild(h2);
      headerLeft.appendChild(sub);
      header.appendChild(headerLeft);
      panel.appendChild(header);

      var oCoTheSua = [];

      var cotHopLe = [];
      row.forEach(function (cell, colIndex) {
        if (INDEX_TOM_TAT.indexOf(colIndex) !== -1) return;
        if (coTrongDanhSachAn(TAT_CA_HEADERS[colIndex])) return;
        var laCotDuocSua = INDEX_DUOC_SUA.indexOf(colIndex) !== -1;
        if (!laCotDuocSua && (cell === null || cell === undefined || cell === '')) return;
        cotHopLe.push({ colIndex: colIndex, tieuDeChuan: chuanHoaTenCot(TAT_CA_HEADERS[colIndex]) });
      });

      var daDùng = {};

      function taoOChiTiet(colIndex) {
        var cell = row[colIndex];
        var viTriTrongCotSua = INDEX_DUOC_SUA.indexOf(colIndex);
        var laCotDuocSua = viTriTrongCotSua !== -1;

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

        if (laCotDuocSua) {
          oCoTheSua.push({
            colIndex: colIndex,
            chuCai: COT_DUOC_SUA[viTriTrongCotSua],
            valueDiv: val,
            giaTriGoc: (cell === null || cell === undefined) ? '' : cell
          });
        }
        return itemEl;
      }

      function taoNhom(tieuDeNhom, icon, danhSachColIndex) {
        var sectionBox = document.createElement('div');
        sectionBox.className = 'adr-detail-section-box';

        var title = document.createElement('div');
        title.className = 'adr-section-title';
        title.innerHTML = '<i class="fa-solid ' + icon + '"></i> ' + tieuDeNhom;
        sectionBox.appendChild(title);

        var grid = document.createElement('div');
        grid.className = 'adr-detail-grid';
        danhSachColIndex.forEach(function (colIndex) {
          grid.appendChild(taoOChiTiet(colIndex));
        });
        sectionBox.appendChild(grid);
        return sectionBox;
      }

      CAC_NHOM_CHI_TIET.forEach(function (nhom) {
        var tenChuanTrongNhom = nhom.cot.map(chuanHoaTenCot);
        var colIndexKhopNhom = cotHopLe
          .filter(function (c) { return !daDùng[c.colIndex] && tenChuanTrongNhom.indexOf(c.tieuDeChuan) !== -1; })
          .map(function (c) { return c.colIndex; });

        if (colIndexKhopNhom.length === 0) return;
        colIndexKhopNhom.forEach(function (ci) { daDùng[ci] = true; });
        panel.appendChild(taoNhom(nhom.ten, nhom.icon, colIndexKhopNhom));
      });

      var conLai = cotHopLe.filter(function (c) { return !daDùng[c.colIndex]; }).map(function (c) { return c.colIndex; });
      if (conLai.length > 0) {
        panel.appendChild(taoNhom('Khác', 'fa-ellipsis', conLai));
      }

      var toolbar = document.createElement('div');
      toolbar.className = 'adr-edit-toolbar';

      if (oCoTheSua.length > 0) {
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

      var btnInPdf = document.createElement('button');
      btnInPdf.className = 'adr-btn-sm adr-btn-print';
      btnInPdf.innerHTML = '<i class="fa-solid fa-file-pdf"></i> In PDF';

      var msgPdf = document.createElement('span');
      msgPdf.className = 'adr-save-msg';

      toolbar.appendChild(btnInPdf);
      toolbar.appendChild(msgPdf);

      btnInPdf.addEventListener('click', function (e) {
        e.stopPropagation();
        btnInPdf.disabled = true;
        msgPdf.className = 'adr-save-msg';
        msgPdf.innerHTML = '<i class="fa-solid fa-spinner fa-spin"></i> Đang tạo PDF...';

        google.script.run
          .withSuccessHandler(function (res) {
            btnInPdf.disabled = false;

            if (!res.success) {
              msgPdf.className = 'adr-save-msg err';
              msgPdf.textContent = '❌ ' + res.message;
              return;
            }

            msgPdf.className = 'adr-save-msg ok';
            msgPdf.innerHTML = '✔ Đã tạo — <a href="' + res.url + '" target="_blank" rel="noopener">Mở PDF</a>';
          })
          .withFailureHandler(function (err) {
            btnInPdf.disabled = false;
            msgPdf.className = 'adr-save-msg err';
            msgPdf.textContent = '❌ Lỗi hệ thống: ' + err.message;
          })
          .xuatPdfBaoCao(soDong);
      });

      panel.appendChild(toolbar);
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

    function refreshPatients() {
      daTaiDuLieu = false;
      loadPatients();
    }

    // ================================================================
    // ---------- TAB 3: THỐNG KÊ ----------
    // ================================================================
    var daTaiThongKe = false;
    var BANG_MAU_DO = ['#dc2626', '#ef4444', '#f87171', '#fca5a5', '#b91c1c', '#991b1b', '#fecaca', '#7f1d1d', '#f43f5e', '#e11d48'];

    Chart.defaults.font.family = "'Inter', 'Segoe UI', Arial, sans-serif";
    Chart.defaults.color = '#6b7280';

    function loadThongKe() {
      
      // KIỂM TRA QUYỀN TRƯỚC KHI GỌI MÁY CHỦ
      if (!QUYEN_XEM_THONG_KE) {
        document.getElementById('tkLoading').style.display = 'none';
        document.getElementById('tkTuChoi').style.display = 'block';
        return; 
      }

      if (daTaiThongKe) return;
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
        .thongKeADR(KHOA_DANG_NHAP, MAT_KHAU_DANG_NHAP); // GỬI TÊN KHOA VÀ MK XUỐNG MÁY CHỦ
    }

    function ve_thong_ke(res) {
      document.getElementById('statTongSo').textContent = res.tongSoBaoCao;
      document.getElementById('statSoKhoa').textContent = res.theoKhoa.labels.length;
      document.getElementById('statSoHoatChat').textContent = res.theoHoatChat.labels.length;
      document.getElementById('statSoThang').textContent = res.theoThang.labels.length;

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

    // ================================================================
    // ---------- TAB 4: KHO BÁO CÁO TỪ DRIVE (SWIPE TO REVEAL) -------
    // ================================================================
    var daTaiKhoBaoCao = false;

    function refreshKhoBaoCao() {
      daTaiKhoBaoCao = false;
      document.getElementById('khoPdfViewer').style.display = 'none';
      document.getElementById('khoChuaChon').style.display = 'flex';
      loadKhoBaoCao();
    }

    function loadKhoBaoCao() {
      if (daTaiKhoBaoCao) return;
      daTaiKhoBaoCao = true;

      document.getElementById('khoLoadingMsg').style.display = 'block';
      document.getElementById('khoEmptyMsg').style.display = 'none';
      document.getElementById('vungDanhSachKho').innerHTML = '';

      google.script.run
        .withSuccessHandler(function(duLieu) {
          document.getElementById('khoLoadingMsg').style.display = 'none';
          ve_kho_bao_cao(duLieu);
        })
        .withFailureHandler(function(err) {
          document.getElementById('khoLoadingMsg').style.display = 'none';
          document.getElementById('vungDanhSachKho').innerHTML = '<div style="color:var(--red-600); text-align:center; padding: 20px; font-weight:600;">❌ Lỗi: ' + err.message + '</div>';
        })
        .layDuLieuKhoBaoCao(); 
    }

    function ve_kho_bao_cao(duLieuNhom) {
      var vungChua = document.getElementById('vungDanhSachKho');
      var cacThang = Object.keys(duLieuNhom);
      
      if (cacThang.length === 0) {
        document.getElementById('khoEmptyMsg').style.display = 'flex';
        return;
      }

      cacThang.forEach(function(thang, index) {
        var details = document.createElement('details');
        details.className = 'thang-box';
        if (index === 0) details.open = true; 

        var summary = document.createElement('summary');
        summary.innerHTML = '<i class="fa-solid fa-calendar-days"></i> ' + thang + ' <span style="color:var(--muted); font-size:12px; margin-left:auto;">' + duLieuNhom[thang].length + ' file</span>';
        details.appendChild(summary);

        var contentDiv = document.createElement('div');
        
        duLieuNhom[thang].forEach(function(file) {
          
          // --- CẤU TRÚC KÉO SANG (SWIPE TO REVEAL) ---
          var swipeWrapper = document.createElement('div');
          swipeWrapper.className = 'swipe-wrapper';

          var swipeTrack = document.createElement('div');
          swipeTrack.className = 'swipe-track';

          // 1. Phần hiển thị tên file (Bấm vào để xem)
          var itemDiv = document.createElement('div');
          itemDiv.className = 'file-item';
          
          var spanTen = document.createElement('div');
          spanTen.className = 'file-name';
          spanTen.innerHTML = '<i class="fa-solid fa-file-pdf"></i> ' + file.tenFile;
          
          spanTen.onclick = function() {
            document.querySelectorAll('.file-item').forEach(function(el){ el.classList.remove('active'); });
            itemDiv.classList.add('active');
            document.getElementById('khoChuaChon').style.display = 'none';
            var iframe = document.getElementById('khoPdfViewer');
            iframe.style.display = 'block';
            iframe.src = file.url;
          };

          itemDiv.appendChild(spanTen);

          // 2. Phần nút bấm ẩn (Kéo sang trái để hiện)
          var swipeActions = document.createElement('div');
          swipeActions.className = 'swipe-actions';

          var btnMail = document.createElement('button');
          btnMail.className = 'btn-gui-mail';
          btnMail.innerHTML = '<i class="fa-solid fa-paper-plane"></i> Mail';
          btnMail.onclick = function(e) {
            e.stopPropagation(); 
            guiBaoCaoMail(file.id, btnMail);
          };

          var btnXoa = document.createElement('button');
          btnXoa.className = 'btn-xoa-file';
          btnXoa.innerHTML = '<i class="fa-solid fa-trash-can"></i> Xóa';
          btnXoa.onclick = function(e) {
            e.stopPropagation();
            xoaBaoCaoGiaoDien(file.id, btnXoa, swipeWrapper);
          };

          swipeActions.appendChild(btnMail);
          swipeActions.appendChild(btnXoa);

          // Ráp các mảnh lại với nhau
          swipeTrack.appendChild(itemDiv);
          swipeTrack.appendChild(swipeActions);
          swipeWrapper.appendChild(swipeTrack);
          contentDiv.appendChild(swipeWrapper);
        });

        details.appendChild(contentDiv);
        vungChua.appendChild(details);
      });
    }

    function guiBaoCaoMail(fileId, btnElement) {
      var xacNhan = confirm("Bạn có chắc chắn muốn gửi báo cáo này đến danh sách email mặc định?");
      if (!xacNhan) return;

      var htmlCu = btnElement.innerHTML;
      btnElement.innerHTML = '<i class="fa-solid fa-spinner fa-spin"></i>';
      btnElement.disabled = true;

      google.script.run
        .withSuccessHandler(function(ketQua) {
          if (ketQua.thanhCong) {
            btnElement.innerHTML = '<i class="fa-solid fa-check"></i> Đã gửi';
            btnElement.style.background = '#dcfce7';
          } else {
            alert("Lỗi: " + ketQua.tinNhan);
            btnElement.innerHTML = htmlCu;
            btnElement.disabled = false;
          }
          
          setTimeout(function() {
             btnElement.innerHTML = htmlCu;
             btnElement.disabled = false;
             btnElement.style.background = '';
          }, 3000);
        })
        .withFailureHandler(function(err) {
          alert("Lỗi hệ thống: " + err.message);
          btnElement.innerHTML = htmlCu;
          btnElement.disabled = false;
        })
        .guiBaoCaoQuaEmail(fileId);
    }

    function xoaBaoCaoGiaoDien(fileId, btnElement, wrapperElement) {
      var xacNhan = confirm("Báo cáo này sẽ bị chuyển vào Thùng rác Drive.\nBạn có chắc chắn muốn xóa?");
      if (!xacNhan) return;

      var htmlCu = btnElement.innerHTML;
      btnElement.innerHTML = '<i class="fa-solid fa-spinner fa-spin"></i>';
      btnElement.disabled = true;

      google.script.run
        .withSuccessHandler(function(ketQua) {
          if (ketQua.thanhCong) {
            // Hiệu ứng thu hẹp rồi biến mất mượt mà
            wrapperElement.style.transition = "opacity 0.3s, height 0.3s, padding 0.3s";
            wrapperElement.style.opacity = "0";
            wrapperElement.style.height = "0";
            wrapperElement.style.padding = "0";
            setTimeout(function() {
              wrapperElement.remove(); // Xóa hẳn phần tử HTML
              
              // Ẩn màn hình xem PDF đi nếu đang xem file vừa xóa
              document.getElementById('khoPdfViewer').style.display = 'none';
              document.getElementById('khoChuaChon').style.display = 'flex';
            }, 300);
          } else {
            alert("Lỗi: " + ketQua.tinNhan);
            btnElement.innerHTML = htmlCu;
            btnElement.disabled = false;
          }
        })
        .withFailureHandler(function(err) {
          alert("Lỗi hệ thống: " + err.message);
          btnElement.innerHTML = htmlCu;
          btnElement.disabled = false;
        })
        .xoaBaoCaoDrive(fileId);
    }
  </script>

</body>
</html>
