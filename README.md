HR Attrition
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>HR Attrition Analysis</title>
<link href="https://fonts.googleapis.com/css2?family=Syne:wght@400;500;600;700;800&family=DM+Sans:wght@300;400;500&display=swap" rel="stylesheet">
<script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/4.4.1/chart.umd.js"></script>
<style>
  *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

  :root {
    --bg: #0a0a0a;
    --surface: #111111;
    --surface2: #181818;
    --surface3: #202020;
    --border: rgba(255,255,255,0.06);
    --border2: rgba(255,255,255,0.1);
    --text: #ffffff;
    --text-secondary: rgba(255,255,255,0.45);
    --text-tertiary: rgba(255,255,255,0.25);
    --green: #4ade80;
    --green-dim: rgba(74,222,128,0.12);
    --red: #f87171;
    --red-dim: rgba(248,113,113,0.12);
    --amber: #fbbf24;
    --amber-dim: rgba(251,191,36,0.1);
    --accent: #4ade80;
    --bar-red: #c0392b;
    --bar-red2: #e74c3c;
  }

  html { scroll-behavior: smooth; }

  body {
    background: var(--bg);
    color: var(--text);
    font-family: 'DM Sans', sans-serif;
    font-size: 14px;
    line-height: 1.5;
    min-height: 100vh;
  }

  /* HEADER */
  .header {
    padding: 48px 48px 0;
    border-bottom: 1px solid var(--border);
    background: var(--bg);
  }
  .header-top {
    display: flex;
    justify-content: space-between;
    align-items: flex-start;
    margin-bottom: 32px;
  }
  .header-eyebrow {
    font-size: 11px;
    letter-spacing: .12em;
    text-transform: uppercase;
    color: var(--text-tertiary);
    margin-bottom: 8px;
  }
  .header-title {
    font-family: 'Syne', sans-serif;
    font-size: 36px;
    font-weight: 800;
    color: var(--text);
    letter-spacing: -.02em;
    line-height: 1.1;
  }
  .header-title span { color: var(--green); }
  .header-sub {
    color: var(--text-secondary);
    font-size: 13px;
    margin-top: 6px;
  }
  .badge {
    background: var(--green-dim);
    color: var(--green);
    font-size: 11px;
    font-weight: 500;
    padding: 4px 10px;
    border-radius: 20px;
    border: 1px solid rgba(74,222,128,0.2);
    letter-spacing: .04em;
  }
  .badge.red {
    background: var(--red-dim);
    color: var(--red);
    border-color: rgba(248,113,113,0.2);
  }

  /* NAV TABS */
  .nav-tabs {
    display: flex;
    gap: 0;
    overflow-x: auto;
  }
  .nav-tab {
    padding: 14px 20px;
    font-size: 12px;
    font-weight: 500;
    color: var(--text-tertiary);
    cursor: pointer;
    border-bottom: 2px solid transparent;
    white-space: nowrap;
    transition: all .2s;
    letter-spacing: .02em;
    background: none;
    border-left: none;
    border-right: none;
    border-top: none;
    outline: none;
  }
  .nav-tab:hover { color: var(--text-secondary); }
  .nav-tab.active { color: var(--green); border-bottom-color: var(--green); }

  /* CONTENT */
  .content { padding: 32px 48px 64px; }

  /* OVERVIEW CARDS */
  .kpi-grid {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 12px;
    margin-bottom: 32px;
  }
  .kpi-card {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 12px;
    padding: 20px 22px;
    transition: border-color .2s;
  }
  .kpi-card:hover { border-color: var(--border2); }
  .kpi-label {
    font-size: 11px;
    color: var(--text-tertiary);
    letter-spacing: .06em;
    text-transform: uppercase;
    margin-bottom: 8px;
  }
  .kpi-value {
    font-family: 'Syne', sans-serif;
    font-size: 28px;
    font-weight: 700;
    color: var(--text);
    letter-spacing: -.02em;
    line-height: 1;
  }
  .kpi-delta {
    margin-top: 6px;
    font-size: 12px;
    display: flex;
    align-items: center;
    gap: 4px;
  }
  .kpi-delta.up { color: var(--red); }
  .kpi-delta.down { color: var(--green); }
  .kpi-delta.neutral { color: var(--text-tertiary); }

  /* SECTION PAGES */
  .page { display: none; }
  .page.active { display: block; }

  /* SECTION LAYOUT */
  .section-header {
    margin-bottom: 24px;
  }
  .section-q {
    font-size: 11px;
    color: var(--green);
    letter-spacing: .1em;
    text-transform: uppercase;
    margin-bottom: 6px;
    font-weight: 500;
  }
  .section-title {
    font-family: 'Syne', sans-serif;
    font-size: 22px;
    font-weight: 700;
    color: var(--text);
    letter-spacing: -.02em;
  }
  .section-sub {
    color: var(--text-secondary);
    font-size: 13px;
    margin-top: 4px;
  }

  /* TWO-COL LAYOUT */
  .two-col {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 16px;
    margin-bottom: 16px;
  }
  .three-col {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 16px;
    margin-bottom: 16px;
  }

  /* CHART CARDS */
  .card {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 12px;
    padding: 22px 24px;
    transition: border-color .2s;
  }
  .card:hover { border-color: var(--border2); }
  .card-label {
    font-size: 11px;
    color: var(--text-tertiary);
    letter-spacing: .06em;
    text-transform: uppercase;
    margin-bottom: 4px;
  }
  .card-value {
    font-family: 'Syne', sans-serif;
    font-size: 32px;
    font-weight: 700;
    color: var(--text);
    letter-spacing: -.02em;
    line-height: 1.1;
  }
  .card-value.red { color: var(--red); }
  .card-value.green { color: var(--green); }
  .card-sub {
    font-size: 12px;
    color: var(--text-secondary);
    margin-top: 4px;
  }
  .card-sub.red { color: var(--red); }
  .card-sub.green { color: var(--green); }
  .card-title {
    font-size: 13px;
    font-weight: 500;
    color: var(--text);
    margin-bottom: 16px;
  }
  .chart-container { position: relative; width: 100%; }

  /* INSIGHT BOX */
  .insight-box {
    background: var(--surface2);
    border: 1px solid var(--border);
    border-left: 3px solid var(--green);
    border-radius: 0 10px 10px 0;
    padding: 16px 20px;
    margin-top: 16px;
    font-size: 13px;
    color: var(--text-secondary);
    line-height: 1.6;
  }
  .insight-box strong { color: var(--text); font-weight: 500; }
  .insight-box.red { border-left-color: var(--red); }
  .insight-box.amber { border-left-color: var(--amber); }

  /* HORIZONTAL BAR */
  .hbar-row {
    display: flex;
    align-items: center;
    gap: 12px;
    margin-bottom: 10px;
  }
  .hbar-label {
    font-size: 12px;
    color: var(--text-secondary);
    width: 90px;
    flex-shrink: 0;
    text-align: right;
  }
  .hbar-track {
    flex: 1;
    background: var(--surface3);
    border-radius: 3px;
    height: 24px;
    position: relative;
    overflow: hidden;
  }
  .hbar-fill {
    height: 100%;
    border-radius: 3px;
    transition: width 1s cubic-bezier(.16,1,.3,1);
    display: flex;
    align-items: center;
    padding-left: 8px;
  }
  .hbar-pct {
    font-size: 11px;
    font-weight: 500;
    color: rgba(255,255,255,0.9);
  }
  .hbar-num {
    font-size: 12px;
    color: var(--text-secondary);
    width: 38px;
    flex-shrink: 0;
    font-weight: 500;
  }

  /* RISK CARDS */
  .risk-card {
    background: var(--surface2);
    border: 1px solid var(--border);
    border-radius: 10px;
    padding: 16px 18px;
    text-align: left;
  }
  .risk-card-icon {
    font-size: 18px;
    margin-bottom: 10px;
  }
  .risk-card-title {
    font-size: 12px;
    font-weight: 500;
    color: var(--text);
    margin-bottom: 4px;
  }
  .risk-card-desc {
    font-size: 11px;
    color: var(--text-secondary);
    line-height: 1.5;
    margin-bottom: 10px;
  }
  .risk-card-impact {
    font-family: 'Syne', sans-serif;
    font-size: 18px;
    font-weight: 700;
    color: var(--green);
  }
  .risk-card-impact-label {
    font-size: 10px;
    color: var(--text-tertiary);
    letter-spacing: .04em;
    text-transform: uppercase;
  }

  /* STAT ROW */
  .stat-row {
    display: flex;
    gap: 16px;
    margin-bottom: 20px;
  }
  .stat-chip {
    background: var(--surface2);
    border: 1px solid var(--border);
    border-radius: 8px;
    padding: 10px 16px;
    flex: 1;
  }
  .stat-chip-label { font-size: 11px; color: var(--text-tertiary); margin-bottom: 3px; }
  .stat-chip-value { font-size: 18px; font-weight: 600; font-family: 'Syne', sans-serif; }

  /* LEGEND */
  .legend {
    display: flex;
    flex-wrap: wrap;
    gap: 16px;
    margin-bottom: 14px;
  }
  .legend-item {
    display: flex;
    align-items: center;
    gap: 6px;
    font-size: 11px;
    color: var(--text-secondary);
  }
  .legend-dot {
    width: 8px;
    height: 8px;
    border-radius: 2px;
    flex-shrink: 0;
  }

  /* OVERVIEW PAGE */
  .overview-grid {
    display: grid;
    grid-template-columns: 1fr 1fr 1fr;
    gap: 16px;
  }
  .overview-main {
    grid-column: span 2;
  }

  /* SCROLLBAR */
  ::-webkit-scrollbar { width: 4px; height: 4px; }
  ::-webkit-scrollbar-track { background: transparent; }
  ::-webkit-scrollbar-thumb { background: var(--border2); border-radius: 4px; }

  /* ANIM */
  @keyframes fadeUp {
    from { opacity: 0; transform: translateY(12px); }
    to   { opacity: 1; transform: translateY(0); }
  }
  .page.active .card,
  .page.active .kpi-card,
  .page.active .insight-box,
  .page.active .risk-card {
    animation: fadeUp .4s ease both;
  }
  .page.active .card:nth-child(2) { animation-delay: .05s; }
  .page.active .card:nth-child(3) { animation-delay: .10s; }
  .page.active .card:nth-child(4) { animation-delay: .15s; }

  @media (max-width: 900px) {
    .header, .content { padding-left: 24px; padding-right: 24px; }
    .kpi-grid { grid-template-columns: 1fr 1fr; }
    .two-col, .three-col, .overview-grid { grid-template-columns: 1fr; }
    .overview-main { grid-column: span 1; }
  }
</style>
</head>
<body>

<div class="header">
  <div class="header-top">
    <div>
      <div class="header-eyebrow">IBM HR Dataset · 1,470 employees · 35 variables</div>
      <div class="header-title">Employee Attrition<br><span>Analysis</span></div>
      <div class="header-sub">Six questions that drive retention decisions</div>
    </div>
    <div style="text-align:right; padding-top:8px;">
      <div class="badge red">16.1% attrition rate</div>
      <div style="font-size:11px; color:var(--text-tertiary); margin-top:8px;">237 employees left</div>
    </div>
  </div>
  <div class="nav-tabs">
    <button class="nav-tab active" onclick="showPage('overview',this)">Overview</button>
    <button class="nav-tab" onclick="showPage('q1',this)">Q1 · Overtime</button>
    <button class="nav-tab" onclick="showPage('q2',this)">Q2 · Compensation</button>
    <button class="nav-tab" onclick="showPage('q3',this)">Q3 · Satisfaction</button>
    <button class="nav-tab" onclick="showPage('q4',this)">Q4 · Tenure</button>
    <button class="nav-tab" onclick="showPage('q5',this)">Q5 · Promotion</button>
    <button class="nav-tab" onclick="showPage('q6',this)">Q6 · Demographics</button>
  </div>
</div>

<div class="content">

  <!-- OVERVIEW -->
  <div class="page active" id="page-overview">
    <div class="kpi-grid">
      <div class="kpi-card">
        <div class="kpi-label">Total employees</div>
        <div class="kpi-value">1,470</div>
        <div class="kpi-delta neutral">IBM workforce sample</div>
      </div>
      <div class="kpi-card">
        <div class="kpi-label">Attrition count</div>
        <div class="kpi-value">237</div>
        <div class="kpi-delta up">↑ 16.1% of workforce</div>
      </div>
      <div class="kpi-card">
        <div class="kpi-label">Avg income — left</div>
        <div class="kpi-value">$4,787</div>
        <div class="kpi-delta up">↓ $2,046 below stayers</div>
      </div>
      <div class="kpi-card">
        <div class="kpi-label">Overtime risk multiplier</div>
        <div class="kpi-value">2.9×</div>
        <div class="kpi-delta up">↑ Highest single factor</div>
      </div>
    </div>

    <div class="section-header">
      <div class="section-q">What's driving attrition?</div>
      <div class="section-title">Six factors — ranked by impact</div>
      <div class="section-sub">Each factor's maximum attrition rate at its highest-risk level</div>
    </div>

    <div class="card" style="margin-bottom:16px;">
      <div class="card-title">Peak attrition rate by factor</div>
      <div style="padding-top:4px;">
        <div class="hbar-row">
          <div class="hbar-label">Overtime</div>
          <div class="hbar-track"><div class="hbar-fill" style="width:72.6%; background: linear-gradient(90deg, #7f1d1d, #c0392b);"><span class="hbar-pct">30.5%</span></div></div>
          <div class="hbar-num" style="color:var(--red);">30.5%</div>
        </div>
        <div class="hbar-row">
          <div class="hbar-label">Tenure 0–1yr</div>
          <div class="hbar-track"><div class="hbar-fill" style="width:83%; background: linear-gradient(90deg, #7f1d1d, #c0392b);"><span class="hbar-pct">34.9%</span></div></div>
          <div class="hbar-num" style="color:var(--red);">34.9%</div>
        </div>
        <div class="hbar-row">
          <div class="hbar-label">Work-life bal.</div>
          <div class="hbar-track"><div class="hbar-fill" style="width:74%; background: linear-gradient(90deg, #7f3200, #c05c00);"><span class="hbar-pct">31.2%</span></div></div>
          <div class="hbar-num" style="color:var(--amber);">31.2%</div>
        </div>
        <div class="hbar-row">
          <div class="hbar-label">Age 18–25</div>
          <div class="hbar-track"><div class="hbar-fill" style="width:85%; background: linear-gradient(90deg, #7f1d1d, #c0392b);"><span class="hbar-pct">35.8%</span></div></div>
          <div class="hbar-num" style="color:var(--red);">35.8%</div>
        </div>
        <div class="hbar-row">
          <div class="hbar-label">Income &lt;$3k</div>
          <div class="hbar-track"><div class="hbar-fill" style="width:68%; background: linear-gradient(90deg, #7f3200, #c05c00);"><span class="hbar-pct">28.6%</span></div></div>
          <div class="hbar-num" style="color:var(--amber);">28.6%</div>
        </div>
        <div class="hbar-row">
          <div class="hbar-label">Single</div>
          <div class="hbar-track"><div class="hbar-fill" style="width:60.7%; background: linear-gradient(90deg, #374100, #608a00);"><span class="hbar-pct">25.5%</span></div></div>
          <div class="hbar-num" style="color:var(--green);">25.5%</div>
        </div>
      </div>
    </div>

    <div class="three-col">
      <div class="risk-card">
        <div class="risk-card-icon">⚡</div>
        <div class="risk-card-title">Overtime + Young (18–25)</div>
        <div class="risk-card-desc">The most dangerous combination in the dataset. Young employees on overtime face extremely high flight risk.</div>
        <div class="risk-card-impact">64.1%</div>
        <div class="risk-card-impact-label">attrition rate</div>
      </div>
      <div class="risk-card">
        <div class="risk-card-icon">📅</div>
        <div class="risk-card-title">First year is critical</div>
        <div class="risk-card-desc">34.9% of employees in their first year leave. Risk drops by nearly half in years 1–3.</div>
        <div class="risk-card-impact">34.9%</div>
        <div class="risk-card-impact-label">year 1 attrition</div>
      </div>
      <div class="risk-card">
        <div class="risk-card-icon">💰</div>
        <div class="risk-card-title">$2,046 pay gap</div>
        <div class="risk-card-desc">Employees who left earned $2,046/month less on average than those who stayed.</div>
        <div class="risk-card-impact">$2,046</div>
        <div class="risk-card-impact-label">monthly gap</div>
      </div>
    </div>

    <div class="insight-box" style="margin-top:0;">
      <strong>Bottom line:</strong> The data points to three high-ROI interventions — eliminating excessive overtime (especially in Sales), structured onboarding for the 0–12 month window, and closing the compensation gap for employees earning under $6k/month. Together, these address the top three risk factors.
    </div>
  </div>

  <!-- Q1 OVERTIME -->
  <div class="page" id="page-q1">
    <div class="section-header">
      <div class="section-q">Question 1</div>
      <div class="section-title">Does working overtime significantly affect attrition?</div>
      <div class="section-sub">And is there a threshold? (Dataset contains Yes/No flag, not actual hours)</div>
    </div>
    <div class="stat-row">
      <div class="stat-chip">
        <div class="stat-chip-label">No overtime attrition</div>
        <div class="stat-chip-value" style="color:var(--green)">10.4%</div>
      </div>
      <div class="stat-chip">
        <div class="stat-chip-label">Overtime attrition</div>
        <div class="stat-chip-value" style="color:var(--red)">30.5%</div>
      </div>
      <div class="stat-chip">
        <div class="stat-chip-label">Risk multiplier</div>
        <div class="stat-chip-value">2.9×</div>
      </div>
      <div class="stat-chip">
        <div class="stat-chip-label">Sales OT rate</div>
        <div class="stat-chip-value" style="color:var(--red)">37.5%</div>
      </div>
    </div>
    <div class="two-col">
      <div class="card">
        <div class="card-title">Attrition rate by overtime status</div>
        <div class="chart-container" style="height:200px"><canvas id="c1a" role="img" aria-label="Bar chart comparing overtime vs no overtime attrition."></canvas></div>
      </div>
      <div class="card">
        <div class="card-title">Overtime effect by department</div>
        <div class="chart-container" style="height:200px"><canvas id="c1b" role="img" aria-label="Grouped bar chart of overtime attrition by department."></canvas></div>
      </div>
    </div>
    <div class="card">
      <div class="card-title">What's driving overtime attrition?</div>
      <div style="margin-top:8px;">
        <div class="hbar-row">
          <div class="hbar-label">Sales OT</div>
          <div class="hbar-track"><div class="hbar-fill" style="width:89.3%; background:linear-gradient(90deg,#7f1d1d,#c0392b);"><span class="hbar-pct">37.5%</span></div></div>
          <div class="hbar-num" style="color:var(--red);">37.5%</div>
        </div>
        <div class="hbar-row">
          <div class="hbar-label">HR OT</div>
          <div class="hbar-track"><div class="hbar-fill" style="width:70%; background:linear-gradient(90deg,#7f1d1d,#c0392b);"><span class="hbar-pct">29.4%</span></div></div>
          <div class="hbar-num" style="color:var(--red);">29.4%</div>
        </div>
        <div class="hbar-row">
          <div class="hbar-label">R&D OT</div>
          <div class="hbar-track"><div class="hbar-fill" style="width:65%; background:linear-gradient(90deg,#7f3200,#c05c00);"><span class="hbar-pct">27.3%</span></div></div>
          <div class="hbar-num" style="color:var(--amber);">27.3%</div>
        </div>
        <div class="hbar-row">
          <div class="hbar-label">Sales no OT</div>
          <div class="hbar-track"><div class="hbar-fill" style="width:32.9%; background:linear-gradient(90deg,#14532d,#166534);"><span class="hbar-pct">13.8%</span></div></div>
          <div class="hbar-num" style="color:var(--green);">13.8%</div>
        </div>
        <div class="hbar-row">
          <div class="hbar-label">R&D no OT</div>
          <div class="hbar-track"><div class="hbar-fill" style="width:20.5%; background:linear-gradient(90deg,#14532d,#166534);"><span class="hbar-pct">8.6%</span></div></div>
          <div class="hbar-num" style="color:var(--green);">8.6%</div>
        </div>
      </div>
    </div>
    <div class="insight-box red" style="margin-top:16px;">
      <strong>Key finding:</strong> No hours threshold exists in this dataset (only a binary flag), but the 2.9× multiplier is striking and consistent. <strong>Sales overtime workers hit 37.5%</strong> — the highest risk segment. The worst combination: overtime + age 18–25 = <strong>64.1% attrition</strong>.
      <br><br><strong>Recommendation:</strong> Flag employees with consistent overtime as flight risk. Pilot workload redistribution in Sales first, where the ROI is highest.
    </div>
  </div>

  <!-- Q2 COMPENSATION -->
  <div class="page" id="page-q2">
    <div class="section-header">
      <div class="section-q">Question 2</div>
      <div class="section-title">How does monthly compensation relate to attrition?</div>
      <div class="section-sub">Attrition rate and income bracket analysis across 1,470 employees</div>
    </div>
    <div class="stat-row">
      <div class="stat-chip">
        <div class="stat-chip-label">Avg income — stayed</div>
        <div class="stat-chip-value" style="color:var(--green)">$6,833</div>
      </div>
      <div class="stat-chip">
        <div class="stat-chip-label">Avg income — left</div>
        <div class="stat-chip-value" style="color:var(--red)">$4,787</div>
      </div>
      <div class="stat-chip">
        <div class="stat-chip-label">Monthly gap</div>
        <div class="stat-chip-value">$2,046</div>
      </div>
      <div class="stat-chip">
        <div class="stat-chip-label">&lt;$3k attrition</div>
        <div class="stat-chip-value" style="color:var(--red)">28.6%</div>
      </div>
    </div>
    <div class="two-col">
      <div class="card">
        <div class="card-title">Attrition rate by income bracket</div>
        <div class="chart-container" style="height:220px"><canvas id="c2a" role="img" aria-label="Bar chart of attrition rate per income bracket."></canvas></div>
      </div>
      <div class="card">
        <div class="card-title">Income distribution — stayed vs left</div>
        <div class="chart-container" style="height:220px"><canvas id="c2b" role="img" aria-label="Overlapping income distribution chart."></canvas></div>
      </div>
    </div>
    <div class="card">
      <div class="card-title">Retention rate by bracket — how many stay?</div>
      <div style="margin-top:8px;">
        <div class="hbar-row">
          <div class="hbar-label">$12k+</div>
          <div class="hbar-track"><div class="hbar-fill" style="width:94.4%; background:linear-gradient(90deg,#14532d,#166534);"><span class="hbar-pct">94.4% stay</span></div></div>
          <div class="hbar-num" style="color:var(--green);">5.6%</div>
        </div>
        <div class="hbar-row">
          <div class="hbar-label">$6k–9k</div>
          <div class="hbar-track"><div class="hbar-fill" style="width:89.2%; background:linear-gradient(90deg,#14532d,#166534);"><span class="hbar-pct">89.2% stay</span></div></div>
          <div class="hbar-num" style="color:var(--green);">10.8%</div>
        </div>
        <div class="hbar-row">
          <div class="hbar-label">$3k–6k</div>
          <div class="hbar-track"><div class="hbar-fill" style="width:87.3%; background:linear-gradient(90deg,#374100,#608a00);"><span class="hbar-pct">87.3% stay</span></div></div>
          <div class="hbar-num" style="color:var(--amber);">12.7%</div>
        </div>
        <div class="hbar-row">
          <div class="hbar-label">$9k–12k</div>
          <div class="hbar-track"><div class="hbar-fill" style="width:83.3%; background:linear-gradient(90deg,#374100,#608a00);"><span class="hbar-pct">83.3% stay</span></div></div>
          <div class="hbar-num" style="color:var(--amber);">16.7%</div>
        </div>
        <div class="hbar-row">
          <div class="hbar-label">&lt;$3k</div>
          <div class="hbar-track"><div class="hbar-fill" style="width:71.4%; background:linear-gradient(90deg,#7f1d1d,#c0392b);"><span class="hbar-pct">71.4% stay</span></div></div>
          <div class="hbar-num" style="color:var(--red);">28.6%</div>
        </div>
      </div>
    </div>
    <div class="insight-box" style="margin-top:16px;">
      <strong>Key finding:</strong> Attrition drops sharply once pay crosses <strong>~$6k/month</strong>. The &lt;$3k bracket loses nearly 1 in 3 employees — over 5× the rate of the $12k+ group. The $9k–12k bracket is a notable anomaly with 16.7%, worth investigating separately.
      <br><br><strong>Recommendation:</strong> Target compensation reviews at employees earning under $6k/month. A $500/month raise in the lowest bracket could yield significant retention gains relative to cost.
    </div>
  </div>

  <!-- Q3 SATISFACTION -->
  <div class="page" id="page-q3">
    <div class="section-header">
      <div class="section-q">Question 3</div>
      <div class="section-title">What matters more for retention — job satisfaction, environment, or work-life balance?</div>
      <div class="section-sub">Each scored 1 (low) to 4 (high) · Attrition rate at each level</div>
    </div>
    <div class="stat-row">
      <div class="stat-chip">
        <div class="stat-chip-label">Work-life bal. score 1</div>
        <div class="stat-chip-value" style="color:var(--red)">31.2%</div>
      </div>
      <div class="stat-chip">
        <div class="stat-chip-label">Env. satisfaction score 1</div>
        <div class="stat-chip-value" style="color:var(--red)">25.4%</div>
      </div>
      <div class="stat-chip">
        <div class="stat-chip-label">Job satisfaction score 1</div>
        <div class="stat-chip-value" style="color:var(--amber)">22.8%</div>
      </div>
      <div class="stat-chip">
        <div class="stat-chip-label">Job satisfaction score 4</div>
        <div class="stat-chip-value" style="color:var(--green)">11.3%</div>
      </div>
    </div>
    <div class="card" style="margin-bottom:16px;">
      <div class="card-title">Attrition rate at each satisfaction score</div>
      <div class="legend">
        <div class="legend-item"><div class="legend-dot" style="background:#4ade80"></div>Work-life balance</div>
        <div class="legend-item"><div class="legend-dot" style="background:#60a5fa"></div>Environment satisfaction</div>
        <div class="legend-item"><div class="legend-dot" style="background:#fbbf24"></div>Job satisfaction</div>
      </div>
      <div class="chart-container" style="height:250px"><canvas id="c3a" role="img" aria-label="Line chart of attrition rate at each score level for three dimensions."></canvas></div>
    </div>
    <div class="two-col">
      <div class="card">
        <div class="card-label">Score 1 comparison</div>
        <div class="card-title" style="margin-bottom:12px;">Who suffers most at the bottom?</div>
        <div class="hbar-row">
          <div class="hbar-label">Work-life bal.</div>
          <div class="hbar-track"><div class="hbar-fill" style="width:74%; background:linear-gradient(90deg,#7f1d1d,#c0392b);"><span class="hbar-pct">31.2%</span></div></div>
          <div class="hbar-num" style="color:var(--red)">31.2%</div>
        </div>
        <div class="hbar-row">
          <div class="hbar-label">Env. satis.</div>
          <div class="hbar-track"><div class="hbar-fill" style="width:60%; background:linear-gradient(90deg,#7f3200,#c05c00);"><span class="hbar-pct">25.4%</span></div></div>
          <div class="hbar-num" style="color:var(--amber)">25.4%</div>
        </div>
        <div class="hbar-row">
          <div class="hbar-label">Job satis.</div>
          <div class="hbar-track"><div class="hbar-fill" style="width:54%; background:linear-gradient(90deg,#7f3200,#c05c00);"><span class="hbar-pct">22.8%</span></div></div>
          <div class="hbar-num" style="color:var(--amber)">22.8%</div>
        </div>
      </div>
      <div class="card">
        <div class="card-label">Score 4 comparison</div>
        <div class="card-title" style="margin-bottom:12px;">Does a perfect score protect you?</div>
        <div class="hbar-row">
          <div class="hbar-label">Work-life bal.</div>
          <div class="hbar-track"><div class="hbar-fill" style="width:42%; background:linear-gradient(90deg,#7f3200,#c05c00);"><span class="hbar-pct">17.6%</span></div></div>
          <div class="hbar-num" style="color:var(--amber)">17.6%</div>
        </div>
        <div class="hbar-row">
          <div class="hbar-label">Env. satis.</div>
          <div class="hbar-track"><div class="hbar-fill" style="width:32%; background:linear-gradient(90deg,#374100,#608a00);"><span class="hbar-pct">13.5%</span></div></div>
          <div class="hbar-num" style="color:var(--green)">13.5%</div>
        </div>
        <div class="hbar-row">
          <div class="hbar-label">Job satis.</div>
          <div class="hbar-track"><div class="hbar-fill" style="width:27%; background:linear-gradient(90deg,#14532d,#166534);"><span class="hbar-pct">11.3%</span></div></div>
          <div class="hbar-num" style="color:var(--green)">11.3%</div>
        </div>
      </div>
    </div>
    <div class="insight-box amber" style="margin-top:16px;">
      <strong>Key finding:</strong> Work-life balance is the most explosive risk at the bottom (31.2% at score 1) but surprisingly still shows 17.6% attrition at score 4 — suggesting it's not sufficient alone. Environment satisfaction has the most consistent downward gradient. Job satisfaction is the most controllable and shows the clearest payoff at score 4 (11.3%).
      <br><br><strong>Recommendation:</strong> Prioritise work-life balance interventions as triage, but invest in job satisfaction programs for long-term retention — it has the most predictable return.
    </div>
  </div>

  <!-- Q4 TENURE -->
  <div class="page" id="page-q4">
    <div class="section-header">
      <div class="section-q">Question 4</div>
      <div class="section-title">Is there a "flight risk" tenure window?</div>
      <div class="section-sub">Attrition rate segmented by years at company</div>
    </div>
    <div class="stat-row">
      <div class="stat-chip">
        <div class="stat-chip-label">Highest risk window</div>
        <div class="stat-chip-value" style="color:var(--red)">0–1 yr</div>
      </div>
      <div class="stat-chip">
        <div class="stat-chip-label">Year 1 attrition</div>
        <div class="stat-chip-value" style="color:var(--red)">34.9%</div>
      </div>
      <div class="stat-chip">
        <div class="stat-chip-label">Avg tenure — stayed</div>
        <div class="stat-chip-value">7.4 yrs</div>
      </div>
      <div class="stat-chip">
        <div class="stat-chip-label">Avg tenure — left</div>
        <div class="stat-chip-value" style="color:var(--red)">5.1 yrs</div>
      </div>
    </div>
    <div class="card" style="margin-bottom:16px;">
      <div class="card-title">Attrition rate by tenure band</div>
      <div class="chart-container" style="height:260px"><canvas id="c4a" role="img" aria-label="Bar chart showing attrition rate decreasing with tenure length."></canvas></div>
    </div>
    <div class="two-col">
      <div class="card">
        <div class="card-title">Risk drops sharply over time</div>
        <div style="margin-top:8px;">
          <div class="hbar-row">
            <div class="hbar-label">0–1 yr</div>
            <div class="hbar-track"><div class="hbar-fill" style="width:83%; background:linear-gradient(90deg,#7f1d1d,#c0392b);"><span class="hbar-pct">34.9%</span></div></div>
            <div class="hbar-num" style="color:var(--red)">34.9%</div>
          </div>
          <div class="hbar-row">
            <div class="hbar-label">1–3 yrs</div>
            <div class="hbar-track"><div class="hbar-fill" style="width:43.8%; background:linear-gradient(90deg,#7f3200,#c05c00);"><span class="hbar-pct">18.4%</span></div></div>
            <div class="hbar-num" style="color:var(--amber)">18.4%</div>
          </div>
          <div class="hbar-row">
            <div class="hbar-label">3–5 yrs</div>
            <div class="hbar-track"><div class="hbar-fill" style="width:31.1%; background:linear-gradient(90deg,#374100,#608a00);"><span class="hbar-pct">13.1%</span></div></div>
            <div class="hbar-num" style="color:var(--green)">13.1%</div>
          </div>
          <div class="hbar-row">
            <div class="hbar-label">5–10 yrs</div>
            <div class="hbar-track"><div class="hbar-fill" style="width:29.3%; background:linear-gradient(90deg,#374100,#608a00);"><span class="hbar-pct">12.3%</span></div></div>
            <div class="hbar-num" style="color:var(--green)">12.3%</div>
          </div>
          <div class="hbar-row">
            <div class="hbar-label">10–15 yrs</div>
            <div class="hbar-track"><div class="hbar-fill" style="width:15.5%; background:linear-gradient(90deg,#14532d,#166534);"><span class="hbar-pct">6.5%</span></div></div>
            <div class="hbar-num" style="color:var(--green)">6.5%</div>
          </div>
          <div class="hbar-row">
            <div class="hbar-label">15+ yrs</div>
            <div class="hbar-track"><div class="hbar-fill" style="width:22.4%; background:linear-gradient(90deg,#374100,#608a00);"><span class="hbar-pct">9.4%</span></div></div>
            <div class="hbar-num" style="color:var(--amber)">9.4%</div>
          </div>
        </div>
      </div>
      <div class="card">
        <div class="card-title">Employee count by tenure band</div>
        <div class="chart-container" style="height:220px"><canvas id="c4b" role="img" aria-label="Bar chart showing employee count per tenure band."></canvas></div>
      </div>
    </div>
    <div class="insight-box" style="margin-top:16px;">
      <strong>Key finding:</strong> Year 0–1 is the critical danger zone at <strong>34.9%</strong>. Risk halves by years 1–3 (18.4%) and continues falling. Employees in years 10–15 are the most stable at 6.5%. The slight uptick at 15+ (9.4%) could reflect retirement-age transitions.
      <br><br><strong>Recommendation:</strong> Invest in structured 30/60/90-day onboarding. Assign mentors to all new hires. Track early engagement scores at month 3 as a leading indicator.
    </div>
  </div>

  <!-- Q5 PROMOTION -->
  <div class="page" id="page-q5">
    <div class="section-header">
      <div class="section-q">Question 5</div>
      <div class="section-title">How does promotion lag affect attrition?</div>
      <div class="section-sub">Years since last promotion vs attrition rate — a counterintuitive story</div>
    </div>
    <div class="stat-row">
      <div class="stat-chip">
        <div class="stat-chip-label">Just promoted risk</div>
        <div class="stat-chip-value" style="color:var(--red)">18.9%</div>
      </div>
      <div class="stat-chip">
        <div class="stat-chip-label">Lowest risk group</div>
        <div class="stat-chip-value" style="color:var(--green)">4–5 yrs</div>
      </div>
      <div class="stat-chip">
        <div class="stat-chip-label">4–5 yr attrition</div>
        <div class="stat-chip-value" style="color:var(--green)">6.6%</div>
      </div>
      <div class="stat-chip">
        <div class="stat-chip-label">Avg yrs — left</div>
        <div class="stat-chip-value">1.9 yrs</div>
      </div>
    </div>
    <div class="card" style="margin-bottom:16px;">
      <div class="card-title">Attrition rate by years since last promotion</div>
      <div class="chart-container" style="height:240px"><canvas id="c5a" role="img" aria-label="Bar chart of attrition by promotion lag — U-shaped pattern."></canvas></div>
    </div>
    <div class="card">
      <div class="card-title">The counterintuitive finding — promotion lag vs risk</div>
      <div style="margin-top:8px;">
        <div class="hbar-row">
          <div class="hbar-label">Just promoted</div>
          <div class="hbar-track"><div class="hbar-fill" style="width:45%; background:linear-gradient(90deg,#7f1d1d,#c0392b);"><span class="hbar-pct">18.9% — highest risk</span></div></div>
          <div class="hbar-num" style="color:var(--red)">18.9%</div>
        </div>
        <div class="hbar-row">
          <div class="hbar-label">2–3 yrs ago</div>
          <div class="hbar-track"><div class="hbar-fill" style="width:40.7%; background:linear-gradient(90deg,#7f3200,#c05c00);"><span class="hbar-pct">17.1%</span></div></div>
          <div class="hbar-num" style="color:var(--amber)">17.1%</div>
        </div>
        <div class="hbar-row">
          <div class="hbar-label">6+ yrs ago</div>
          <div class="hbar-track"><div class="hbar-fill" style="width:38.8%; background:linear-gradient(90deg,#7f3200,#c05c00);"><span class="hbar-pct">16.3%</span></div></div>
          <div class="hbar-num" style="color:var(--amber)">16.3%</div>
        </div>
        <div class="hbar-row">
          <div class="hbar-label">1 yr ago</div>
          <div class="hbar-track"><div class="hbar-fill" style="width:32.6%; background:linear-gradient(90deg,#374100,#608a00);"><span class="hbar-pct">13.7%</span></div></div>
          <div class="hbar-num" style="color:var(--green)">13.7%</div>
        </div>
        <div class="hbar-row">
          <div class="hbar-label">4–5 yrs ago</div>
          <div class="hbar-track"><div class="hbar-fill" style="width:15.7%; background:linear-gradient(90deg,#14532d,#166534);"><span class="hbar-pct">6.6% — safest</span></div></div>
          <div class="hbar-num" style="color:var(--green)">6.6%</div>
        </div>
      </div>
    </div>
    <div class="insight-box amber" style="margin-top:16px;">
      <strong>Counterintuitive finding:</strong> Employees promoted most recently (0 years ago) have the <strong>highest attrition at 18.9%</strong>. This likely reflects reactive or mismatched promotions — people were promoted to retain them but it didn't address the root cause. The 4–5 year group is the most stable.
      <br><br><strong>Recommendation:</strong> Audit promotion quality, not just frequency. A promotion that increases scope without proper support may accelerate departure. Follow up new promotions with role clarity conversations at 30/60 days.
    </div>
  </div>

  <!-- Q6 DEMOGRAPHICS -->
  <div class="page" id="page-q6">
    <div class="section-header">
      <div class="section-q">Question 6</div>
      <div class="section-title">Is there a demographic pattern in attrition?</div>
      <div class="section-sub">Age, gender, and marital status as attrition predictors</div>
    </div>
    <div class="stat-row">
      <div class="stat-chip">
        <div class="stat-chip-label">Highest age risk</div>
        <div class="stat-chip-value" style="color:var(--red)">18–25</div>
      </div>
      <div class="stat-chip">
        <div class="stat-chip-label">18–25 attrition</div>
        <div class="stat-chip-value" style="color:var(--red)">35.8%</div>
      </div>
      <div class="stat-chip">
        <div class="stat-chip-label">Single employees</div>
        <div class="stat-chip-value" style="color:var(--red)">25.5%</div>
      </div>
      <div class="stat-chip">
        <div class="stat-chip-label">Gender gap</div>
        <div class="stat-chip-value">2.2 pts</div>
      </div>
    </div>
    <div class="two-col">
      <div class="card">
        <div class="card-title">Attrition rate by age group</div>
        <div class="chart-container" style="height:220px"><canvas id="c6a" role="img" aria-label="Bar chart of attrition by age group."></canvas></div>
      </div>
      <div class="card">
        <div class="card-title">Attrition by marital status</div>
        <div class="chart-container" style="height:220px"><canvas id="c6b" role="img" aria-label="Bar chart of attrition by marital status."></canvas></div>
      </div>
    </div>
    <div class="card" style="margin-bottom:16px;">
      <div class="card-title">Highest-risk demographic combinations</div>
      <div style="margin-top:8px;">
        <div class="hbar-row">
          <div class="hbar-label" style="width:150px;">OT + Age 18–25</div>
          <div class="hbar-track"><div class="hbar-fill" style="width:100%; background:linear-gradient(90deg,#7f0000,#dc2626);"><span class="hbar-pct">64.1% attrition</span></div></div>
          <div class="hbar-num" style="color:var(--red)">64.1%</div>
        </div>
        <div class="hbar-row">
          <div class="hbar-label" style="width:150px;">OT + Age 26–35</div>
          <div class="hbar-track"><div class="hbar-fill" style="width:60.9%; background:linear-gradient(90deg,#7f1d1d,#c0392b);"><span class="hbar-pct">39%</span></div></div>
          <div class="hbar-num" style="color:var(--red)">39.0%</div>
        </div>
        <div class="hbar-row">
          <div class="hbar-label" style="width:150px;">No OT + 18–25</div>
          <div class="hbar-track"><div class="hbar-fill" style="width:35.4%; background:linear-gradient(90deg,#7f3200,#c05c00);"><span class="hbar-pct">22.6%</span></div></div>
          <div class="hbar-num" style="color:var(--amber)">22.6%</div>
        </div>
        <div class="hbar-row">
          <div class="hbar-label" style="width:150px;">OT + 46–60</div>
          <div class="hbar-track"><div class="hbar-fill" style="width:32%; background:linear-gradient(90deg,#374100,#608a00);"><span class="hbar-pct">20.5%</span></div></div>
          <div class="hbar-num" style="color:var(--amber)">20.5%</div>
        </div>
        <div class="hbar-row">
          <div class="hbar-label" style="width:150px;">No OT + 36–45</div>
          <div class="hbar-track"><div class="hbar-fill" style="width:9.4%; background:linear-gradient(90deg,#14532d,#166534);"><span class="hbar-pct">6%</span></div></div>
          <div class="hbar-num" style="color:var(--green)">6.0%</div>
        </div>
      </div>
    </div>
    <div class="insight-box" style="margin-top:0;">
      <strong>Key finding:</strong> Young employees (18–25) leave at <strong>35.8%</strong> — nearly 4× the 36–45 group. Single employees leave at more than double the rate of divorced employees. Gender alone is a weak predictor (2.2pt gap), but the intersection of <strong>young + single + overtime</strong> is the highest-risk cohort in the entire dataset (64.1%).
      <br><br><strong>Recommendation:</strong> Target early-career development programs and mentoring specifically at under-25 employees. Review workload for young workers in overtime-heavy roles.
    </div>
  </div>

</div>

<script>
const tc = 'rgba(255,255,255,0.35)';
const gc = 'rgba(255,255,255,0.06)';
const def = {
  responsive: true,
  maintainAspectRatio: false,
  plugins: { legend: { display: false } },
  scales: {
    x: { ticks: { color: tc, font: { size: 11 } }, grid: { color: gc }, border: { color: 'rgba(255,255,255,0.08)' } },
    y: { ticks: { color: tc, font: { size: 11 }, callback: v => v + '%' }, grid: { color: gc }, border: { color: 'rgba(255,255,255,0.08)' } }
  }
};

function mkChart(id, type, labels, datasets, extraOpts) {
  const el = document.getElementById(id);
  if (!el) return;
  new Chart(el, {
    type,
    data: { labels, datasets },
    options: Object.assign({}, def, extraOpts || {})
  });
}

window.addEventListener('load', () => {
  // Q1a
  mkChart('c1a','bar',['No overtime','Overtime'],[{data:[10.4,30.5],backgroundColor:['#166534','#991b1b'],borderRadius:6,barThickness:60}]);
  // Q1b
  mkChart('c1b','bar',['Human Resources','R&D','Sales'],[
    {label:'No OT',data:[15.2,8.6,13.8],backgroundColor:'#1d4ed8',borderRadius:4,barThickness:20},
    {label:'OT',data:[29.4,27.3,37.5],backgroundColor:'#991b1b',borderRadius:4,barThickness:20}
  ],{plugins:{legend:{display:false}}});
  // Q2a
  mkChart('c2a','bar',['<$3k','$3k–6k','$6k–9k','$9k–12k','$12k+'],[{data:[28.6,12.7,10.8,16.7,5.6],backgroundColor:['#991b1b','#c05c00','#3d6b00','#14532d','#052e16'],borderRadius:6}]);
  // Q2b
  mkChart('c2b','bar',['<$3k','$3k–6k','$6k–9k','$9k–12k','$12k+'],[
    {label:'Stayed',data:[71.4,87.3,89.2,83.3,94.4],backgroundColor:'rgba(74,222,128,0.5)',borderRadius:4,barThickness:20},
    {label:'Left',data:[28.6,12.7,10.8,16.7,5.6],backgroundColor:'rgba(248,113,113,0.7)',borderRadius:4,barThickness:20}
  ],{scales:{x:{stacked:true,ticks:{color:tc,font:{size:10}},grid:{color:gc},border:{color:'rgba(255,255,255,0.08)'}},y:{stacked:true,ticks:{color:tc,font:{size:11},callback:v=>v+'%'},grid:{color:gc},border:{color:'rgba(255,255,255,0.08)'}}}});
  // Q3a
  mkChart('c3a','line',['Score 1','Score 2','Score 3','Score 4'],[
    {label:'Work-life balance',data:[31.2,16.9,14.2,17.6],borderColor:'#4ade80',backgroundColor:'rgba(74,222,128,0.05)',tension:.4,pointRadius:5,fill:true},
    {label:'Env. satisfaction',data:[25.4,15.0,13.7,13.5],borderColor:'#60a5fa',backgroundColor:'rgba(96,165,250,0.05)',tension:.4,pointRadius:5,fill:true},
    {label:'Job satisfaction',data:[22.8,16.4,16.5,11.3],borderColor:'#fbbf24',backgroundColor:'rgba(251,191,36,0.05)',tension:.4,pointRadius:5,fill:true}
  ],{scales:{x:{ticks:{color:tc,font:{size:11}},grid:{color:gc},border:{color:'rgba(255,255,255,0.08)'}},y:{min:0,max:36,ticks:{color:tc,font:{size:11},callback:v=>v+'%'},grid:{color:gc},border:{color:'rgba(255,255,255,0.08)'}}}});
  // Q4a
  mkChart('c4a','bar',['0–1 yr','1–3 yrs','3–5 yrs','5–10 yrs','10–15 yrs','15+ yrs'],[{
    data:[34.9,18.4,13.1,12.3,6.5,9.4],
    backgroundColor:['#991b1b','#c05c00','#3d6b00','#14532d','#052e16','#1d4a2d'],
    borderRadius:6
  }]);
  // Q4b
  mkChart('c4b','bar',['0–1 yr','1–3 yrs','3–5 yrs','5–10 yrs','10–15 yrs','15+ yrs'],[{
    data:[215,255,306,448,108,138],
    backgroundColor:'rgba(255,255,255,0.12)',
    borderRadius:6
  }],{scales:{x:{ticks:{color:tc,font:{size:10}},grid:{color:gc},border:{color:'rgba(255,255,255,0.08)'}},y:{ticks:{color:tc,font:{size:11}},grid:{color:gc},border:{color:'rgba(255,255,255,0.08)'}}}});
  // Q5a
  mkChart('c5a','bar',['Just promoted','1 yr ago','2–3 yrs ago','4–5 yrs ago','6+ yrs ago'],[{
    data:[18.9,13.7,17.1,6.6,16.3],
    backgroundColor:['#991b1b','#3d6b00','#c05c00','#052e16','#c05c00'],
    borderRadius:6
  }]);
  // Q6a
  mkChart('c6a','bar',['18–25','26–35','36–45','46–60'],[{
    data:[35.8,19.1,9.2,12.5],
    backgroundColor:['#991b1b','#c05c00','#14532d','#3d6b00'],
    borderRadius:6
  }]);
  // Q6b
  mkChart('c6b','bar',['Single','Married','Divorced'],[{
    data:[25.5,12.5,10.1],
    backgroundColor:['#991b1b','#3d6b00','#14532d'],
    borderRadius:6
  }]);
});

function showPage(id, btn) {
  document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
  document.querySelectorAll('.nav-tab').forEach(t => t.classList.remove('active'));
  document.getElementById('page-' + id).classList.add('active');
  btn.classList.add('active');
}
</script>
</body>
</html>
