<!DOCTYPE html>
<html>
<head>
  <base target="_top">
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Thống kê ADR</title>

  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700;800&display=swap" rel="stylesheet">
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.1/css/all.min.css">
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
      --ink: #1f2430;
      --ink-soft: #4b5563;
      --muted: #9aa4b2;
      --line: #ece7e7;
      --bg: #faf7f7;
    }
    * { box-sizing: border-box; }
    body {
      margin: 0;
      font-family: 'Inter', 'Segoe UI', Arial, sans-serif;
      background: var(--bg);
      background-image:
        radial-gradient(circle at 100% 0%, rgba(220,38,38,0.06), transparent 40%),
        radial-gradient(circle at 0% 100%, rgba(220,38,38,0.05), transparent 35%);
      color: var(--ink);
      -webkit-font-smoothing: antialiased;
    }

    header {
      background: linear-gradient(120deg, var(--red-700) 0%, var(--red-600) 55%, var(--red-500) 100%);
      color: #fff;
      padding: 20px 26px;
      box-shadow: 0 4px 20px rgba(185,28,28,0.25);
      display: flex;
      align-items: center;
      justify-content: space-between;
      flex-wrap: wrap;
      gap: 12px;
    }
    header .left { display: flex; align-items: center; gap: 14px; }
    header .header-icon {
      width: 42px; height: 42px;
      background: rgba(255,255,255,0.18);
      border-radius: 12px;
      display: flex; align-items: center; justify-content: center;
      font-size: 19px;
      flex-shrink: 0;
    }
    header h1 { margin: 0; font-size: 20px; font-weight: 800; }
    header .header-sub { font-size: 12.5px; opacity: 0.85; margin-top: 2px; }

    .back-link {
      display: inline-flex;
      align-items: center;
      gap: 8px;
      background: rgba(255,255,255,0.16);
      color: #fff;
      text-decoration: none;
      padding: 10px 18px;
      border-radius: 10px;
      font-size: 13.5px;
      font-weight: 600;
      transition: background 0.15s;
    }
    .back-link:hover { background: rgba(255,255,255,0.28); }

    .container { padding: 26px 22px 60px; max-width: 1300px; margin: 0 auto; }

    /* ---- Thẻ tổng quan ---- */
    .stat-row {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
      gap: 16px;
      margin-bottom: 24px;
    }
    .stat-card {
      background: #fff;
      border-radius: 14px;
      padding: 20px 22px;
      border: 1px solid var(--line);
      box-shadow: 0 1px 3px rgba(185,28,28,0.06);
      display: flex;
      align-items: center;
      gap: 16px;
    }
    .stat-card .icon {
      width: 46px; height: 46px;
      border-radius: 12px;
      background: var(--red-50);
      color: var(--red-600);
      display: flex; align-items: center; justify-content: center;
      font-size: 19px;
      flex-shrink: 0;
    }
    .stat-card .num { font-size: 26px; font-weight: 800; color: var(--ink); line-height: 1.1; }
    .stat-card .label { font-size: 12.5px; color: var(--muted); font-weight: 500; margin-top: 2px; }

    /* ---- Lưới biểu đồ ---- */
    .chart-grid {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(420px, 1fr));
      gap: 20px;
    }
    .chart-card {
      background: #fff;
      border-radius: 14px;
      padding: 22px;
      border: 1px solid var(--line);
      box-shadow: 0 1px 3px rgba(185,28,28,0.06);
    }
    .chart-card.full { grid-column: 1 / -1; }
    .chart-card h3 {
      margin: 0 0 16px;
      font-size: 14.5px;
      font-weight: 700;
      color: var(--ink);
      display: flex;
      align-items: center;
      gap: 9px;
    }
    .chart-card h3 i { color: var(--red-500); font-size: 15px; }
    .chart-wrap { position: relative; height: 300px; }

    /* ---- Loading / lỗi ---- */
    #loadingBox {
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      gap: 14px;
      padding: 80px 20px;
      color: var(--muted);
      font-size: 14px;
      font-weight: 500;
    }
    .spinner {
      width: 44px; height: 44px;
      border-radius: 50%;
      border: 4px solid var(--red-100);
      border-top-color: var(--red-600);
      animation: spin 0.8s linear infinite;
    }
    @keyframes spin { to { transform: rotate(360deg); } }

    #errorBox {
      display: none;
      background: var(--red-50);
      color: var(--red-700);
      border: 1px solid var(--red-200);
      padding: 16px 20px;
      border-radius: 12px;
      font-size: 14px;
      font-weight: 500;
    }

    #contentBox { display: none; }

    @media (max-width: 760px) {
      .container { padding: 16px 12px 50px; }
      header { padding: 16px 18px; }
      header h1 { font-size: 17px; }
      .chart-grid { grid-template-columns: 1fr; }
      .chart-wrap { height: 260px; }
    }
  </style>
</head>
<body>

  <header>
    <div class="left">
      <div class="header-icon"><i class="fa-solid fa-chart-pie"></i></div>
      <div>
        <h1>Thống kê ADR</h1>
        <div class="header-sub">Tổng hợp số liệu từ sheet "tong hop ADR"</div>
      </div>
    </div>
    <a class="back-link" href="?"><i class="fa-solid fa-arrow-left"></i> Quay lại nhập báo cáo</a>
  </header>

  <div class="container">

    <div id="loadingBox">
      <div class="spinner"></div>
      <div>Đang tính toán số liệu thống kê...</div>
    </div>

    <div id="errorBox"></div>

    <div id="contentBox">
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

  <script>
    // Bảng màu đỏ đồng bộ với giao diện chính, dùng cho nhiều biểu đồ
    var BANG_MAU_DO = ['#dc2626', '#ef4444', '#f87171', '#fca5a5', '#b91c1c', '#991b1b', '#fecaca', '#7f1d1d', '#f43f5e', '#e11d48'];

    Chart.defaults.font.family = "'Inter', 'Segoe UI', Arial, sans-serif";
    Chart.defaults.color = '#6b7280';

    google.script.run
      .withSuccessHandler(function (res) {
        document.getElementById('loadingBox').style.display = 'none';

        if (!res.success) {
          var box = document.getElementById('errorBox');
          box.style.display = 'block';
          box.textContent = '❌ ' + res.message;
          return;
        }

        document.getElementById('contentBox').style.display = 'block';
        veThongKe(res);
      })
      .withFailureHandler(function (err) {
        document.getElementById('loadingBox').style.display = 'none';
        var box = document.getElementById('errorBox');
        box.style.display = 'block';
        box.textContent = '❌ Lỗi hệ thống: ' + err.message;
      })
      .thongKeADR();

    function veThongKe(res) {
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
