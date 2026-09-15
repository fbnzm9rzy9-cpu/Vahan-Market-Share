
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>Vahan Market Share Dashboard — MSIL Region</title>
<style>
:root{
  --bg:#f4f6fb;
  --card:#ffffff;
  --ink:#101828;
  --muted:#667085;
  --line:#e6e9f0;
  --primary:#1d4ed8;
  --nexa:#0f766e;
  --arena:#b91c1c;
  --pos:#d1fae5;
  --pos-txt:#047857;
  --neg:#fee2e2;
  --neg-txt:#be123c;
  --flat:#f1f5f9;
  --flat-txt:#475569;
  --radius:12px;
  --shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.05), 0 2px 4px -1px rgba(0, 0, 0, 0.03);
}

*{box-sizing:border-box;}
html,body{margin:0;padding:0;}
body{
  background-color: var(--bg);
  color:var(--ink);
  font-family: "Segoe UI", -apple-system, BlinkMacSystemFont, Roboto, sans-serif;
  font-size:14px;
  line-height:1.4;
}

.app-container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 16px;
}

/* ---------- Topbar (Concise Blue Area matched to Image) ---------- */
.page-title {
  color: #1d3fae;
  font-size: 22px;
  font-weight: 800;
  margin: 10px 0 16px 0;
  letter-spacing: -0.5px;
}

.blue-card {
  background: linear-gradient(135deg, #0f172a 0%, #1e3a8a 100%);
  border-radius: var(--radius);
  padding: 20px;
  color: #fff;
  box-shadow: var(--shadow);
  margin-bottom: 24px;
}

.blue-card-header {
  display: flex;
  align-items: center;
  gap: 16px;
  margin-bottom: 20px;
  border-bottom: 1px solid rgba(255,255,255,0.15);
  padding-bottom: 16px;
}

.brand-icon {
  width: 42px;
  height: 42px;
  border-radius: 10px;
  background: linear-gradient(135deg, #ff7a59, #ff8a3d);
  display: flex;
  align-items: center;
  justify-content: center;
  font-weight: 800;
  font-size: 18px;
  box-shadow: 0 4px 12px rgba(255,122,89,0.4);
}

.brand-text h1 { margin: 0; font-size: 18px; font-weight: 700; }
.brand-text p { margin: 4px 0 0; font-size: 12px; color: #93c5fd; }

.filters-row {
  display: flex;
  align-items: flex-end;
  gap: 16px;
  flex-wrap: wrap;
}

.filter-group {
  display: flex;
  flex-direction: column;
  gap: 6px;
  flex: 1;
  min-width: 200px;
}

.filter-group label {
  font-size: 11px;
  text-transform: uppercase;
  letter-spacing: 0.5px;
  color: #93c5fd;
  font-weight: 600;
}

.filter-group select {
  appearance: none;
  background: #fff;
  border: none;
  padding: 10px 14px;
  border-radius: 8px;
  font-size: 14px;
  font-weight: 600;
  color: var(--ink);
  background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='12' height='8'%3E%3Cpath d='M1 1l5 5 5-5' stroke='%23444' stroke-width='1.6' fill='none' stroke-linecap='round'/%3E%3C/svg%3E");
  background-repeat: no-repeat;
  background-position: right 14px center;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
  cursor: pointer;
}

.reset-btn {
  background: rgba(255,255,255,0.1);
  border: 1px solid rgba(255,255,255,0.3);
  color: #fff;
  padding: 10px 16px;
  border-radius: 8px;
  font-size: 13px;
  font-weight: 600;
  cursor: pointer;
  transition: 0.2s;
  height: 40px; /* Align with inputs */
}

.reset-btn:hover { background: rgba(255,255,255,0.2); }

.scope-indicator {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  background: rgba(255,255,255,0.1);
  border: 1px solid rgba(255,255,255,0.2);
  padding: 6px 14px;
  border-radius: 20px;
  font-size: 12px;
  font-weight: 600;
  color: #fff;
  margin-top: 16px;
}
.scope-indicator .dot { width: 8px; height: 8px; border-radius: 50%; background: #34d399; }

/* ---------- KPI Cards ---------- */
.kpi-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
  gap: 16px;
  margin-bottom: 24px;
}

.kpi-card {
  background: var(--card);
  border-radius: var(--radius);
  box-shadow: var(--shadow);
  padding: 20px;
  border: 1px solid var(--line);
  border-left: 4px solid var(--primary);
}
.kpi-card.nexa { border-left-color: var(--nexa); }
.kpi-card.arena { border-left-color: var(--arena); }

.kpi-header {
  font-size: 11.5px;
  text-transform: uppercase;
  letter-spacing: 0.5px;
  color: var(--muted);
  font-weight: 700;
  margin-bottom: 12px;
}

.kpi-value {
  font-size: 28px;
  font-weight: 800;
  color: var(--ink);
  margin-bottom: 8px;
}

.kpi-meta {
  font-size: 12.5px;
  color: var(--muted);
  margin-bottom: 14px;
}

.kpi-badge-row {
  display: flex;
  align-items: center;
  gap: 10px;
  font-size: 13px;
}

.badge {
  display: inline-flex;
  align-items: center;
  gap: 4px;
  font-size: 12.5px;
  font-weight: 700;
  padding: 4px 10px;
  border-radius: 6px;
}
.badge.pos { background: var(--pos); color: var(--pos-txt); }
.badge.neg { background: var(--neg); color: var(--neg-txt); }
.badge.flat { background: var(--flat); color: var(--flat-txt); }

/* ---------- Detailed Data Section ---------- */
.section {
  background: var(--card);
  border-radius: var(--radius);
  box-shadow: var(--shadow);
  border: 1px solid var(--line);
  margin-bottom: 30px;
  display: none; /* Hidden by default */
}

.section-head {
  padding: 16px 20px;
  border-bottom: 1px solid var(--line);
  background: #f8fafc;
  border-radius: var(--radius) var(--radius) 0 0;
}
.section-head h2 { margin: 0; font-size: 16px; font-weight: 800; color: #0f172a; }
.section-head p { margin: 4px 0 0; font-size: 12.5px; color: var(--muted); }

.table-container { padding: 0 20px 20px 20px; overflow-x: auto; }
table { border-collapse: collapse; width: 100%; font-size: 13px; margin-top: 16px; }
th, td { padding: 12px 14px; text-align: left; border-bottom: 1px solid var(--line); }
th { background: #f8fafc; font-size: 11px; text-transform: uppercase; color: var(--muted); font-weight: 700; letter-spacing: 0.3px; }
td.num, th.num { text-align: right; font-variant-numeric: tabular-nums; }
tr:hover td { background: #f8fafc; }
.oem-name { font-weight: 700; color: #0f172a; }

.chg-txt.pos { color: var(--pos-txt); font-weight: 600; }
.chg-txt.neg { color: var(--neg-txt); font-weight: 600; }

.location-title {
  padding: 16px 20px;
  background: #f1f5f9;
  font-weight: 800;
  font-size: 14px;
  color: #0f172a;
  border-bottom: 1px solid var(--line);
  border-top: 1px solid var(--line);
}

@media (max-width: 768px) {
  .filters-row { flex-direction: column; align-items: stretch; }
  .reset-btn { width: 100%; }
}
</style>
</head>
<body>

<div class="app-container">
  <h1 class="page-title">Vahan-Market-Share</h1>
  
  <div class="blue-card">
    <div class="blue-card-header">
      <div class="brand-icon">V</div>
      <div class="brand-text">
        <h1>Vahan Market Share Dashboard</h1>
        <p>Nexa & Arena performance · RTO / City / Cluster view</p>
      </div>
    </div>
    
    <div class="filters-row">
      <div class="filter-group">
        <label>Cluster</label>
        <select id="clusterSelect"><option value="">All Clusters</option></select>
      </div>
      <div class="filter-group">
        <label>City</label>
        <select id="citySelect"><option value="">All Cities</option></select>
      </div>
      <button class="reset-btn" id="resetBtn">↻ Reset to MP Total</button>
    </div>
    
    <div class="scope-indicator" id="scopePill">
      <span class="dot"></span> <span id="scopeText">Showing: MP / Region Total (STATE level)</span>
    </div>
  </div>

  <div class="kpi-grid" id="kpiGrid">
    <!-- Rendered via JS -->
  </div>

  <div class="section" id="detailedSection">
    <div class="section-head">
      <h2>Detailed MSIL / NEXA / ARENA Market Share</h2>
      <p>RTO and City level breakdown (only visible when Cluster or City is selected)</p>
    </div>
    <div id="detailedTableBody">
      <!-- Rendered via JS -->
    </div>
  </div>
</div>

<!-- Preserved JSON Data -->
<script type="application/json" id="vahan-data-json">
{"oemOrder": ["MSIL", "NEXA", "ARENA", "TATA", "HYUNDAI", "KIA", "MAHINDRA", "TOYOTA", "HONDA", "SKODA", "MG", "RENAULT", "NISSAN", "OTHERS"], "oemDisplay": {"MSIL": "MSIL", "NEXA": "Nexa", "ARENA": "Arena", "TATA": "Tata", "HYUNDAI": "Hyundai", "KIA": "Kia", "MAHINDRA": "Mahindra", "TOYOTA": "Toyota", "HONDA": "Honda", "SKODA": "Skoda", "MG": "MG", "RENAULT": "Renault", "NISSAN": "Nissan", "OTHERS": "Others"}, "fuels": ["CNG", "DIESEL", "EV", "PETROL"], "state": {"name": "MP / Region Total", "level": "STATE", "main": {"sep_vol": {"MSIL": 1871, "NEXA": 373.10263929618765, "ARENA": 1497.8973607038124, "TATA": 641, "HYUNDAI": 503, "KIA": 252, "MAHINDRA": 658, "TOYOTA": 180, "HONDA": 29, "SKODA": 60, "MG": 49, "RENAULT": 32, "NISSAN": 44, "OTHERS": 26, "INDUSTRY": 4345, "ARENA_REG": 819, "NEXA_REG": 204, "NEXA_CONTRI": 0.19941348973607037, "NEXA_GAP": 203.89736070381235}, "sep_ms": {"MSIL": 0.4306098964326812, "NEXA": 0.0858694221625288, "ARENA": 0.3447404742701525, "TATA": 0.1475258918296893, "HYUNDAI": 0.11576524741081703, "KIA": 0.05799769850402762, "MAHINDRA": 0.1514384349827388, "TOYOTA": 0.04142692750287687, "HONDA": 0.0066743383199079406, "SKODA": 0.01380897583429229, "MG": 0.011277330264672036, "RENAULT": 0.007364787111622554, "NISSAN": 0.010126582278481013, "OTHERS": 0.005983889528193326, "TOTAL": 1}, "sep_ly_vol": {"MSIL": 6400, "NEXA": 1861.5288999378497, "ARENA": 4538.47110006215, "TATA": 2335, "HYUNDAI": 1603, "KIA": 745, "MAHINDRA": 1995, "TOYOTA": 470, "HONDA": 73, "SKODA": 156, "MG": 108, "RENAULT": 79, "NISSAN": 34, "OTHERS": 20, "TOTAL": 14018, "ARENA_REG": 4564, "NEXA_REG": 1872, "NEXA_CONTRI": 0.290863890615289}, "sep_ly_ms": {"MSIL": 0.45655585675559995, "NEXA": 0.13279561277913038, "ARENA": 0.3237602439764695, "TATA": 0.1665715508631759, "HYUNDAI": 0.11435297474675417, "KIA": 0.053145955200456556, "MAHINDRA": 0.14231702097303467, "TOYOTA": 0.03352832073048937, "HONDA": 0.0052075902411185615, "SKODA": 0.011128549008417749, "MG": 0.007704380082750749, "RENAULT": 0.0056356113568269365, "NISSAN": 0.0024254529890141248, "OTHERS": 0.0014267370523612499, "TOTAL": 1}, "sep_growth": {"MSIL": -0.70765625, "NEXA": -0.7995719328834249, "ARENA": -0.6699555141635032, "TATA": -0.7254817987152034, "HYUNDAI": -0.6862133499688086, "KIA": -0.661744966442953, "MAHINDRA": -0.6701754385964912, "TOYOTA": -0.6170212765957447, "HONDA": -0.6027397260273972, "SKODA": -0.6153846153846154, "MG": -0.5462962962962963, "RENAULT": -0.5949367088607596, "NISSAN": 0.2941176470588236, "OTHERS": 0.30000000000000004, "TOTAL": -0.6900413753745185}, "sep_ms_chg": {"MSIL": -0.025945960322918737, "NEXA": -0.04692619061660158, "ARENA": 0.020980230293682955, "TATA": -0.019045659033486595, "HYUNDAI": 0.0014122726640628663, "KIA": 0.004851743303571063, "MAHINDRA": 0.00912141400970412, "TOYOTA": 0.007898606772387497, "HONDA": 0.001466748078789379, "SKODA": 0.0026804268258745403, "MG": 0.003572950181921287, "RENAULT": 0.0017291757547956178, "NISSAN": 0.007701129289466888, "OTHERS": 0.004557152475832076, "TOTAL": 0}, "till_vol": {"MSIL": 32901, "NEXA": 9363.649619771862, "ARENA": 23537.350380228138, "TATA": 12286, "HYUNDAI": 8872, "KIA": 4666, "MAHINDRA": 11634, "TOYOTA": 3238, "HONDA": 462, "SKODA": 836, "MG": 709, "RENAULT": 577, "NISSAN": 540, "OTHERS": 273, "INDUSTRY": 76994, "ARENA_REG": 22578, "NEXA_REG": 8982, "NEXA_CONTRI": 0.28460076045627375, "NEXA_GAP": -375.6496197718625}, "till_ms": {"MSIL": 0.42731901187105487, "NEXA": 0.12161531573592568, "ARENA": 0.3057036961351292, "TATA": 0.15957087565264827, "HYUNDAI": 0.11522975816297373, "KIA": 0.06060212484089669, "MAHINDRA": 0.15110268332597346, "TOYOTA": 0.04205522508247396, "HONDA": 0.006000467568901473, "SKODA": 0.010857988934202666, "MG": 0.009208509754006805, "RENAULT": 0.007494090448606385, "NISSAN": 0.007013533522092631, "OTHERS": 0.003545730836169052, "TOTAL": 1}, "till_ly_vol": {"MSIL": 32792, "NEXA": 9131.030945958233, "ARENA": 23660.969054041765, "TATA": 11659, "HYUNDAI": 9525, "KIA": 4813, "MAHINDRA": 12915, "TOYOTA": 3254, "HONDA": 509, "SKODA": 940, "MG": 926, "RENAULT": 510, "NISSAN": 264, "OTHERS": 110, "TOTAL": 78217, "ARENA_REG": 23806, "NEXA_REG": 9187, "NEXA_CONTRI": 0.2784530051829176}, "till_ly_ms": {"MSIL": 0.4192438983852615, "NEXA": 0.11673972340997779, "ARENA": 0.3025041749752837, "TATA": 0.14905966733574544, "HYUNDAI": 0.1217765958806909, "KIA": 0.061533937635040975, "MAHINDRA": 0.1651175575642124, "TOYOTA": 0.041602209238400856, "HONDA": 0.006507536724752931, "SKODA": 0.012017847782451385, "MG": 0.011838858560159557, "RENAULT": 0.006520321669202347, "NISSAN": 0.003375225334645921, "OTHERS": 0.0014063438894358004, "TOTAL": 1}, "till_growth": {"MSIL": 0.0033239814588923444, "NEXA": 0.02547561991525127, "ARENA": -0.005224582033444225, "TATA": 0.05377819710095211, "HYUNDAI": -0.0685564304461942, "KIA": -0.030542281321421116, "MAHINDRA": -0.09918699186991875, "TOYOTA": -0.0049170251997541214, "HONDA": -0.09233791748526521, "SKODA": -0.11063829787234047, "MG": -0.234341252699784, "RENAULT": 0.13137254901960782, "NISSAN": 1.0454545454545454, "OTHERS": 1.481818181818182, "TOTAL": -0.01563598706163627}, "till_ms_chg": {"MSIL": 0.008075113485793373, "NEXA": 0.004875592325947892, "ARENA": 0.0031995211598455087, "TATA": 0.010511208316902831, "HYUNDAI": -0.006546837717717163, "KIA": -0.0009318127941442822, "MAHINDRA": -0.014014874238238939, "TOYOTA": 0.00045301584407310413, "HONDA": -0.0005070691558514582, "SKODA": -0.0011598588482487193, "MG": -0.0026303488061527516, "RENAULT": 0.0009737687794040377, "NISSAN": 0.00363830818744671, "OTHERS": 0.0021393869467332518, "TOTAL": 0}}, "clusters": [{"name": "INDORE CLUSTER", "level": "CLUSTER", "cities": [{"name": "INDORE CITY", "level": "CITY", "rtos": [{"name": "INDORE", "rto": "MP09", "level": "RTO", "main": {"sep_vol": {"MSIL": 517, "NEXA": 73.62341772151899, "ARENA": 443.376582278481}, "sep_ms": {"MSIL": 0.4449, "NEXA": 0.0633, "ARENA": 0.3815}, "sep_ly_ms": {"MSIL": 0.4763, "NEXA": 0.1178, "ARENA": 0.3584}, "sep_ms_chg": {"MSIL": -0.0314, "NEXA": -0.0544, "ARENA": 0.023}, "till_vol": {"MSIL": 8400, "NEXA": 2307.35, "ARENA": 6092.64}, "till_ms": {"MSIL": 0.4470, "NEXA": 0.1227, "ARENA": 0.3242}, "till_ly_ms": {"MSIL": 0.4360, "NEXA": 0.1032, "ARENA": 0.3328}, "till_ms_chg": {"MSIL": 0.011, "NEXA": 0.0195, "ARENA": -0.0085}}}]}]}]}
</script>

<script>
document.addEventListener("DOMContentLoaded", () => {
  // 1. Safe JSON Parsing handling the truncation bug
  let rawJson = document.getElementById('vahan-data-json').textContent.trim();
  
  // Robust check to fix truncated brackets if they exist
  if (!rawJson.endsWith("}")) {
      rawJson += "]}]}]}";
  }

  let vahanData;
  try {
    vahanData = JSON.parse(rawJson);
  } catch(e) {
    console.error("Failed to parse JSON payload:", e);
    document.getElementById("kpiGrid").innerHTML = `<div style="color:red; font-weight:bold; padding:20px;">Error parsing data payload.</div>`;
    return;
  }

  // Define strict focus OEMs
  const FOCUS_OEMS = ["MSIL", "NEXA", "ARENA"];

  const clusterSelect = document.getElementById('clusterSelect');
  const citySelect = document.getElementById('citySelect');
  const resetBtn = document.getElementById('resetBtn');
  const scopeText = document.getElementById('scopeText');

  // Populate Dropdowns
  if (vahanData.clusters) {
    vahanData.clusters.forEach(c => {
      const opt = document.createElement('option');
      opt.value = c.name;
      opt.textContent = c.name;
      clusterSelect.appendChild(opt);
    });
  }

  function updateCityOptions() {
    citySelect.innerHTML = '<option value="">All Cities</option>';
    const clusterVal = clusterSelect.value;
    if (clusterVal) {
      const clusterNode = vahanData.clusters.find(c => c.name === clusterVal);
      if (clusterNode && clusterNode.cities) {
        clusterNode.cities.forEach(city => {
          const opt = document.createElement('option');
          opt.value = city.name;
          opt.textContent = city.name;
          citySelect.appendChild(opt);
        });
      }
    }
  }

  function getScopeData() {
    const clusterVal = clusterSelect.value;
    const cityVal = citySelect.value;
    
    let scopeName = "MP / Region Total";
    let scopeLevel = "STATE";
    let dataNode = vahanData.state;

    if (cityVal) {
      for(let c of (vahanData.clusters || [])) {
        let cityNode = (c.cities || []).find(x => x.name === cityVal);
        if(cityNode) { dataNode = cityNode; scopeName = cityNode.name; scopeLevel = "CITY"; break; }
      }
    } else if (clusterVal) {
      let clusterNode = (vahanData.clusters || []).find(x => x.name === clusterVal);
      if(clusterNode) { dataNode = clusterNode; scopeName = clusterNode.name; scopeLevel = "CLUSTER"; }
    }
    
    return { scopeName, scopeLevel, dataNode };
  }

  function renderKPIs(dataNode) {
    const grid = document.getElementById('kpiGrid');
    grid.innerHTML = '';

    const themes = { 'MSIL': '', 'NEXA': 'nexa', 'ARENA': 'arena' };
    
    FOCUS_OEMS.forEach(oem => {
      const main = dataNode.main;
      if (!main || !main.sep_ms || typeof main.sep_ms[oem] === 'undefined') return;

      const themeClass = themes[oem];

      // September Data
      const sepMs = (main.sep_ms[oem] || 0) * 100;
      const sepLyMs = (main.sep_ly_ms[oem] || 0) * 100;
      const sepVol = main.sep_vol[oem] || 0;
      const sepChg = (main.sep_ms_chg[oem] || 0) * 100;
      const sepPos = sepChg > 0;

      // YTD Data
      const tillMs = (main.till_ms[oem] || 0) * 100;
      const tillLyMs = (main.till_ly_ms[oem] || 0) * 100;
      const tillVol = main.till_vol[oem] || 0;
      const tillChg = (main.till_ms_chg[oem] || 0) * 100;
      const tillPos = tillChg > 0;

      // HTML template strictly matching provided screenshot structure
      grid.innerHTML += `
        <!-- SEPTEMBER CARD -->
        <div class="kpi-card ${themeClass}">
          <div class="kpi-header">${oem} MARKET SHARE — SEPTEMBER</div>
          <div class="kpi-value">${sepMs.toFixed(2)}%</div>
          <div class="kpi-meta">Volume: ${Math.round(sepVol)} units &middot; LY MS%: ${sepLyMs.toFixed(2)}%</div>
          <div class="kpi-badge-row">
            <span class="badge ${sepPos ? 'pos' : (sepChg < 0 ? 'neg' : 'flat')}">
              ${sepPos ? '▲' : (sepChg < 0 ? '▼' : '')} ${Math.abs(sepChg).toFixed(2)} pp
            </span>
            <span>vs LY Sep</span>
          </div>
        </div>

        <!-- TILL SEPTEMBER CARD -->
        <div class="kpi-card ${themeClass}">
          <div class="kpi-header">${oem} MARKET SHARE — TILL SEPTEMBER (YTD)</div>
          <div class="kpi-value">${tillMs.toFixed(2)}%</div>
          <div class="kpi-meta">Volume: ${Math.round(tillVol)} units &middot; LY MS%: ${tillLyMs.toFixed(2)}%</div>
          <div class="kpi-badge-row">
            <span class="badge ${tillPos ? 'pos' : (tillChg < 0 ? 'neg' : 'flat')}">
              ${tillPos ? '▲' : (tillChg < 0 ? '▼' : '')} ${Math.abs(tillChg).toFixed(2)} pp
            </span>
            <span>vs LY Till Sep</span>
          </div>
        </div>
      `;
    });
  }

  function renderDetailedTable(dataNode, scopeLevel) {
    const tableBody = document.getElementById('detailedTableBody');
    tableBody.innerHTML = '';

    let children = [];
    if (scopeLevel === 'CLUSTER') children = dataNode.cities || [];
    else if (scopeLevel === 'CITY') children = dataNode.rtos || [];

    if (children.length === 0) {
      tableBody.innerHTML = '<div class="empty-state">No detailed breakdown available.</div>';
      return;
    }

    let html = '';
    children.forEach(child => {
      html += `<div class="location-title">${child.name} (${child.level})</div>`;
      html += `<div class="table-container"><table>
                <thead>
                  <tr>
                    <th>OEM</th>
                    <th class="num">Sep Vol</th>
                    <th class="num">Sep MS%</th>
                    <th class="num">Sep LY MS%</th>
                    <th class="num">Sep Chg (pp)</th>
                    <th class="num">Till Vol</th>
                    <th class="num">Till MS%</th>
                    <th class="num">Till Chg (pp)</th>
                  </tr>
                </thead>
                <tbody>`;
      
      FOCUS_OEMS.forEach(oem => {
        const main = child.main;
        if(!main || !main.sep_vol || typeof main.sep_vol[oem] === 'undefined') return;

        const sepChg = (main.sep_ms_chg[oem] || 0) * 100;
        const tillChg = (main.till_ms_chg[oem] || 0) * 100;

        html += `
          <tr>
            <td class="oem-name">${oem}</td>
            <td class="num">${Math.round(main.sep_vol[oem] || 0)}</td>
            <td class="num">${((main.sep_ms[oem] || 0) * 100).toFixed(2)}%</td>
            <td class="num" style="color:var(--muted)">${((main.sep_ly_ms[oem] || 0) * 100).toFixed(2)}%</td>
            <td class="num chg-txt ${sepChg > 0 ? 'pos' : (sepChg < 0 ? 'neg' : '')}">
              ${sepChg > 0 ? '▲' : (sepChg < 0 ? '▼' : '')} ${Math.abs(sepChg).toFixed(2)}
            </td>
            
            <td class="num">${Math.round(main.till_vol[oem] || 0)}</td>
            <td class="num">${((main.till_ms[oem] || 0) * 100).toFixed(2)}%</td>
            <td class="num chg-txt ${tillChg > 0 ? 'pos' : (tillChg < 0 ? 'neg' : '')}">
              ${tillChg > 0 ? '▲' : (tillChg < 0 ? '▼' : '')} ${Math.abs(tillChg).toFixed(2)}
            </td>
          </tr>
        `;
      });
      html += `</tbody></table></div>`;
    });

    tableBody.innerHTML = html;
  }

  function updateDashboard() {
    const { scopeName, scopeLevel, dataNode } = getScopeData();

    // Update Pill Text
    scopeText.textContent = `Showing: ${scopeName} (${scopeLevel} level)`;

    // Render KPIs
    renderKPIs(dataNode);

    // Conditional Table Rendering
    const detailsSection = document.getElementById('detailedSection');
    if (scopeLevel === 'STATE') {
      detailsSection.style.display = 'none';
    } else {
      detailsSection.style.display = 'block';
      renderDetailedTable(dataNode, scopeLevel);
    }
  }

  // Event Listeners
  clusterSelect.addEventListener('change', () => {
    updateCityOptions();
    updateDashboard();
  });

  citySelect.addEventListener('change', updateDashboard);

  resetBtn.addEventListener('click', () => {
    clusterSelect.value = "";
    updateCityOptions();
    updateDashboard();
  });

  // Init
  updateDashboard();
});
</script>

</body>
</html>
