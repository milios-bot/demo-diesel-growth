<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Diesel Growth — Client Portal Demo</title>
<link href="https://fonts.googleapis.com/css2?family=Barlow+Condensed:wght@300;400;500;600;700;800;900&family=Oswald:wght@400;500;600;700&family=Teko:wght@400;500;600;700&display=swap" rel="stylesheet">
<script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/4.4.1/chart.umd.min.js"></script>
<style>
:root{
  --primary:#F97316;--primary-light:#FB923C;--primary-glow:rgba(249,115,22,0.25);--primary-dim:rgba(249,115,22,0.08);
  --bg-base:#0d0d0d;--bg-card:#161616;--bg-elevated:#1e1e1e;--border:#222;--border-light:#2a2a2a;
  --text-primary:#f0f0f0;--text-secondary:#aaa;--text-muted:#666;--text-dim:#444;
  --green:#22c55e;--green-dim:rgba(34,197,94,0.12);--red:#ef4444;--red-dim:rgba(239,68,68,0.12);
  --yellow:#eab308;--yellow-dim:rgba(234,179,8,0.12);--blue:#3b82f6;--blue-dim:rgba(59,130,246,0.12);
  --radius:12px;--font-display:'Oswald',sans-serif;--font-heading:'Teko',sans-serif;--font-body:'Barlow Condensed',sans-serif;
}
*{margin:0;padding:0;box-sizing:border-box}
body{background:var(--bg-base);color:var(--text-primary);font-family:var(--font-body);min-height:100vh;overflow-x:hidden}
.diamond-bg{background-image:url("data:image/svg+xml,%3Csvg width='24' height='24' xmlns='http://www.w3.org/2000/svg'%3E%3Crect width='24' height='24' fill='%23131313'/%3E%3Cpath d='M12 0L24 12L12 24L0 12Z' fill='none' stroke='%231a1a1a' stroke-width='.6'/%3E%3Ccircle cx='12' cy='12' r='1.2' fill='%23181818'/%3E%3C/svg%3E")}
.login-wrapper{min-height:100vh;display:flex;align-items:center;justify-content:center;padding:24px}
.login-box{background:var(--bg-card);border:1px solid var(--border-light);border-radius:16px;padding:48px 40px;width:100%;max-width:420px;position:relative;overflow:hidden;animation:slideUp .5s ease}
.login-box::before{content:'';position:absolute;top:0;left:0;right:0;height:4px;background:linear-gradient(90deg,var(--primary),var(--primary-light),var(--primary))}
.login-logo{display:flex;align-items:center;gap:14px;margin-bottom:32px}
.login-logo-icon{width:52px;height:52px;border-radius:12px;background:linear-gradient(135deg,var(--primary),#EA580C);display:flex;align-items:center;justify-content:center;font-family:var(--font-display);font-weight:700;font-size:22px;color:#fff;box-shadow:0 4px 24px var(--primary-glow)}
.login-logo h1{font-family:var(--font-display);font-size:24px;font-weight:700;text-transform:uppercase;letter-spacing:1px;line-height:1.1}
.login-logo h1 span{color:var(--primary)}
.login-subtitle{color:var(--text-muted);font-size:13px;letter-spacing:.5px}
.login-field{margin-bottom:18px}
.login-field label{display:block;font-size:11px;text-transform:uppercase;letter-spacing:1.5px;color:var(--text-muted);margin-bottom:8px;font-weight:600}
.login-field input{width:100%;padding:14px 16px;background:var(--bg-base);border:1px solid var(--border);border-radius:8px;color:var(--text-primary);font-family:var(--font-body);font-size:15px;transition:border-color .2s;outline:none}
.login-field input:focus{border-color:var(--primary);box-shadow:0 0 0 3px var(--primary-dim)}
.login-btn{width:100%;padding:14px;background:linear-gradient(135deg,var(--primary),#EA580C);border:none;border-radius:8px;color:#fff;font-family:var(--font-display);font-size:16px;font-weight:600;text-transform:uppercase;letter-spacing:2px;cursor:pointer;transition:all .2s;margin-top:8px}
.login-btn:hover{transform:translateY(-1px);box-shadow:0 6px 24px var(--primary-glow)}
.demo-hint{text-align:center;margin-top:16px;font-size:12px;color:var(--text-dim)}
.demo-hint span{color:var(--primary);font-weight:600}
.dashboard{display:none;animation:fadeIn .4s ease}.dashboard.active{display:block}
.header{border-bottom:3px solid var(--primary)}
.header-inner{max-width:1280px;margin:0 auto;padding:18px 28px;display:flex;justify-content:space-between;align-items:center;flex-wrap:wrap;gap:16px}
.header-left{display:flex;align-items:center;gap:14px}
.header-logo{width:44px;height:44px;border-radius:10px;background:linear-gradient(135deg,var(--primary),#EA580C);display:flex;align-items:center;justify-content:center;font-family:var(--font-display);font-weight:700;font-size:19px;color:#fff;box-shadow:0 3px 16px var(--primary-glow)}
.header-title{font-family:var(--font-display);font-size:20px;font-weight:700;text-transform:uppercase;letter-spacing:1px}
.header-title span{color:var(--primary)}
.header-label{font-size:12px;color:var(--text-dim);letter-spacing:.5px}
.header-right{text-align:right;display:flex;align-items:center;gap:12px;flex-wrap:wrap;justify-content:flex-end}
.live-indicator{display:flex;align-items:center;gap:6px;font-size:11px;color:var(--green);font-weight:600;text-transform:uppercase;letter-spacing:1px}
.live-dot{width:7px;height:7px;border-radius:50%;background:var(--green);animation:pulse 2s infinite}
@keyframes pulse{0%,100%{opacity:1;transform:scale(1)}50%{opacity:.5;transform:scale(1.3)}}
.live-clock{font-size:12px;color:var(--text-muted);letter-spacing:.5px}
.header-client-name{font-size:16px;font-weight:600;color:var(--text-secondary)}
.header-client-meta{font-size:12px;color:var(--text-dim)}
.badge-owner{display:inline-block;padding:4px 12px;border-radius:20px;font-size:10px;font-weight:700;text-transform:uppercase;letter-spacing:1px;background:var(--primary-dim);color:var(--primary);border:1px solid rgba(249,115,22,0.3)}
.logout-btn{background:transparent;border:1px solid var(--border);color:var(--text-muted);padding:5px 14px;border-radius:6px;cursor:pointer;font-family:var(--font-body);font-size:11px;font-weight:600;text-transform:uppercase;letter-spacing:1px;transition:all .2s}
.logout-btn:hover{border-color:var(--primary);color:var(--primary)}
.nav-tabs{max-width:1280px;margin:0 auto;padding:14px 28px 0;display:flex;gap:4px;border-bottom:1px solid var(--border);overflow-x:auto}
.nav-tab{background:transparent;border:none;border-bottom:2px solid transparent;color:var(--text-muted);padding:10px 22px;cursor:pointer;font-family:var(--font-body);font-size:13px;font-weight:600;text-transform:uppercase;letter-spacing:1.5px;border-radius:8px 8px 0 0;transition:all .2s;white-space:nowrap}
.nav-tab:hover{color:var(--text-secondary);background:var(--bg-card)}
.nav-tab.active{color:var(--primary);background:var(--bg-elevated);border-bottom-color:var(--primary)}
.main{max-width:1280px;margin:0 auto;padding:28px}
.tab-content{display:none;animation:fadeIn .3s ease}.tab-content.active{display:block}
.stats-grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(185px,1fr));gap:16px;margin-bottom:28px}
.stat-card{background:linear-gradient(145deg,var(--bg-elevated),var(--bg-card));border:1px solid var(--border-light);border-radius:var(--radius);padding:20px;position:relative;overflow:hidden;transition:transform .2s,border-color .2s}
.stat-card:hover{transform:translateY(-2px);border-color:#333}
.stat-glow{position:absolute;top:0;right:0;width:70px;height:70px;border-radius:0 12px 0 50px;pointer-events:none}
.glow-up{background:linear-gradient(135deg,rgba(249,115,22,0.12),transparent)}
.glow-down{background:linear-gradient(135deg,rgba(34,197,94,0.12),transparent)}
.stat-label{font-size:11px;text-transform:uppercase;letter-spacing:1.5px;color:var(--text-muted);margin-bottom:8px;font-weight:600}
.stat-value{font-family:var(--font-heading);font-size:36px;font-weight:600;color:var(--text-primary);line-height:1;margin-bottom:8px}
.stat-change{font-size:12px;font-weight:600;display:flex;align-items:center;gap:4px}
.change-up{color:var(--primary)}.change-down{color:var(--green)}
.change-vs{color:var(--text-dim);font-weight:400}
.panel{background:linear-gradient(145deg,var(--bg-elevated),var(--bg-card));border:1px solid var(--border-light);border-radius:var(--radius);padding:24px;margin-bottom:20px}
.panel-header{display:flex;justify-content:space-between;align-items:center;margin-bottom:18px;flex-wrap:wrap;gap:10px}
.panel-title{font-size:13px;text-transform:uppercase;letter-spacing:1.5px;color:var(--text-muted);font-weight:600}
.panel-action{background:transparent;border:1px solid var(--border);color:var(--primary);padding:5px 14px;border-radius:6px;cursor:pointer;font-family:var(--font-body);font-size:11px;font-weight:600;letter-spacing:1px;text-transform:uppercase;transition:all .2s}
.panel-action:hover{background:var(--primary-dim);border-color:var(--primary)}
.grid-2{display:grid;grid-template-columns:1fr 1fr;gap:20px}
.grid-2-1{display:grid;grid-template-columns:2fr 1fr;gap:20px}
.table-wrap{overflow-x:auto}
table{width:100%;border-collapse:collapse}
th{text-align:left;padding:10px 14px;font-size:11px;color:var(--text-dim);text-transform:uppercase;letter-spacing:1px;border-bottom:1px solid var(--border-light);font-weight:600}
td{padding:12px 14px;font-size:13px;color:var(--text-secondary);border-bottom:1px solid #1a1a1a}
tr.clickable-row{cursor:pointer;transition:background .15s}
tr.clickable-row:hover td{background:rgba(249,115,22,0.06)}
.td-id{color:var(--primary);font-weight:600}.td-bold{color:var(--text-primary);font-weight:500}.td-dim{color:var(--text-muted)}
.badge{display:inline-block;padding:3px 10px;border-radius:20px;font-size:10px;font-weight:700;text-transform:uppercase;letter-spacing:.5px}
.badge-green{background:var(--green-dim);color:var(--green);border:1px solid rgba(34,197,94,0.2)}
.badge-orange{background:var(--primary-dim);color:var(--primary);border:1px solid rgba(249,115,22,0.2)}
.badge-gray{background:rgba(100,100,100,0.12);color:#888;border:1px solid rgba(100,100,100,0.2)}
.badge-red{background:var(--red-dim);color:var(--red);border:1px solid rgba(239,68,68,0.2)}
.badge-yellow{background:var(--yellow-dim);color:var(--yellow);border:1px solid rgba(234,179,8,0.2)}
.badge-blue{background:var(--blue-dim);color:#60a5fa;border:1px solid rgba(59,130,246,0.2)}
.badge-purple{background:rgba(168,85,247,0.12);color:#c084fc;border:1px solid rgba(168,85,247,0.2)}
.insight-card{display:flex;gap:14px;align-items:flex-start;background:var(--primary-dim);border:1px solid var(--border);border-left:3px solid var(--primary);border-radius:10px;padding:16px;margin-bottom:12px}
.insight-icon{font-size:22px;flex-shrink:0}
.insight-text{font-size:14px;color:#bbb;line-height:1.55}
.tech-card{background:var(--bg-card);border:1px solid var(--border);border-radius:10px;padding:16px;margin-bottom:10px}
.tech-name{font-weight:600;font-size:15px;color:var(--text-primary)}
.tech-meta{font-size:12px;color:var(--text-muted);margin-top:3px}
.tech-stat-num{font-family:var(--font-heading);font-size:24px;font-weight:600;color:var(--primary)}
.tech-stat-label{font-size:11px;color:var(--text-dim);text-transform:uppercase;letter-spacing:1px}
.duty-badge{display:inline-flex;align-items:center;gap:5px;font-size:12px;font-weight:700;text-transform:uppercase;letter-spacing:1px}
.duty-on{color:var(--green)}.duty-off{color:var(--red)}
.duty-dot{width:7px;height:7px;border-radius:50%}
.dot-on{background:var(--green)}.dot-off{background:var(--red)}
.chart-container{position:relative;width:100%;height:240px}
.modal-overlay{display:none;position:fixed;top:0;left:0;right:0;bottom:0;background:rgba(0,0,0,0.7);backdrop-filter:blur(4px);z-index:1000;align-items:center;justify-content:center;padding:20px}
.modal-overlay.open{display:flex}
.modal{background:var(--bg-card);border:1px solid var(--border-light);border-radius:16px;width:100%;max-width:600px;max-height:90vh;overflow-y:auto;position:relative;animation:slideUp .3s ease}
.modal-top{background:linear-gradient(135deg,var(--bg-elevated),var(--bg-card));padding:24px 28px;border-radius:16px 16px 0 0;border-bottom:1px solid var(--border);position:relative}
.modal-close{position:absolute;top:16px;right:20px;background:transparent;border:1px solid var(--border);color:var(--text-muted);width:32px;height:32px;border-radius:8px;cursor:pointer;font-size:18px;display:flex;align-items:center;justify-content:center;transition:all .2s}
.modal-close:hover{border-color:var(--primary);color:var(--primary)}
.modal-job-id{font-family:var(--font-display);font-size:24px;font-weight:700;margin-bottom:4px}
.modal-job-id span{color:var(--primary)}
.modal-body{padding:24px 28px}
.detail-section{margin-bottom:20px}
.detail-section-title{font-size:11px;text-transform:uppercase;letter-spacing:2px;color:var(--primary);font-weight:700;margin-bottom:12px;padding-bottom:6px;border-bottom:1px solid var(--border)}
.detail-grid{display:grid;grid-template-columns:1fr 1fr;gap:0}
.detail-row{display:flex;flex-direction:column;padding:8px 0}
.detail-row.full{grid-column:1/-1}
.detail-label{font-size:11px;text-transform:uppercase;letter-spacing:1px;color:var(--text-dim);font-weight:600;margin-bottom:3px}
.detail-value{font-size:15px;color:var(--text-primary);font-weight:500}
.detail-value.urgent{color:var(--red);font-weight:700}
.detail-value.low{color:var(--green)}
.detail-value.not-driveable{color:var(--red);font-weight:700}
.detail-value.driveable{color:var(--green)}
.modal-note{background:var(--primary-dim);border:1px solid var(--border);border-left:3px solid var(--primary);border-radius:8px;padding:12px 16px;font-size:13px;color:var(--text-secondary);line-height:1.5}
/* PIPELINE */
.pipeline-wrap{display:grid;grid-template-columns:repeat(6,1fr);gap:12px;overflow-x:auto;padding-bottom:8px}
.pipeline-col{background:var(--bg-card);border:1px solid var(--border);border-radius:var(--radius);min-width:160px}
.pipeline-col-header{padding:10px 12px;border-bottom:1px solid var(--border);display:flex;justify-content:space-between;align-items:center}
.pipeline-col-title{font-size:10px;text-transform:uppercase;letter-spacing:1.5px;font-weight:700}
.pipeline-col-count{font-size:11px;font-weight:700;padding:2px 8px;border-radius:20px;background:var(--bg-elevated)}
.pipeline-cards{padding:8px;display:flex;flex-direction:column;gap:7px;min-height:60px}
.pipeline-card{background:var(--bg-elevated);border:1px solid var(--border-light);border-radius:8px;padding:9px 11px;cursor:pointer;transition:border-color .2s}
.pipeline-card:hover{border-color:var(--primary)}
.pipeline-card-id{font-size:10px;color:var(--primary);font-weight:700;margin-bottom:3px}
.pipeline-card-company{font-size:12px;color:var(--text-primary);font-weight:500;margin-bottom:1px}
.pipeline-card-issue{font-size:10px;color:var(--text-muted)}
.pipeline-card-tech{font-size:10px;color:var(--text-secondary);margin-top:4px}
/* GOAL BAR */
.goal-bar-track{height:12px;background:var(--bg-base);border-radius:6px;overflow:hidden;margin:10px 0 6px}
.goal-bar-fill{height:100%;border-radius:6px;background:linear-gradient(90deg,var(--primary),var(--green));transition:width 1.2s ease}
.goal-bar-labels{display:flex;justify-content:space-between;font-size:11px;color:var(--text-muted)}
/* REVIEW */
.review-stats-grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(130px,1fr));gap:12px}
.review-stat{background:var(--bg-base);border-radius:8px;padding:14px;text-align:center;border:1px solid var(--border)}
.review-stat-val{font-family:var(--font-heading);font-size:28px;font-weight:600;color:var(--primary)}
.review-stat-label{font-size:11px;color:var(--text-muted);text-transform:uppercase;letter-spacing:1px;margin-top:3px}
.footer{max-width:1280px;margin:0 auto;padding:20px 28px;border-top:1px solid var(--border);display:flex;justify-content:space-between;flex-wrap:wrap;gap:8px}
.footer-text{font-size:11px;color:var(--text-dim);letter-spacing:.5px}
@media(max-width:800px){.grid-2,.grid-2-1{grid-template-columns:1fr}.stats-grid{grid-template-columns:repeat(2,1fr)}.pipeline-wrap{grid-template-columns:1fr}.detail-grid{grid-template-columns:1fr}}
@keyframes fadeIn{from{opacity:0}to{opacity:1}}
@keyframes slideUp{from{opacity:0;transform:translateY(20px)}to{opacity:1;transform:translateY(0)}}
.s1{animation:slideUp .4s ease .05s both}.s2{animation:slideUp .4s ease .1s both}.s3{animation:slideUp .4s ease .15s both}
.s4{animation:slideUp .4s ease .2s both}.s5{animation:slideUp .4s ease .25s both}.s6{animation:slideUp .4s ease .3s both}
</style>
</head>
<body>

<!-- JOB MODAL -->
<div class="modal-overlay" id="jobModal" onclick="if(event.target===this)closeModal()">
  <div class="modal">
    <div class="modal-top">
      <button class="modal-close" onclick="closeModal()">×</button>
      <div class="modal-job-id" id="modalJobId"></div>
      <div id="modalJobStatus"></div>
    </div>
    <div class="modal-body" id="modalBody"></div>
  </div>
</div>

<!-- LOGIN -->
<div id="loginScreen" class="login-wrapper diamond-bg">
  <div class="login-box">
    <div class="login-logo">
      <div class="login-logo-icon">DG</div>
      <div>
        <h1>Diesel <span>Growth</span></h1>
        <p class="login-subtitle">Client Command Center</p>
      </div>
    </div>
    <div class="login-field">
      <label>Business ID</label>
      <input type="text" id="inputUser" placeholder="Enter your business ID" autocomplete="off" onkeydown="if(event.key==='Enter')doLogin()">
    </div>
    <div class="login-field">
      <label>Password</label>
      <input type="password" id="inputPass" placeholder="Enter password" onkeydown="if(event.key==='Enter')doLogin()">
    </div>
    <button class="login-btn" id="loginBtn" onclick="doLogin()">Access Portal</button>
    <p class="login-error" id="loginError" style="color:var(--red);font-size:13px;margin-top:12px;text-align:center;display:none">Invalid credentials.</p>
    <p class="demo-hint">Demo: ID <span>lonestar</span> · Password <span>diesel2026</span></p>
  </div>
</div>

<!-- DASHBOARD -->
<div id="dashboard" class="dashboard">
  <div class="header diamond-bg">
    <div class="header-inner">
      <div class="header-left">
        <div class="header-logo">DG</div>
        <div>
          <div class="header-title">Diesel <span>Growth</span></div>
          <div class="header-label">Client Command Center</div>
        </div>
      </div>
      <div class="header-right">
        <div class="live-clock" id="liveClock"></div>
        <div class="live-indicator"><span class="live-dot"></span>Live</div>
        <div>
          <span class="header-client-name">Lone Star Diesel Repair</span><br>
          <span class="header-client-meta">Dallas, TX · Since March 2026</span>
        </div>
        <span class="badge-owner">OWNER</span>
        <button class="logout-btn" onclick="doLogout()">Sign Out</button>
      </div>
    </div>
  </div>
  <div class="nav-tabs">
    <button class="nav-tab active" data-tab="overview">Overview</button>
    <button class="nav-tab" data-tab="jobs">Jobs</button>
    <button class="nav-tab" data-tab="techs">Technicians</button>
    <button class="nav-tab" data-tab="insights">Insights</button>
  </div>
  <div class="main">

    <!-- OVERVIEW -->
    <div class="tab-content active" id="tab-overview">
      <div class="stats-grid">
        <div class="stat-card s1"><div class="stat-glow glow-up"></div><div class="stat-label">Calls Answered</div><div class="stat-value">347</div><div class="stat-change change-up">▲ 12% <span class="change-vs">vs last month</span></div></div>
        <div class="stat-card s2"><div class="stat-glow glow-up"></div><div class="stat-label">Jobs Closed</div><div class="stat-value">284</div><div class="stat-change change-up">▲ 18% <span class="change-vs">vs last month</span></div></div>
        <div class="stat-card s3"><div class="stat-glow glow-up"></div><div class="stat-label">Revenue Tracked</div><div class="stat-value">$62,840</div><div class="stat-change change-up">▲ 21% <span class="change-vs">vs last month</span></div></div>
        <div class="stat-card s4"><div class="stat-glow glow-down"></div><div class="stat-label">Avg Response</div><div class="stat-value">3.8 min</div><div class="stat-change change-down">▼ 22% <span class="change-vs">vs last month</span></div></div>
        <div class="stat-card s5"><div class="stat-glow glow-up"></div><div class="stat-label">Time Saved</div><div class="stat-value">87 hrs</div><div class="stat-change change-up">▲ 18% <span class="change-vs">vs last month</span></div></div>
        <div class="stat-card s6"><div class="stat-glow glow-up"></div><div class="stat-label">Close Rate</div><div class="stat-value">81.8%</div><div class="stat-change change-up">▲ 8% <span class="change-vs">vs last month</span></div></div>
      </div>
      <div class="stats-grid">
        <div class="stat-card s1"><div class="stat-glow glow-up"></div><div class="stat-label">Avg Job Value</div><div class="stat-value">$834</div><div class="stat-change change-up">▲ 9% <span class="change-vs">vs last month</span></div></div>
        <div class="stat-card s2"><div class="stat-glow glow-down"></div><div class="stat-label">Missed Calls</div><div class="stat-value">2</div><div class="stat-change change-down">▼ 75% <span class="change-vs">vs last month</span></div></div>
        <div class="stat-card s3"><div class="stat-glow glow-up"></div><div class="stat-label">Repeat Callers</div><div class="stat-value">38%</div><div class="stat-change change-up">▲ 14% <span class="change-vs">vs last month</span></div></div>
      </div>

      <!-- REVENUE GOAL -->
      <div class="panel">
        <div class="panel-header">
          <span class="panel-title">Monthly Revenue Goal</span>
          <span style="font-size:13px;color:var(--green);font-weight:700">Goal exceeded! 🎉</span>
        </div>
        <div style="display:flex;align-items:baseline;gap:8px;margin-bottom:8px">
          <span style="font-family:var(--font-heading);font-size:28px;font-weight:600;color:var(--green)">104%</span>
          <span style="font-size:13px;color:var(--text-muted)">of $60,000 monthly goal</span>
        </div>
        <div class="goal-bar-track"><div class="goal-bar-fill" style="width:100%;background:var(--green)"></div></div>
        <div class="goal-bar-labels"><span>$0</span><span>$30,000</span><span>$60,000</span></div>
      </div>

      <div class="grid-2-1" style="margin-bottom:20px">
        <div class="panel"><div class="panel-header"><span class="panel-title">Call Volume & Conversions</span></div><div class="chart-container"><canvas id="chartCalls"></canvas></div></div>
        <div class="panel"><div class="panel-header"><span class="panel-title">Lead Sources</span></div><div class="chart-container"><canvas id="chartSources"></canvas></div></div>
      </div>
      <div class="grid-2" style="margin-bottom:20px">
        <div class="panel"><div class="panel-header"><span class="panel-title">Response Time Trend</span></div><div class="chart-container"><canvas id="chartResponse"></canvas></div></div>
        <div class="panel"><div class="panel-header"><span class="panel-title">Revenue Tracked</span></div><div class="chart-container"><canvas id="chartRevenue"></canvas></div></div>
      </div>
      <div class="panel">
        <div class="panel-header"><span class="panel-title">Recent Jobs</span><button class="panel-action" onclick="switchTab('jobs')">View All →</button></div>
        <div class="table-wrap"><table><thead><tr><th>Job ID</th><th>Caller</th><th>Issue</th><th>Tech</th><th>Status</th></tr></thead><tbody id="recentJobsBody"></tbody></table></div>
      </div>
    </div>

    <!-- JOBS -->
    <div class="tab-content" id="tab-jobs">
      <div class="panel">
        <div class="panel-header">
          <span class="panel-title">Job Pipeline</span>
          <div style="display:flex;gap:8px;flex-wrap:wrap">
            <input id="jobSearch" placeholder="Search jobs..." oninput="filterJobs()" style="background:var(--bg-base);border:1px solid var(--border);border-radius:6px;padding:5px 12px;color:var(--text-primary);font-family:var(--font-body);font-size:12px;outline:none;width:160px">
            <select id="statusFilter" onchange="filterJobs()" style="background:var(--bg-base);border:1px solid var(--border);border-radius:6px;padding:5px 10px;color:var(--text-muted);font-family:var(--font-body);font-size:12px;outline:none;cursor:pointer">
              <option value="">All Status</option>
              <option>Pending</option><option>Assigned</option><option>En Route</option>
              <option>On Site</option><option>In Progress</option><option>Completed</option>
            </select>
            <button class="panel-action" onclick="exportCSV()">Export CSV</button>
            <button class="panel-action" id="toggleViewBtn" onclick="toggleView()">Table View</button>
          </div>
        </div>
        <div id="pipelineView"></div>
        <div id="tableView" style="display:none">
          <div class="table-wrap"><table><thead><tr><th>Job ID</th><th>Date</th><th>Caller</th><th>Issue</th><th>Tech</th><th>Revenue</th><th>Status</th></tr></thead><tbody id="allJobsBody"></tbody></table></div>
        </div>
      </div>
      <div class="grid-2">
        <div class="panel"><div class="panel-header"><span class="panel-title">Jobs by Type</span></div><div class="chart-container"><canvas id="chartJobTypes"></canvas></div></div>
        <div class="panel"><div class="panel-header"><span class="panel-title">Jobs by Day of Week</span></div><div class="chart-container"><canvas id="chartJobDays"></canvas></div></div>
      </div>
    </div>

    <!-- TECHS -->
    <div class="tab-content" id="tab-techs">
      <div class="panel">
        <div class="panel-header"><span class="panel-title">Technician Roster & Status</span></div>
        <div id="techRoster"></div>
      </div>
      <div class="panel"><div class="panel-header"><span class="panel-title">Tech Utilization This Month</span></div><div class="chart-container"><canvas id="chartTechUtil"></canvas></div></div>
    </div>

    <!-- INSIGHTS -->
    <div class="tab-content" id="tab-insights">
      <div class="panel">
        <div class="panel-header"><span class="panel-title">Review Generation</span></div>
        <div class="review-stats-grid">
          <div class="review-stat"><div class="review-stat-val">284</div><div class="review-stat-label">Requests Sent</div></div>
          <div class="review-stat"><div class="review-stat-val">198</div><div class="review-stat-label">Links Clicked</div></div>
          <div class="review-stat"><div class="review-stat-val">69%</div><div class="review-stat-label">Click Rate</div></div>
          <div class="review-stat"><div class="review-stat-val">143</div><div class="review-stat-label">Reviews Est.</div></div>
        </div>
        <p style="font-size:12px;color:var(--text-muted);margin-top:14px">Review requests fire automatically 1 hour after job is accepted. Reminder sends 3 days later if not clicked.</p>
      </div>
      <div class="panel"><div class="panel-header"><span class="panel-title">AI-Powered Insights</span></div><div id="insightsList"></div></div>
      <div class="panel">
        <div class="panel-header"><span class="panel-title">Monthly Scorecard</span></div>
        <table><thead><tr><th>Metric</th><th>Target</th><th>Actual</th><th>Status</th></tr></thead><tbody>
          <tr><td class="td-bold">Calls Answered</td><td class="td-dim">300</td><td class="td-bold">347</td><td><span class="badge badge-green">Hit</span></td></tr>
          <tr><td class="td-bold">Jobs Closed</td><td class="td-dim">250</td><td class="td-bold">284</td><td><span class="badge badge-green">Hit</span></td></tr>
          <tr><td class="td-bold">Avg Response</td><td class="td-dim">5 min</td><td class="td-bold">3.8 min</td><td><span class="badge badge-green">Hit</span></td></tr>
          <tr><td class="td-bold">Close Rate</td><td class="td-dim">75%</td><td class="td-bold">81.8%</td><td><span class="badge badge-green">Hit</span></td></tr>
          <tr><td class="td-bold">Revenue</td><td class="td-dim">$60,000</td><td class="td-bold">$62,840</td><td><span class="badge badge-green">Hit</span></td></tr>
          <tr><td class="td-bold">Missed Calls</td><td class="td-dim">5</td><td class="td-bold">2</td><td><span class="badge badge-green">Hit</span></td></tr>
        </tbody></table>
      </div>
    </div>

  </div>
  <div class="footer">
    <span class="footer-text">DIESEL GROWTH · CONFIDENTIAL CLIENT PORTAL · DEMO</span>
    <span class="footer-text" id="footerSync">Live sync · 284 jobs · Demo mode</span>
  </div>
</div>

<script>
// ── DEMO DATA ──────────────────────────────────────────
const DEMO_JOBS = [
  {id:'JOB-4821',date:'Apr 20, 2026',callerName:'Marcus Webb',callbackNumber:'+15107891234',company:'Western Freight Co.',fleetName:'WFC Express',truckType:'Kenworth T680',truckNumber:'WF-2241',issue:'DEF system fault',issueDetail:'DEF injector clogged, warning lights on dash, truck showing SCR fault code',urgency:'Urgent',driveable:'Limited',jobLocation:'I-10 E, Mile Marker 142, Houston TX',onsiteContact:'Marcus Webb',tech:'Carlos M.',response:'4 min',revenue:'$920',status:'Completed',notes:'DEF injector replaced on site. Cleared all fault codes. Truck back on the road.'},
  {id:'JOB-4820',date:'Apr 20, 2026',callerName:'Darnell Hughes',callbackNumber:'+15102345678',company:'Gulf Coast Carriers',fleetName:'GCC Fleet',truckType:'Peterbilt 579',truckNumber:'GCC-0089',issue:'Engine overheating',issueDetail:'Coolant leak from radiator hose, temp gauge in red zone',urgency:'Critical',driveable:'No',jobLocation:'US-59 S, near Rosenberg TX',onsiteContact:'Darnell Hughes',tech:'Danny R.',response:'6 min',revenue:'$1,240',status:'Completed',notes:'Radiator hose replaced. System flushed and refilled.'},
  {id:'JOB-4819',date:'Apr 20, 2026',callerName:'Roberto Castillo',callbackNumber:'+15103456789',company:'Tex-Mex Transport',fleetName:'TMT South',truckType:'Freightliner Cascadia',truckNumber:'TMT-0334',issue:'Air brake failure',issueDetail:'Air pressure dropping rapidly, brakes not holding',urgency:'Critical',driveable:'No',jobLocation:'SH-288 at Pearland TX',onsiteContact:'Roberto Castillo',tech:'James T.',response:'8 min',revenue:'$1,100',status:'En Route',notes:''},
  {id:'JOB-4818',date:'Apr 20, 2026',callerName:'Sandra Kim',callbackNumber:'+15104567890',company:'Lone Star Logistics',fleetName:'LSL Fleet A',truckType:'Volvo VNL 860',truckNumber:'LSL-0112',issue:'Electrical fault',issueDetail:'Multiple warning lights, truck losing power intermittently',urgency:'High',driveable:'Limited',jobLocation:'610 Loop N, Houston TX',onsiteContact:'Sandra Kim',tech:'Marcus W.',response:'5 min',revenue:'$750',status:'On Site',notes:''},
  {id:'JOB-4817',date:'Apr 19, 2026',callerName:'Travis Boudreaux',callbackNumber:'+15105678901',company:'Bayou Freight Lines',fleetName:'Bayou Express',truckType:'Mack Anthem',truckNumber:'BFL-0567',issue:'Transmission slipping',issueDetail:'Truck losing gears between 3rd and 4th, slipping under load',urgency:'High',driveable:'Yes',jobLocation:'I-45 N at Conroe TX',onsiteContact:'Travis Boudreaux',tech:'Carlos M.',response:'3 min',revenue:'$1,450',status:'Completed',notes:''},
  {id:'JOB-4816',date:'Apr 19, 2026',callerName:'Angela Torres',callbackNumber:'+15106789012',company:'Rio Grande Hauling',fleetName:'RGH Intermodal',truckType:'International LT',truckNumber:'RGH-0890',issue:'DPF regeneration failure',issueDetail:'DPF light solid, truck in derate mode, forced regen not working',urgency:'High',driveable:'Limited',jobLocation:'IH-10 W near Katy TX',onsiteContact:'Angela Torres',tech:'Danny R.',response:'7 min',revenue:'$880',status:'Completed',notes:''},
  {id:'JOB-4815',date:'Apr 19, 2026',callerName:'Jerome Washington',callbackNumber:'+15107890123',company:'Capital City Transport',fleetName:'CCT Fleet',truckType:'Kenworth W990',truckNumber:'CCT-0223',issue:'Fuel system issue',issueDetail:'Truck stalling under load, suspected fuel injector problem',urgency:'Urgent',driveable:'No',jobLocation:'SH-130 at Pflugerville TX',onsiteContact:'Jerome Washington',tech:'James T.',response:'9 min',revenue:'$1,680',status:'Completed',notes:''},
  {id:'JOB-4814',date:'Apr 19, 2026',callerName:'Priya Nair',callbackNumber:'+15108901234',company:'Sunrise Logistics',fleetName:'SL West',truckType:'Freightliner Cascadia',truckNumber:'SL-0445',issue:'EGR valve failure',issueDetail:'EGR fault codes, truck in limp mode, reduced power',urgency:'Medium',driveable:'Limited',jobLocation:'IH-35 N near Round Rock TX',onsiteContact:'Priya Nair',tech:'Marcus W.',response:'5 min',revenue:'$960',status:'Completed',notes:''},
  {id:'JOB-4813',date:'Apr 18, 2026',callerName:'Duke Patterson',callbackNumber:'+15109012345',company:'Panhandle Freight',fleetName:'PHF North',truckType:'Peterbilt 389',truckNumber:'PHF-0667',issue:'Coolant system leak',issueDetail:'Water pump failing, steam from engine bay',urgency:'Critical',driveable:'No',jobLocation:'US-287 near Amarillo TX',onsiteContact:'Duke Patterson',tech:'Chris R.',response:'4 min',revenue:'$1,120',status:'Completed',notes:''},
  {id:'JOB-4812',date:'Apr 18, 2026',callerName:'Maria Gonzalez',callbackNumber:'+15100123456',company:'South Texas Express',fleetName:'STE Fleet B',truckType:'Volvo VNL 760',truckNumber:'STE-0334',issue:'Turbocharger failure',issueDetail:'Loss of boost pressure, black smoke, turbo noise',urgency:'High',driveable:'Limited',jobLocation:'IH-37 S near Corpus Christi TX',onsiteContact:'Maria Gonzalez',tech:'Danny R.',response:'6 min',revenue:'$2,100',status:'Completed',notes:''},
  {id:'JOB-4811',date:'Apr 18, 2026',callerName:'Bobby Chen',callbackNumber:'+15101234560',company:'Pacific Rim Haulers',fleetName:'PRH West Coast',truckType:'Kenworth T880',truckNumber:'PRH-0112',issue:'Fifth wheel issue',issueDetail:'Trailer coupling not locking, safety concern',urgency:'Urgent',driveable:'No',jobLocation:'I-20 W near Odessa TX',onsiteContact:'Bobby Chen',tech:'Carlos M.',response:'5 min',revenue:'$640',status:'Completed',notes:''},
  {id:'JOB-4810',date:'Apr 18, 2026',callerName:'Lisa Morales',callbackNumber:'+15102345601',company:'Border Express LLC',fleetName:'BE South',truckType:'International 9900i',truckNumber:'BE-0778',issue:'Suspension failure',issueDetail:'Air bag blown, truck sitting low on right side',urgency:'High',driveable:'No',jobLocation:'US-83 near Laredo TX',onsiteContact:'Lisa Morales',tech:'James T.',response:'7 min',revenue:'$890',status:'Completed',notes:''},
  {id:'JOB-4809',date:'Apr 17, 2026',callerName:'Tyrone Jackson',callbackNumber:'+15103456012',company:'Magnolia Freight',fleetName:'MF Central',truckType:'Mack Pinnacle',truckNumber:'MF-0556',issue:'Check engine light',issueDetail:'Multiple fault codes, truck running rough',urgency:'Medium',driveable:'Yes',jobLocation:'SH-6 near Bryan TX',onsiteContact:'Tyrone Jackson',tech:'Marcus W.',response:'4 min',revenue:'$720',status:'Completed',notes:''},
  {id:'JOB-4808',date:'Apr 17, 2026',callerName:'Rachel Foster',callbackNumber:'+15104560123',company:'Foster Family Trucking',fleetName:'FFT Owner Ops',truckType:'Peterbilt 567',truckNumber:'FFT-0001',issue:'DEF dosing unit',issueDetail:'DEF quality fault, truck going into derate',urgency:'High',driveable:'Limited',jobLocation:'IH-20 E near Abilene TX',onsiteContact:'Rachel Foster',tech:'Chris R.',response:'3 min',revenue:'$840',status:'Completed',notes:''},
  {id:'JOB-4807',date:'Apr 17, 2026',callerName:'Carlos Rivera',callbackNumber:'+15105601234',company:'Central Texas Carriers',fleetName:'CTC Express',truckType:'Freightliner Cascadia',truckNumber:'CTC-0890',issue:'Power steering failure',issueDetail:'Steering very stiff, power steering fluid leaking',urgency:'Critical',driveable:'No',jobLocation:'US-183 near Austin TX',onsiteContact:'Carlos Rivera',tech:'Danny R.',response:'8 min',revenue:'$980',status:'Completed',notes:''},
  {id:'JOB-4806',date:'Apr 17, 2026',callerName:'Wendy Brooks',callbackNumber:'+15106012345',company:'Bluebell Transport',fleetName:'BT Reefer',truckType:'Kenworth T270',truckNumber:'BT-0334',issue:'Reefer unit failure',issueDetail:'Refrigeration unit not maintaining temp, load at risk',urgency:'Critical',driveable:'Yes',jobLocation:'IH-10 E near Beaumont TX',onsiteContact:'Wendy Brooks',tech:'Carlos M.',response:'5 min',revenue:'$1,340',status:'Completed',notes:''},
  {id:'JOB-4805',date:'Apr 17, 2026',callerName:'Kevin Murphy',callbackNumber:'+15107123456',company:'Murphy Intermodal',fleetName:'MI Fleet',truckType:'International LT',truckNumber:'MI-0667',issue:'EGR cooler leak',issueDetail:'White smoke from exhaust, EGR coolant mixing',urgency:'Urgent',driveable:'No',jobLocation:'SH-225 near Pasadena TX',onsiteContact:'Kevin Murphy',tech:'James T.',response:'6 min',revenue:'$1,560',status:'Completed',notes:''},
  {id:'JOB-4804',date:'Apr 16, 2026',callerName:'Ashley Davis',callbackNumber:'+15108234567',company:'Davis Dedicated',fleetName:'DD Fleet A',truckType:'Volvo VNL 860',truckNumber:'DD-0112',issue:'Alternator failure',issueDetail:'Battery light on, truck losing electrical power',urgency:'High',driveable:'Limited',jobLocation:'US-77 near Victoria TX',onsiteContact:'Ashley Davis',tech:'Marcus W.',response:'4 min',revenue:'$780',status:'Completed',notes:''},
  {id:'JOB-4803',date:'Apr 16, 2026',callerName:'Steven Park',callbackNumber:'+15109345678',company:'Lone Star Diesel Repair',fleetName:'Test Fleet',truckType:'Peterbilt 579',truckNumber:'TEST-001',issue:'Scheduled maintenance',issueDetail:'30,000 mile service — oil, filters, DEF fluid, safety check',urgency:'Low',driveable:'Yes',jobLocation:'1234 Industrial Blvd, Dallas TX',onsiteContact:'Steven Park',tech:'Chris R.',response:'N/A',revenue:'$420',status:'Completed',notes:'All service items completed. Next service at 60,000 miles.'},
  {id:'JOB-4802',date:'Apr 16, 2026',callerName:'Diana Prince',callbackNumber:'+15100456789',company:'Olympus Freight',fleetName:'OF Express',truckType:'Kenworth T680',truckNumber:'OF-0334',issue:'Brake adjustment',issueDetail:'Brake pads worn, pulling to right on braking',urgency:'Medium',driveable:'Yes',jobLocation:'IH-35 S near San Antonio TX',onsiteContact:'Diana Prince',tech:'Danny R.',response:'5 min',revenue:'$560',status:'Completed',notes:''},
  {id:'JOB-4801',date:'Apr 20, 2026',callerName:'Nathan Cross',callbackNumber:'+15101567890',company:'Cross Country Carriers',fleetName:'CCC Fleet',truckType:'Mack Anthem',truckNumber:'CCC-0889',issue:'Fuel leak',issueDetail:'Diesel dripping from fuel line near injection pump',urgency:'Critical',driveable:'No',jobLocation:'I-40 W near Amarillo TX',onsiteContact:'Nathan Cross',tech:'—',response:'—',revenue:'—',status:'Pending',notes:''},
];

const DEMO_TECHS = [
  {name:'Carlos M.',specialty:'DEF / Aftertreatment Specialist',on_duty:true,priority:1,login_id:'carlos.m',jobs:89,avgResp:'3.4 min',acceptRate:'97%',revenue:'$21,400'},
  {name:'Danny R.',specialty:'Engine Diagnostics & Repair',on_duty:true,priority:2,login_id:'danny.r',jobs:74,avgResp:'4.1 min',acceptRate:'94%',revenue:'$18,200'},
  {name:'James T.',specialty:'Electrical / Fuel Systems',on_duty:true,priority:3,login_id:'james.t',jobs:62,avgResp:'5.2 min',acceptRate:'91%',revenue:'$14,800'},
  {name:'Marcus W.',specialty:'Brake / Suspension / Air Systems',on_duty:false,priority:4,login_id:'marcus.w',jobs:41,avgResp:'4.8 min',acceptRate:'89%',revenue:'$9,600'},
  {name:'Chris R.',specialty:'Mobile Diesel Repair',on_duty:true,priority:5,login_id:'chris.r',jobs:38,avgResp:'4.5 min',acceptRate:'92%',revenue:'$8,840'},
];

let viewMode = 'pipeline', charts = {};

// ── LOGIN ──────────────────────────────────────────────
function doLogin() {
  const user = document.getElementById('inputUser').value.trim().toLowerCase();
  const pass = document.getElementById('inputPass').value;
  const err = document.getElementById('loginError');
  if(user === 'lonestar' && pass === 'diesel2026') {
    document.getElementById('loginScreen').style.display = 'none';
    document.getElementById('dashboard').classList.add('active');
    startClock();
    initDashboard();
  } else {
    err.style.display = 'block';
    err.textContent = 'Invalid credentials. Try: lonestar / diesel2026';
  }
}

function doLogout() {
  document.getElementById('dashboard').classList.remove('active');
  document.getElementById('loginScreen').style.display = 'flex';
  document.getElementById('inputUser').value = '';
  document.getElementById('inputPass').value = '';
  document.getElementById('loginError').style.display = 'none';
  Object.values(charts).forEach(c=>c.destroy()); charts={};
  document.querySelectorAll('.tab-content').forEach(t=>t.classList.remove('active'));
  document.getElementById('tab-overview').classList.add('active');
  document.querySelectorAll('.nav-tab').forEach(t=>t.classList.remove('active'));
  document.querySelector('.nav-tab[data-tab="overview"]').classList.add('active');
}

// ── CLOCK ──────────────────────────────────────────────
function startClock() {
  const tick = () => {
    const el = document.getElementById('liveClock');
    if(!el) return;
    const now = new Date();
    el.textContent = now.toLocaleDateString('en-US',{weekday:'short',month:'short',day:'numeric'}) + ' · ' + now.toLocaleTimeString('en-US',{hour:'numeric',minute:'2-digit',second:'2-digit'});
  };
  tick(); setInterval(tick, 1000);
}

// ── NAV ────────────────────────────────────────────────
document.addEventListener('DOMContentLoaded', () => {
  document.querySelector('.nav-tabs').addEventListener('click', e => {
    if(e.target.classList.contains('nav-tab')) switchTab(e.target.dataset.tab);
  });
});

function switchTab(t) {
  document.querySelectorAll('.nav-tab').forEach(n=>n.classList.remove('active'));
  document.querySelector(`.nav-tab[data-tab="${t}"]`)?.classList.add('active');
  document.querySelectorAll('.tab-content').forEach(n=>n.classList.remove('active'));
  document.getElementById('tab-'+t)?.classList.add('active');
}

// ── INIT ───────────────────────────────────────────────
function initDashboard() {
  renderRecentJobs();
  renderPipeline(DEMO_JOBS);
  renderAllJobsTable(DEMO_JOBS);
  renderTechs();
  renderInsights();
  buildCharts();
}

// ── RECENT JOBS ────────────────────────────────────────
function renderRecentJobs() {
  document.getElementById('recentJobsBody').innerHTML = DEMO_JOBS.slice(0,6).map(j =>
    `<tr class="clickable-row" onclick="openModal('${j.id}')">
      <td class="td-id">${j.id}</td>
      <td class="td-bold">${j.callerName}</td>
      <td class="td-dim">${j.issue}</td>
      <td>${j.tech}</td>
      <td>${badge(j.status)}</td>
    </tr>`).join('');
}

// ── PIPELINE ───────────────────────────────────────────
const COLS = [
  {key:'Pending',color:'var(--text-muted)'},
  {key:'Assigned',color:'var(--primary)'},
  {key:'En Route',color:'var(--blue)'},
  {key:'On Site',color:'#c084fc'},
  {key:'In Progress',color:'var(--yellow)'},
  {key:'Completed',color:'var(--green)'},
];

function renderPipeline(jobs) {
  const grouped = {};
  COLS.forEach(c => grouped[c.key] = []);
  jobs.forEach(j => { if(grouped[j.status]) grouped[j.status].push(j); });
  document.getElementById('pipelineView').innerHTML = `<div class="pipeline-wrap">${COLS.map(col => {
    const cards = grouped[col.key] || [];
    return `<div class="pipeline-col">
      <div class="pipeline-col-header">
        <span class="pipeline-col-title" style="color:${col.color}">${col.key}</span>
        <span class="pipeline-col-count" style="color:${col.color}">${cards.length}</span>
      </div>
      <div class="pipeline-cards">
        ${cards.length ? cards.slice(0,8).map(j=>`<div class="pipeline-card" onclick="openModal('${j.id}')">
          <div class="pipeline-card-id">${j.id}</div>
          <div class="pipeline-card-company">${j.company}</div>
          <div class="pipeline-card-issue">${j.issue}</div>
          <div class="pipeline-card-tech">${j.tech !== '—' ? 'Tech: '+j.tech : ''}</div>
        </div>`).join('') : '<div style="font-size:11px;color:var(--text-dim);text-align:center;padding:12px 0">Empty</div>'}
      </div>
    </div>`;
  }).join('')}</div>`;
}

function renderAllJobsTable(jobs) {
  document.getElementById('allJobsBody').innerHTML = jobs.map(j =>
    `<tr class="clickable-row" onclick="openModal('${j.id}')">
      <td class="td-id">${j.id}</td>
      <td class="td-dim">${j.date}</td>
      <td class="td-bold">${j.company}</td>
      <td class="td-dim">${j.issue}</td>
      <td>${j.tech}</td>
      <td class="td-bold">${j.revenue}</td>
      <td>${badge(j.status)}</td>
    </tr>`).join('');
}

function filterJobs() {
  const search = (document.getElementById('jobSearch')?.value||'').toLowerCase();
  const status = document.getElementById('statusFilter')?.value||'';
  const filtered = DEMO_JOBS.filter(j => {
    const ms = !search || (j.id+j.callerName+j.company+j.issue+j.tech).toLowerCase().includes(search);
    const mst = !status || j.status === status;
    return ms && mst;
  });
  if(viewMode === 'pipeline') renderPipeline(filtered);
  else renderAllJobsTable(filtered);
}

function toggleView() {
  const btn = document.getElementById('toggleViewBtn');
  const pv = document.getElementById('pipelineView');
  const tv = document.getElementById('tableView');
  if(viewMode === 'pipeline') {
    viewMode = 'table'; pv.style.display = 'none'; tv.style.display = 'block'; btn.textContent = 'Pipeline View';
  } else {
    viewMode = 'pipeline'; pv.style.display = 'block'; tv.style.display = 'none'; btn.textContent = 'Table View';
  }
}

function exportCSV() {
  const headers = ['Job ID','Date','Caller','Company','Issue','Urgency','Tech','Status','Revenue'];
  const rows = DEMO_JOBS.map(j => [j.id,j.date,j.callerName,j.company,j.issue,j.urgency,j.tech,j.status,j.revenue].map(v=>'"'+String(v||'').replace(/"/g,"''")+'"').join(','));
  const csv = [headers.join(','),...rows].join('\n');
  const blob = new Blob([csv],{type:'text/csv'});
  const url = URL.createObjectURL(blob);
  const a = document.createElement('a');
  a.href=url; a.download='diesel-growth-demo-jobs.csv';
  document.body.appendChild(a); a.click(); document.body.removeChild(a);
  URL.revokeObjectURL(url);
}

// ── TECHS ──────────────────────────────────────────────
function renderTechs() {
  document.getElementById('techRoster').innerHTML = DEMO_TECHS.map(t => `
    <div class="tech-card">
      <div style="display:flex;justify-content:space-between;align-items:flex-start;flex-wrap:wrap;gap:10px">
        <div>
          <div class="tech-name">
            ${t.name}
            <span class="duty-badge ${t.on_duty?'duty-on':'duty-off'}" style="margin-left:8px;font-size:11px">
              <span class="duty-dot ${t.on_duty?'dot-on':'dot-off'}"></span>
              ${t.on_duty?'ON DUTY':'OFF DUTY'}
            </span>
          </div>
          <div class="tech-meta">${t.specialty} · Avg response: ${t.avgResp} · Accept rate: ${t.acceptRate} · Login: ${t.login_id}</div>
        </div>
        <div style="display:flex;gap:20px;text-align:right">
          <div><div class="tech-stat-num">${t.jobs}</div><div class="tech-stat-label">Jobs this month</div></div>
          <div><div class="tech-stat-num">${t.revenue}</div><div class="tech-stat-label">Revenue</div></div>
        </div>
      </div>
    </div>`).join('');
}

// ── INSIGHTS ───────────────────────────────────────────
function renderInsights() {
  const insights = [
    {icon:'🔥', text:'DEF system jobs are your #1 issue type this month at 31% of all calls — Carlos M. handles them fastest at 3.4 min avg response.'},
    {icon:'📞', text:'38% of callers this month are repeat customers — up from 24% last month. Strong sign clients trust the service.'},
    {icon:'💰', text:'Average job value hit $834 this month — up 9% vs last month. Turbocharger and fuel system jobs are driving the increase.'},
    {icon:'⚡', text:'Response time dropped to 3.8 min avg — down from 4.9 min last month. The AI dispatch system is working.'},
    {icon:'📈', text:'Job volume up 18% vs last month with only 2 missed calls — the AI receptionist is capturing nearly every inbound call.'},
    {icon:'🌟', text:'You exceeded your $60,000 revenue goal by 4.7% — first month hitting the goal since launch.'},
  ];
  document.getElementById('insightsList').innerHTML = insights.map(i =>
    `<div class="insight-card"><span class="insight-icon">${i.icon}</span><span class="insight-text">${i.text}</span></div>`).join('');
}

// ── JOB MODAL ──────────────────────────────────────────
function openModal(jobId) {
  const job = DEMO_JOBS.find(j => j.id === jobId);
  if(!job) return;
  document.getElementById('modalJobId').innerHTML = `<span>${job.id}</span> — ${job.date}`;
  document.getElementById('modalJobStatus').innerHTML = badge(job.status);
  const urgClass = (job.urgency||'').toLowerCase().includes('critical')||(job.urgency||'').toLowerCase().includes('urgent') ? 'urgent' : 'low';
  const notDrive = job.driveable === 'No';
  let html = `
    <div class="detail-section"><div class="detail-section-title">Caller Information</div><div class="detail-grid">
      <div class="detail-row"><div class="detail-label">Caller Name</div><div class="detail-value">${job.callerName}</div></div>
      <div class="detail-row"><div class="detail-label">Callback</div><div class="detail-value">${job.callbackNumber}</div></div>
      <div class="detail-row"><div class="detail-label">Company</div><div class="detail-value">${job.company}</div></div>
      <div class="detail-row"><div class="detail-label">Fleet</div><div class="detail-value">${job.fleetName}</div></div>
    </div></div>
    <div class="detail-section"><div class="detail-section-title">Truck Information</div><div class="detail-grid">
      <div class="detail-row"><div class="detail-label">Truck Type</div><div class="detail-value">${job.truckType}</div></div>
      <div class="detail-row"><div class="detail-label">Unit Number</div><div class="detail-value">${job.truckNumber}</div></div>
    </div></div>
    <div class="detail-section"><div class="detail-section-title">Issue Details</div><div class="detail-grid">
      <div class="detail-row"><div class="detail-label">Urgency</div><div class="detail-value ${urgClass}">${job.urgency}</div></div>
      <div class="detail-row"><div class="detail-label">Driveable</div><div class="detail-value ${notDrive?'not-driveable':'driveable'}">${notDrive?'NO — Truck cannot move':'YES — Truck can move'}</div></div>
      <div class="detail-row full"><div class="detail-label">Issue Summary</div><div class="detail-value">${job.issue}</div></div>
      <div class="detail-row full"><div class="detail-label">Full Description</div><div class="detail-value" style="color:var(--text-secondary);font-weight:400">${job.issueDetail}</div></div>
    </div></div>
    <div class="detail-section"><div class="detail-section-title">Location & Contact</div><div class="detail-grid">
      <div class="detail-row full"><div class="detail-label">Job Location</div><div class="detail-value">${job.jobLocation} <a href="https://maps.google.com/?q=${encodeURIComponent(job.jobLocation)}" target="_blank" style="color:var(--primary);font-size:11px;margin-left:6px">Open Maps ↗</a></div></div>
      <div class="detail-row"><div class="detail-label">On-Site Contact</div><div class="detail-value">${job.onsiteContact}</div></div>
      <div class="detail-row"><div class="detail-label">Date</div><div class="detail-value">${job.date}</div></div>
    </div></div>
    <div class="detail-section"><div class="detail-section-title">Dispatch Details</div><div class="detail-grid">
      <div class="detail-row"><div class="detail-label">Tech Assigned</div><div class="detail-value">${job.tech}</div></div>
      <div class="detail-row"><div class="detail-label">Status</div><div class="detail-value" style="margin-top:4px">${badge(job.status)}</div></div>
      <div class="detail-row"><div class="detail-label">Response Time</div><div class="detail-value">${job.response}</div></div>
      <div class="detail-row"><div class="detail-label">Revenue</div><div class="detail-value" style="color:var(--green)">${job.revenue}</div></div>
    </div></div>`;
  if(job.notes) html += `<div class="detail-section"><div class="detail-section-title">Tech Notes</div><div class="modal-note">${job.notes}</div></div>`;
  document.getElementById('modalBody').innerHTML = html;
  document.getElementById('jobModal').classList.add('open');
  document.body.style.overflow = 'hidden';
}

function closeModal() {
  document.getElementById('jobModal').classList.remove('open');
  document.body.style.overflow = '';
}

document.addEventListener('keydown', e => { if(e.key === 'Escape') closeModal(); });

// ── BADGE ──────────────────────────────────────────────
function badge(st) {
  const m = {Completed:'badge-green',Assigned:'badge-green','En Route':'badge-blue','On Site':'badge-purple','In Progress':'badge-orange',Pending:'badge-gray'};
  return `<span class="badge ${m[st]||'badge-gray'}">${st}</span>`;
}

// ── CHARTS ─────────────────────────────────────────────
function buildCharts() {
  const cDef = {responsive:true,maintainAspectRatio:false,plugins:{legend:{display:false}},scales:{x:{grid:{color:'#1e1e1e'},ticks:{color:'#555',font:{family:'Barlow Condensed',size:12}}},y:{grid:{color:'#1e1e1e'},ticks:{color:'#555',font:{family:'Barlow Condensed',size:12}}}}};
  const wk = ['Week 1','Week 2','Week 3','Week 4'];

  charts.calls = new Chart(document.getElementById('chartCalls'), {type:'bar',data:{labels:wk,datasets:[
    {label:'Calls',data:[72,88,97,90],backgroundColor:'rgba(249,115,22,0.7)',borderRadius:4},
    {label:'Closed',data:[58,72,84,70],backgroundColor:'rgba(34,197,94,0.6)',borderRadius:4},
  ]},options:{...cDef,plugins:{legend:{display:true,labels:{color:'#888',font:{family:'Barlow Condensed',size:11},boxWidth:10}}}}});

  charts.sources = new Chart(document.getElementById('chartSources'), {type:'doughnut',data:{labels:['Google','Repeat','Referral','Direct'],datasets:[{data:[45,32,15,8],backgroundColor:['#F97316','#FB923C','#FDBA74','#FED7AA'],borderWidth:0}]},options:{responsive:true,maintainAspectRatio:false,plugins:{legend:{position:'bottom',labels:{color:'#888',font:{family:'Barlow Condensed',size:11},boxWidth:10,padding:12}}}}});

  charts.response = new Chart(document.getElementById('chartResponse'), {type:'line',data:{labels:wk,datasets:[{label:'Avg Min',data:[5.2,4.6,3.9,3.8],borderColor:'#F97316',backgroundColor:'rgba(249,115,22,0.08)',fill:true,tension:.35,pointRadius:5,pointBackgroundColor:'#F97316'}]},options:cDef});

  charts.revenue = new Chart(document.getElementById('chartRevenue'), {type:'bar',data:{labels:wk,datasets:[{label:'Revenue',data:[13200,16800,18440,14400],backgroundColor:'rgba(34,197,94,0.6)',borderRadius:4}]},options:{...cDef,scales:{...cDef.scales,y:{...cDef.scales.y,ticks:{...cDef.scales.y.ticks,callback:v=>'$'+v.toLocaleString()}}}}});

  charts.jobTypes = new Chart(document.getElementById('chartJobTypes'), {type:'bar',data:{labels:['DEF System','Engine Diag','Electrical','Aftertreatment','Brake/Air','Coolant','Other'],datasets:[{data:[88,61,48,42,31,8,6],backgroundColor:'rgba(249,115,22,0.6)',borderRadius:4}]},options:{...cDef,indexAxis:'y'}});

  charts.jobDays = new Chart(document.getElementById('chartJobDays'), {type:'bar',data:{labels:['Mon','Tue','Wed','Thu','Fri','Sat','Sun'],datasets:[{data:[54,48,42,46,38,20,12],backgroundColor:'rgba(251,146,60,0.5)',borderRadius:4}]},options:cDef});

  charts.techUtil = new Chart(document.getElementById('chartTechUtil'), {type:'bar',data:{labels:['Carlos M.','Danny R.','James T.','Marcus W.','Chris R.'],datasets:[{label:'Jobs',data:[89,74,62,41,38],backgroundColor:['rgba(249,115,22,0.8)','rgba(34,197,94,0.7)','rgba(34,197,94,0.6)','rgba(234,179,8,0.5)','rgba(34,197,94,0.6)'],borderRadius:4}]},options:cDef});
}
</script>
</body>
</html>
