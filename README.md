<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>375 Bar · BEO</title>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
<style>
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&family=Playfair+Display:wght@400;500&display=swap');

:root {
  --bg:        #0e0e0c;
  --surface:   #161614;
  --surface2:  #1e1e1b;
  --surface3:  #252521;
  --border:    #2e2e29;
  --border2:   #3a3a34;
  --gold:      #c8a96e;
  --gold-dim:  #8b6f3f;
  --gold-bg:   #1c1810;
  --text:      #f0ece4;
  --text2:     #a09880;
  --text3:     #5a5648;
  --red:       #cc4444;
  --radius:    6px;
}

* { box-sizing: border-box; margin: 0; padding: 0; }
html, body { height: 100%; }
body {
  background: var(--bg);
  color: var(--text);
  font-family: 'Inter', sans-serif;
  font-size: 13px;
  line-height: 1.5;
  min-height: 100vh;
}

/* ══ SCROLLBAR ══ */
::-webkit-scrollbar { width: 6px; }
::-webkit-scrollbar-track { background: var(--bg); }
::-webkit-scrollbar-thumb { background: var(--border2); border-radius: 3px; }

/* ══ PASSWORD SCREEN ══ */
#lockScreen {
  position: fixed; inset: 0;
  background: var(--bg);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 1000;
  flex-direction: column;
  gap: 0;
}
.lock-box {
  width: 360px;
  text-align: center;
}
.lock-logo {
  font-family: 'Playfair Display', serif;
  font-size: 28px;
  font-weight: 400;
  color: var(--text);
  letter-spacing: 0.08em;
  margin-bottom: 4px;
}
.lock-sub {
  font-size: 10px;
  letter-spacing: 0.2em;
  text-transform: uppercase;
  color: var(--gold);
  margin-bottom: 48px;
}
.lock-title {
  font-size: 12px;
  letter-spacing: 0.12em;
  text-transform: uppercase;
  color: var(--text3);
  margin-bottom: 16px;
}
.lock-input {
  width: 100%;
  background: var(--surface2);
  border: 1px solid var(--border2);
  border-radius: var(--radius);
  color: var(--text);
  font-size: 16px;
  font-family: inherit;
  padding: 14px 18px;
  text-align: center;
  letter-spacing: 0.2em;
  outline: none;
  transition: border-color 0.2s;
  margin-bottom: 12px;
}
.lock-input:focus { border-color: var(--gold); }
.lock-input.error { border-color: var(--red); }
.lock-btn {
  width: 100%;
  background: var(--gold);
  color: #0e0e0c;
  border: none;
  border-radius: var(--radius);
  font-size: 11px;
  font-weight: 700;
  letter-spacing: 0.14em;
  text-transform: uppercase;
  padding: 14px;
  cursor: pointer;
  font-family: inherit;
  transition: background 0.2s;
}
.lock-btn:hover { background: #d4b87a; }
.lock-error {
  font-size: 11px;
  color: var(--red);
  margin-top: 10px;
  height: 16px;
}
.lock-divider {
  width: 40px; height: 1px;
  background: var(--border2);
  margin: 32px auto;
}

/* ══ APP SHELL ══ */
#app { display: none; flex-direction: column; min-height: 100vh; }
#app.visible { display: flex; }

/* ══ TOP NAV ══ */
.topnav {
  background: var(--surface);
  border-bottom: 1px solid var(--border);
  padding: 0 32px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  height: 56px;
  position: sticky;
  top: 0;
  z-index: 50;
}
.nav-brand {
  font-family: 'Playfair Display', serif;
  font-size: 16px;
  letter-spacing: 0.06em;
  color: var(--text);
  display: flex;
  align-items: center;
  gap: 12px;
}
.nav-brand .dot {
  width: 6px; height: 6px;
  border-radius: 50%;
  background: var(--gold);
}
.nav-tabs {
  display: flex;
  gap: 0;
}
.nav-tab {
  padding: 0 18px;
  height: 56px;
  display: flex;
  align-items: center;
  font-size: 11px;
  font-weight: 600;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  color: var(--text3);
  border: none;
  background: none;
  cursor: pointer;
  border-bottom: 2px solid transparent;
  transition: all 0.15s;
  font-family: inherit;
}
.nav-tab:hover { color: var(--text2); }
.nav-tab.active { color: var(--gold); border-bottom-color: var(--gold); }
.nav-actions { display: flex; gap: 8px; align-items: center; }
.btn-sm {
  padding: 7px 14px;
  border-radius: var(--radius);
  font-size: 10px;
  font-weight: 600;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  cursor: pointer;
  font-family: inherit;
  transition: all 0.15s;
  border: none;
}
.btn-outline {
  background: none;
  border: 1px solid var(--border2);
  color: var(--text2);
}
.btn-outline:hover { border-color: var(--text2); color: var(--text); }
.btn-gold {
  background: var(--gold);
  color: #0e0e0c;
  font-weight: 700;
}
.btn-gold:hover { background: #d4b87a; }

/* ══ MAIN CONTENT ══ */
.main { flex: 1; overflow-y: auto; }
.pane { display: none; }
.pane.active { display: block; }

/* ══ INPUT FORM ══ */
.form-wrap {
  max-width: 760px;
  margin: 0 auto;
  padding: 40px 24px 80px;
}
.form-page-title {
  margin-bottom: 32px;
  padding-bottom: 20px;
  border-bottom: 1px solid var(--border);
  display: flex;
  justify-content: space-between;
  align-items: flex-end;
}
.form-page-title h1 {
  font-family: 'Playfair Display', serif;
  font-size: 26px;
  font-weight: 400;
  color: var(--text);
  letter-spacing: 0.02em;
}
.form-page-title .hint {
  font-size: 11px;
  color: var(--text3);
}

.form-block {
  background: var(--surface);
  border: 1px solid var(--border);
  border-radius: var(--radius);
  margin-bottom: 16px;
  overflow: hidden;
}
.form-block-header {
  padding: 12px 20px;
  border-bottom: 1px solid var(--border);
  font-size: 9px;
  font-weight: 700;
  letter-spacing: 0.16em;
  text-transform: uppercase;
  color: var(--gold);
  background: var(--gold-bg);
}

.field-row {
  display: grid;
  border-bottom: 1px solid var(--border);
}
.field-row.cols-2 { grid-template-columns: 1fr 1fr; }
.field-row.cols-1 { grid-template-columns: 1fr; }
.field-row:last-child { border-bottom: none; }

.field {
  padding: 14px 20px;
  border-right: 1px solid var(--border);
  display: flex;
  flex-direction: column;
  gap: 5px;
}
.field:last-child { border-right: none; }
.field label {
  font-size: 9px;
  font-weight: 600;
  letter-spacing: 0.12em;
  text-transform: uppercase;
  color: var(--text3);
}
.field input, .field select {
  background: none;
  border: none;
  outline: none;
  color: var(--text);
  font-size: 13px;
  font-family: inherit;
  padding: 0;
  width: 100%;
}
.field input::placeholder { color: var(--text3); }
.field select option { background: var(--surface2); }

/* Cocktail rows */
.c-table { width: 100%; border-collapse: collapse; }
.c-table th {
  font-size: 9px;
  font-weight: 600;
  letter-spacing: 0.12em;
  text-transform: uppercase;
  color: var(--text3);
  padding: 10px 16px;
  text-align: left;
  background: var(--surface2);
  border-bottom: 1px solid var(--border);
}
.c-table td {
  border-bottom: 1px solid var(--border);
  vertical-align: middle;
  padding: 0;
}
.c-table tr:last-child td { border-bottom: none; }
.c-table .rnum {
  font-size: 11px;
  color: var(--text3);
  text-align: center;
  padding: 12px 16px;
  width: 40px;
  font-weight: 500;
}
.c-table select, .c-table input[type=number] {
  width: 100%;
  background: none;
  border: none;
  outline: none;
  color: var(--text);
  font-size: 13px;
  font-family: inherit;
  padding: 12px 16px;
  cursor: pointer;
}
.c-table select option { background: var(--surface2); }
.c-table tr:hover td { background: var(--surface2); }

.form-actions {
  display: flex;
  gap: 10px;
  margin-top: 24px;
}
.btn-lg {
  padding: 12px 28px;
  border-radius: var(--radius);
  font-size: 11px;
  font-weight: 700;
  letter-spacing: 0.12em;
  text-transform: uppercase;
  cursor: pointer;
  font-family: inherit;
  transition: all 0.15s;
  border: none;
}
.btn-lg.gold { background: var(--gold); color: #0e0e0c; }
.btn-lg.gold:hover { background: #d4b87a; }
.btn-lg.ghost {
  background: none;
  border: 1px solid var(--border2);
  color: var(--text2);
}
.btn-lg.ghost:hover { border-color: var(--text2); color: var(--text); }

/* ══ OUTPUT ══ */
.out-wrap {
  max-width: 800px;
  margin: 0 auto;
  padding: 40px 24px 80px;
}

/* Output doc */
.out-doc {
  background: var(--surface);
  border: 1px solid var(--border);
  border-radius: var(--radius);
  overflow: hidden;
}

.out-doc-header {
  background: var(--surface2);
  border-bottom: 1px solid var(--border);
  padding: 28px 32px;
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
}
.out-doc-header .left .brand-sm {
  font-size: 9px;
  font-weight: 700;
  letter-spacing: 0.2em;
  text-transform: uppercase;
  color: var(--gold);
  margin-bottom: 6px;
}
.out-doc-header .left h2 {
  font-family: 'Playfair Display', serif;
  font-size: 22px;
  font-weight: 400;
  color: var(--text);
  letter-spacing: 0.02em;
}
.out-doc-header .right {
  text-align: right;
  font-size: 11px;
  color: var(--text2);
  line-height: 2;
}
.out-doc-header .right strong { color: var(--text); }

.out-info-bar {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  border-bottom: 1px solid var(--border);
}
.out-info-cell {
  padding: 14px 20px;
  border-right: 1px solid var(--border);
}
.out-info-cell:last-child { border-right: none; }
.out-info-cell .lbl {
  font-size: 9px;
  font-weight: 600;
  letter-spacing: 0.12em;
  text-transform: uppercase;
  color: var(--text3);
  margin-bottom: 4px;
}
.out-info-cell .val {
  font-size: 14px;
  font-weight: 500;
  color: var(--text);
}
.out-info-cell.gold-cell { background: var(--gold-bg); }
.out-info-cell.gold-cell .val { color: var(--gold); font-weight: 700; }

.out-body { padding: 24px 32px; }

.out-section-hdr {
  font-size: 9px;
  font-weight: 700;
  letter-spacing: 0.16em;
  text-transform: uppercase;
  color: var(--gold);
  margin: 24px 0 12px;
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.out-section-hdr:first-child { margin-top: 0; }
.out-section-hdr span {
  font-size: 10px;
  font-weight: 400;
  color: var(--text3);
  letter-spacing: 0;
  text-transform: none;
}

/* Bar chips */
.bar-chips { display: flex; flex-wrap: wrap; gap: 8px; margin-bottom: 8px; }
.bar-chip {
  background: var(--surface2);
  border: 1px solid var(--border2);
  border-radius: var(--radius);
  padding: 8px 14px;
  font-size: 12px;
  font-weight: 500;
  display: flex;
  align-items: center;
  gap: 10px;
  color: var(--text2);
}
.bar-chip .qty {
  background: var(--gold);
  color: #0e0e0c;
  width: 20px; height: 20px;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 10px;
  font-weight: 700;
}

/* Cocktail cards */
.cocktail-card {
  background: var(--surface2);
  border: 1px solid var(--border);
  border-radius: var(--radius);
  margin-bottom: 14px;
  overflow: hidden;
}
.cc-head {
  padding: 14px 20px;
  border-bottom: 1px solid var(--border);
  display: flex;
  justify-content: space-between;
  align-items: center;
  background: var(--surface3);
}
.cc-head .cname {
  font-size: 13px;
  font-weight: 600;
  color: var(--text);
  letter-spacing: 0.03em;
}
.cc-head .cmeta {
  display: flex;
  gap: 12px;
  font-size: 11px;
  color: var(--text2);
  align-items: center;
}
.cc-head .tag {
  background: var(--gold-bg);
  border: 1px solid var(--gold-dim);
  color: var(--gold);
  font-size: 9px;
  font-weight: 700;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  padding: 3px 8px;
  border-radius: 3px;
}

.ing-tbl { width: 100%; border-collapse: collapse; }
.ing-tbl th {
  font-size: 9px;
  font-weight: 600;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  color: var(--text3);
  padding: 8px 20px;
  text-align: left;
  border-bottom: 1px solid var(--border);
}
.ing-tbl th.r { text-align: right; }
.ing-tbl td {
  padding: 9px 20px;
  font-size: 12px;
  border-bottom: 1px solid var(--border);
  color: var(--text2);
}
.ing-tbl tr:last-child td { border-bottom: none; }
.ing-tbl td.ing-name { color: var(--text); font-weight: 500; }
.ing-tbl td.r { text-align: right; }
.bottles-badge {
  display: inline-block;
  background: var(--gold);
  color: #0e0e0c;
  font-size: 10px;
  font-weight: 700;
  padding: 2px 9px;
  border-radius: 3px;
  letter-spacing: 0.04em;
}

.garnish-row {
  background: var(--gold-bg);
  border-top: 1px solid #2a2210;
  padding: 10px 20px;
  display: flex;
  align-items: center;
  gap: 16px;
  flex-wrap: wrap;
}
.garnish-row .g-lbl {
  font-size: 9px;
  font-weight: 700;
  letter-spacing: 0.14em;
  text-transform: uppercase;
  color: var(--gold-dim);
}
.garnish-item {
  display: flex;
  align-items: center;
  gap: 8px;
  font-size: 12px;
  color: var(--text2);
}
.garnish-item .g-count {
  background: rgba(200,169,110,0.15);
  color: var(--gold);
  font-size: 10px;
  font-weight: 700;
  padding: 2px 8px;
  border-radius: 3px;
}

.out-footer {
  margin-top: 24px;
  padding-top: 16px;
  border-top: 1px solid var(--border);
  display: flex;
  justify-content: space-between;
  font-size: 10px;
  color: var(--text3);
}

/* Empty state */
.empty-out {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  min-height: 60vh;
  color: var(--text3);
  text-align: center;
  gap: 12px;
}
.empty-out .icon { font-size: 40px; opacity: 0.3; }
.empty-out p { font-size: 12px; line-height: 1.7; }

/* ══ PRINT / PDF STYLES ══ */
@media print {
  body { background: #fff; color: #000; }
  .topnav, .form-actions, .nav-actions, #lockScreen { display: none !important; }
  .pane { display: block !important; }
  #pane-input { display: none !important; }
  .out-wrap { padding: 0; max-width: 100%; }
  .out-doc { border: none; background: #fff; }
  .out-doc-header { background: #f5f5f5; }
  .cocktail-card { background: #fafafa; border: 1px solid #ddd; }
  .cc-head { background: #f0f0f0; }
  ::-webkit-scrollbar { display: none; }
}
</style>
</head>
<body>

<!-- ══ LOCK SCREEN ══ -->
<div id="lockScreen">
  <div class="lock-box">
    <div class="lock-logo">375 Bar</div>
    <div class="lock-sub">Event Operations</div>
    <div class="lock-divider"></div>
    <div class="lock-title">Enter Access Code</div>
    <input type="password" class="lock-input" id="pwInput" placeholder="••••••••" maxlength="20">
    <button class="lock-btn" onclick="checkPassword()">Enter</button>
    <div class="lock-error" id="pwError"></div>
  </div>
</div>

<!-- ══ APP ══ -->
<div id="app">

  <nav class="topnav">
    <div class="nav-brand">
      <div class="dot"></div>
      375 Bar · BEO
    </div>
    <div class="nav-tabs">
      <button class="nav-tab active" onclick="switchPane('input')" id="tab-input">Input Form</button>
      <button class="nav-tab" onclick="switchPane('output')" id="tab-output">Batch Output</button>
    </div>
    <div class="nav-actions">
      <button class="btn-sm btn-outline" onclick="clearForm()">Clear Form</button>
      <button class="btn-sm btn-gold" onclick="generateAndSwitch()">Generate Output</button>
    </div>
  </nav>

  <div class="main">

    <!-- INPUT PANE -->
    <div class="pane active" id="pane-input">
    <div class="form-wrap">

      <div class="form-page-title">
        <h1>Banquet Event Order</h1>
        <span class="hint">All fields used to calculate batch recipes</span>
      </div>

      <!-- Event Info -->
      <div class="form-block">
        <div class="form-block-header">Event Information</div>
        <div class="field-row cols-2">
          <div class="field">
            <label>Event Name</label>
            <input type="text" id="i_name" placeholder="e.g. Smith Wedding">
          </div>
          <div class="field">
            <label>Event Date</label>
            <input type="text" id="i_date" placeholder="MM / DD / YYYY">
          </div>
        </div>
        <div class="field-row cols-2">
          <div class="field">
            <label>Event Time</label>
            <input type="text" id="i_time" placeholder="e.g. 7:00 PM">
          </div>
          <div class="field">
            <label>Guest Count</label>
            <input type="number" id="i_guests" placeholder="0" min="1">
          </div>
        </div>
        <div class="field-row cols-2">
          <div class="field">
            <label>Event Type</label>
            <select id="i_type">
              <option value="">— Select —</option>
              <option>Corporate</option>
              <option>Social</option>
            </select>
          </div>
          <div class="field">
            <label>Tequila Upgrade</label>
            <select id="i_tequila">
              <option>No</option>
              <option>Yes</option>
            </select>
          </div>
        </div>
        <div class="field-row cols-1">
          <div class="field">
            <label>Location</label>
            <input type="text" id="i_location" placeholder="Pool, Rooftop, Main Room...">
          </div>
        </div>
      </div>

      <!-- Cocktail Selection -->
      <div class="form-block">
        <div class="form-block-header">Cocktail Selection — Select up to 8 <span id="loadingMsg" style="font-weight:400;font-style:italic;margin-left:12px;font-size:10px;letter-spacing:0"></span></div>
        <table class="c-table">
          <thead>
            <tr>
              <th style="width:40px">#</th>
              <th>Cocktail</th>
              <th style="width:160px">Service Type</th>
            </tr>
          </thead>
          <tbody id="cocktailRows"></tbody>
        </table>
      </div>

      <!-- Bar Setup -->
      <div class="form-block">
        <div class="form-block-header">Bar Setup — Select up to 6</div>
        <table class="c-table">
          <thead>
            <tr>
              <th style="width:40px">#</th>
              <th>Bar Type</th>
              <th style="width:100px">Qty</th>
            </tr>
          </thead>
          <tbody id="barRows"></tbody>
        </table>
      </div>

      <div class="form-actions">
        <button class="btn-lg gold" onclick="generateAndSwitch()">Generate Batch Output</button>
        <button class="btn-lg ghost" onclick="clearForm()">Clear Form</button>
      </div>

    </div>
    </div>

    <!-- OUTPUT PANE -->
    <div class="pane" id="pane-output">
    <div class="out-wrap">

      <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:20px;">
        <div style="font-size:11px;color:var(--text3)" id="out-meta"></div>
        <div style="display:flex;gap:8px;">
          <button class="btn-sm btn-outline" onclick="window.print()">🖨 Print</button>
          <button class="btn-sm btn-gold" onclick="downloadPDF()">↓ Download PDF</button>
        </div>
      </div>

      <div id="outputDoc">
        <div class="empty-out">
          <div class="icon">🍸</div>
          <p>Fill in the event details on the Input Form<br>and click <strong style="color:var(--text2)">Generate Batch Output</strong></p>
        </div>
      </div>

    </div>
    </div>

  </div>
</div>

<script>
// ══ PASSWORD ══════════════════════════════════════════════════
// Change this to your preferred password
const PASSWORD = "375bar2026";

function checkPassword() {
  const val = document.getElementById('pwInput').value;
  const inp = document.getElementById('pwInput');
  const err = document.getElementById('pwError');
  if (val === PASSWORD) {
    document.getElementById('lockScreen').style.display = 'none';
    document.getElementById('app').classList.add('visible');
    loadMasterSheet();
  } else {
    inp.classList.add('error');
    err.textContent = 'Incorrect access code. Please try again.';
    inp.value = '';
    setTimeout(() => { inp.classList.remove('error'); err.textContent = ''; }, 3000);
  }
}
document.addEventListener('keydown', e => {
  if (e.key === 'Enter' && document.getElementById('lockScreen').style.display !== 'none') {
    checkPassword();
  }
});

// ══ MASTER RECIPE DATA — loaded live from Google Sheet ════════
// Sheet ID and tab name
const SHEET_ID  = "1kkXEBh3ACAPsGiwl4M1Cp4tUa7UpQLfAUkgGjaggGB8";
const SHEET_TAB = "MASTER SHEET";

let MASTER = [];  // populated by loadMasterSheet()

async function loadMasterSheet() {
  const url = `https://docs.google.com/spreadsheets/d/${SHEET_ID}/gviz/tq?tqx=out:csv&sheet=${encodeURIComponent(SHEET_TAB)}`;
  showLoadingState(true);
  try {
    const res  = await fetch(url);
    const text = await res.text();
    MASTER = parseCSV(text);
    buildRows();
  } catch (e) {
    showLoadingState(false, 'Could not load recipe data. Check the sheet is set to Anyone with the link can view.');
    console.error(e);
  }
  showLoadingState(false);
}

function showLoadingState(loading, error) {
  const el = document.getElementById('loadingMsg');
  if (!el) return;
  if (error) { el.textContent = '⚠ ' + error; el.style.color = 'var(--red)'; }
  else if (loading) { el.textContent = 'Loading recipe data from Google Sheets...'; el.style.color = 'var(--text3)'; }
  else { el.textContent = ''; }
}

function parseCSV(text) {
  // Parse CSV — handles quoted fields
  const lines = text.trim().split('\n');
  const rows  = lines.map(line => {
    const cols = [];
    let cur = '', inQ = false;
    for (let i = 0; i < line.length; i++) {
      const c = line[i];
      if (c === '"') { inQ = !inQ; }
      else if (c === ',' && !inQ) { cols.push(cur.trim()); cur = ''; }
      else { cur += c; }
    }
    cols.push(cur.trim());
    return cols;
  });

  // Skip header row
  // Cols: 0=ID, 1=Cocktail, 2=Ingredient, 3=PerServing, 4=Unit,
  //       5=Order, 6=ContainerType, 7=ContainerSize,
  //       8=Garnish1, 9=Garnish2, 10=Glassware, 11=PopScore
  const recipes = {};
  for (let i = 1; i < rows.length; i++) {
    const r    = rows[i];
    const name = (r[1] || '').trim().toUpperCase();
    if (!name) continue;

    if (!recipes[name]) {
      recipes[name] = {
        name      : name,
        popScore  : parseFloat(r[11]) || 0.5,
        glassware : (r[10] || '').trim(),
        garnish1  : (r[8]  || '').trim(),
        garnish2  : (r[9]  || '').trim(),
        ingredients: []
      };
    }
    const oz = parseFloat(r[3]) || 0;
    const cs = parseFloat(r[7]) || 1;
    if (oz > 0) {
      recipes[name].ingredients.push({
        name: (r[2] || '').trim(),
        oz  : oz,
        ct  : (r[6] || '').trim(),
        cs  : cs,
        ord : parseInt(r[5]) || 0
      });
    }
  }

  // Sort ingredients by order field within each recipe
  return Object.values(recipes).map(rec => {
    rec.ingredients.sort((a, b) => a.ord - b.ord);
    return rec;
  }).sort((a, b) => a.name.localeCompare(b.name));
}

// Legacy placeholder so nothing breaks before load
const MASTER_PLACEHOLDER = [];
  {name:"GRILL OLD FASHIONED",popScore:0.6,glassware:"ROCKS",garnish1:"LEMON TWIST",garnish2:"",ingredients:[
    {name:"MAKERS MARK BOURBON",oz:2,ct:"LITER BOTTLE",cs:33.8},
    {name:"MAKERS SEAGRAM BARREL",oz:1,ct:"750ML BOTTLE",cs:25.36},
    {name:"CANE SYRUP",oz:0.25,ct:"QUART BOTTLE",cs:32},
    {name:"ANGOSTURA",oz:0.0625,ct:"OZ",cs:1}]},
  {name:"CARBONE NEGRONI",popScore:0.6,glassware:"ROCKS",garnish1:"LEMON TWIST",garnish2:"HALF ORANGE WHEEL",ingredients:[
    {name:"CAMPARI",oz:1.5,ct:"LITER BOTTLE",cs:33.8},
    {name:"BOATYARD GIN",oz:1,ct:"750ML BOTTLE",cs:25.36},
    {name:"CARPANO ANTICA",oz:1,ct:"LITER BOTTLE",cs:33.8},
    {name:"PUNT E MES",oz:0.25,ct:"750ML BOTTLE",cs:25.36}]},
  {name:"ROCCO MARGARITA",popScore:1,glassware:"ROCKS",garnish1:"HALF GRAPEFRUIT WHEEL",garnish2:"",ingredients:[
    {name:"PUEBLO VIEJO BLANCO",oz:2,ct:"LITER BOTTLE",cs:33.8},
    {name:"LIME JUICE",oz:0.75,ct:"QUART BOTTLE",cs:32},
    {name:"MANDARINE NAPOLEON",oz:0.5,ct:"750ML BOTTLE",cs:25.36},
    {name:"HONEY SYRUP",oz:0.25,ct:"QUART BOTTLE",cs:32},
    {name:"GRAPEFRUIT BITTERS",oz:0.042,ct:"OZ",cs:1}]},
  {name:"SPRITZ TORRISI",popScore:0.6,glassware:"HIGHBALL",garnish1:"ORANGE WHEEL",garnish2:"",ingredients:[
    {name:"APEROL",oz:2,ct:"LITER BOTTLE",cs:33.8},
    {name:"GIFFARD RHUBARB",oz:0.5,ct:"750ML BOTTLE",cs:25.36},
    {name:"GIFFARD PECHE",oz:0.25,ct:"750ML BOTTLE",cs:25.36},
    {name:"LEMON JUICE",oz:0.25,ct:"QUART BOTTLE",cs:32}]},
  {name:"BELMONDO",popScore:0.6,glassware:"ROCK",garnish1:"LEMON WHEEL",garnish2:"",ingredients:[
    {name:"PUEBLO VIEJO BLANCO",oz:1,ct:"LITER BOTTLE",cs:33.8},
    {name:"SCHLADERER PEAR",oz:1,ct:"750ML BOTTLE",cs:25.36},
    {name:"LEMON JUICE",oz:0.75,ct:"QUART BOTTLE",cs:32},
    {name:"SALERS",oz:0.25,ct:"750ML BOTTLE",cs:25.36},
    {name:"HONEY SYRUP",oz:0.5,ct:"QUART BOTTLE",cs:32},
    {name:"GINGER SYRUP",oz:0.25,ct:"QUART BOTTLE",cs:32}]},
  {name:"ELDERFLOWER SPRITZ",popScore:0.7,glassware:"AP WINE",garnish1:"HALF LEMON WHEEL",garnish2:"",ingredients:[
    {name:"ST GERMAIN",oz:1.5,ct:"750ML BOTTLE",cs:25.36},
    {name:"BOLS TRIPLE SEC",oz:0.5,ct:"LITER BOTTLE",cs:33.8},
    {name:"GIFFARD PECHE",oz:0.5,ct:"750ML BOTTLE",cs:25.36}]},
  {name:"CALABRIAN COSMO",popScore:0.5,glassware:"COUPE",garnish1:"LIME WHEEL",garnish2:"",ingredients:[
    {name:"GREY GOOSE",oz:2,ct:"LITER BOTTLE",cs:33.8},
    {name:"LIME JUICE",oz:1,ct:"QUART BOTTLE",cs:32},
    {name:"RED PEPPER CORDIAL",oz:0.75,ct:"QUART BOTTLE",cs:32},
    {name:"BOLS TRIPLE SEC",oz:0.5,ct:"LITER BOTTLE",cs:33.8},
    {name:"APEROL",oz:0.5,ct:"LITER BOTTLE",cs:33.8}]},
  {name:"GOLD DOLLAR",popScore:0.5,glassware:"HIGHBALL",garnish1:"MINT CROWN",garnish2:"",ingredients:[
    {name:"PUEBLO VIEJO BLANCO",oz:1.5,ct:"LITER BOTTLE",cs:33.8},
    {name:"CAZADORES REPOSADO",oz:0.5,ct:"LITER BOTTLE",cs:33.8},
    {name:"PLANTATION PINEAPPLE",oz:0.5,ct:"750ML BOTTLE",cs:25.36},
    {name:"PASSIONFRUIT CORDIAL",oz:2,ct:"QUART BOTTLE",cs:32},
    {name:"LEMON JUICE",oz:0.25,ct:"QUART BOTTLE",cs:32}]},
  {name:"THE MARTINI",popScore:0.6,glassware:"MARTINI",garnish1:"LEMON TWIST",garnish2:"OLIVE",ingredients:[
    {name:"BOATYARD GIN",oz:3,ct:"750ML BOTTLE",cs:25.36},
    {name:"NOILLY PRAT DRY",oz:0.5,ct:"LITER BOTTLE",cs:33.8}]},
  {name:"KANGAROO",popScore:0.6,glassware:"MARTINI",garnish1:"LEMON TWIST",garnish2:"OLIVE",ingredients:[
    {name:"GREY GOOSE",oz:3,ct:"LITER BOTTLE",cs:33.8},
    {name:"NOILLY PRAT DRY",oz:0.25,ct:"LITER BOTTLE",cs:33.8}]},
  {name:"CHAMPAGNE COCKTAIL",popScore:0.7,glassware:"COUPE",garnish1:"",garnish2:"",ingredients:[
    {name:"GREY GOOSE",oz:1,ct:"LITER BOTTLE",cs:33.8},
    {name:"SAUTERNE",oz:0.5,ct:"750ML BOTTLE",cs:25.36},
    {name:"ST GERMAIN",oz:0.25,ct:"750ML BOTTLE",cs:25.36}]},
  {name:"GRILL SOUR",popScore:0.8,glassware:"COUPE",garnish1:"LEMON WHEEL",garnish2:"",ingredients:[
    {name:"BOATYARD GIN",oz:2,ct:"750ML BOTTLE",cs:25.36},
    {name:"LEMON JUICE",oz:0.75,ct:"QUART BOTTLE",cs:32},
    {name:"LAVENDER HONEY",oz:0.75,ct:"QUART BOTTLE",cs:32}]},
  {name:"MANHATTAN",popScore:0.6,glassware:"MARTINI",garnish1:"CHERRY",garnish2:"",ingredients:[
    {name:"WILD TURKEY RYE",oz:2.5,ct:"LITER BOTTLE",cs:33.8},
    {name:"CARPANO ANTICA",oz:0.66,ct:"LITER BOTTLE",cs:33.8},
    {name:"CHERRY HEERING",oz:0.33,ct:"750ML BOTTLE",cs:25.36},
    {name:"ANGOSTURA",oz:0.042,ct:"OZ",cs:1},
    {name:"ELMAKULE BITTERS",oz:0.042,ct:"OZ",cs:1}]},
  {name:"ESPRESSO MARTINI",popScore:1,glassware:"COUPE",garnish1:"",garnish2:"",ingredients:[
    {name:"GREY GOOSE",oz:1.5,ct:"LITER BOTTLE",cs:33.8},
    {name:"KAHLUA",oz:1.5,ct:"1.75L BOTTLE",cs:59.15},
    {name:"DECAF ESPRESSO",oz:1.5,ct:"QUART BOTTLE",cs:32}]},
  {name:"RASPBERRY",popScore:0.4,glassware:"HIGHBALL",garnish1:"RASPBERRY",garnish2:"",ingredients:[
    {name:"PUEBLO VIEJO BLANCO",oz:2,ct:"LITER BOTTLE",cs:33.8},
    {name:"LIME JUICE",oz:0.75,ct:"QUART BOTTLE",cs:32},
    {name:"GRAPEFRUIT JUICE",oz:0.5,ct:"QUART BOTTLE",cs:32},
    {name:"CANE SYRUP",oz:0.5,ct:"QUART BOTTLE",cs:32},
    {name:"RASPBERRY PUREE",oz:0.75,ct:"LITER BOTTLE",cs:33.8}]},
  {name:"SKINNY MARG",popScore:1,glassware:"ROCKS",garnish1:"LIME WHEEL",garnish2:"",ingredients:[
    {name:"PUEBLO VIEJO BLANCO",oz:2,ct:"LITER BOTTLE",cs:33.8},
    {name:"LIME JUICE",oz:0.75,ct:"QUART BOTTLE",cs:32},
    {name:"BOLS TRIPLE SEC",oz:0.75,ct:"LITER BOTTLE",cs:33.8}]},
  {name:"COSMOPOLITAN",popScore:0.8,glassware:"COUPE",garnish1:"LIME WHEEL",garnish2:"",ingredients:[
    {name:"GREY GOOSE",oz:2,ct:"LITER BOTTLE",cs:33.8},
    {name:"BOLS TRIPLE SEC",oz:0.75,ct:"LITER BOTTLE",cs:33.8},
    {name:"CRANBERRY JUICE",oz:0.5,ct:"OZ",cs:1},
    {name:"LIME JUICE",oz:0.5,ct:"QUART BOTTLE",cs:32}]},
  {name:"COASTAL SPRITZ",popScore:0.3,glassware:"HIGHBALL",garnish1:"HALF ORANGE WHEEL",garnish2:"",ingredients:[
    {name:"PENTIRE APERITIF",oz:1.5,ct:"700ML BOTTLE",cs:23.67},
    {name:"GRENADINE",oz:0.25,ct:"QUART BOTTLE",cs:32},
    {name:"GRAPEFRUIT JUICE",oz:1,ct:"QUART BOTTLE",cs:32},
    {name:"LEMON JUICE",oz:1,ct:"QUART BOTTLE",cs:32},
    {name:"GRAPEFRUIT SODA",oz:1.5,ct:"8OZ BOTTLE",cs:8}]},
  {name:"CUCUMBER LEMONADE",popScore:0.7,glassware:"ROCKS",garnish1:"CUCUMBER SLICE",garnish2:"",ingredients:[
    {name:"ASSAM TEA",oz:3,ct:"QUART BOTTLE",cs:32},
    {name:"LEMON JUICE",oz:1,ct:"QUART BOTTLE",cs:32},
    {name:"CANE SYRUP",oz:1,ct:"QUART BOTTLE",cs:32},
    {name:"CUCUMBER JUICE",oz:0.125,ct:"QUART BOTTLE",cs:32}]},
  {name:"GRAPEFRUIT SODA NA",popScore:0.5,glassware:"HIGHBALL",garnish1:"HALF GRAPEFRUIT WHEEL",garnish2:"",ingredients:[
    {name:"GRAPEFRUIT CORDIAL",oz:1.5,ct:"QUART BOTTLE",cs:32},
    {name:"LEMON JUICE",oz:0.5,ct:"QUART BOTTLE",cs:32},
    {name:"CLUB SODA",oz:5,ct:"8OZ BOTTLE",cs:8}]},
  {name:"PUNCH NA",popScore:0.5,glassware:"COUPE",garnish1:"LIME WHEEL",garnish2:"",ingredients:[
    {name:"ORANGE JUICE",oz:2,ct:"QUART BOTTLE",cs:32},
    {name:"LIME JUICE",oz:0.5,ct:"QUART BOTTLE",cs:32},
    {name:"PASSIONFRUIT PUREE",oz:0.5,ct:"KG CONTAINER",cs:35.27},
    {name:"ROSES GRENADINE",oz:0.75,ct:"25OZ BOTTLE",cs:25},
    {name:"CLUB SODA",oz:1,ct:"8OZ BOTTLE",cs:8}]},
  {name:"CLASSIC OLD FASHIONED",popScore:0.6,glassware:"ROCKS",garnish1:"ORANGE TWIST",garnish2:"",ingredients:[
    {name:"MAKERS MARK BOURBON",oz:2.5,ct:"LITER BOTTLE",cs:33.8},
    {name:"CANE SYRUP",oz:0.25,ct:"QUART BOTTLE",cs:32},
    {name:"ANGOSTURA",oz:0.0625,ct:"OZ",cs:1}]},
  {name:"CRANBERRY MOJITO NA",popScore:0.6,glassware:"HIGHBALL",garnish1:"MINT CROWN",garnish2:"",ingredients:[
    {name:"LIME JUICE",oz:1,ct:"QUART BOTTLE",cs:32},
    {name:"CRANBERRY JUICE",oz:1,ct:"OZ",cs:1},
    {name:"MINT SYRUP",oz:0.75,ct:"QUART BOTTLE",cs:32}]},
  {name:"BLUE HAWAII",popScore:0.7,glassware:"MARTINI",garnish1:"PINEAPPLE SLICE",garnish2:"",ingredients:[
    {name:"GREY GOOSE",oz:0.75,ct:"LITER BOTTLE",cs:33.8},
    {name:"BACARDI SUPERIOR",oz:0.75,ct:"LITER BOTTLE",cs:33.8},
    {name:"BLUE CURACAO",oz:0.5,ct:"750ML BOTTLE",cs:25.36},
    {name:"PINEAPPLE JUICE",oz:3,ct:"OZ",cs:1},
    {name:"LIME JUICE",oz:0.75,ct:"QUART BOTTLE",cs:32},
    {name:"CANE SYRUP",oz:0.5,ct:"QUART BOTTLE",cs:32}]},
  {name:"CLASSIC NEGRONI",popScore:0.8,glassware:"ROCKS",garnish1:"ORANGE TWIST",garnish2:"",ingredients:[
    {name:"BOATYARD GIN",oz:1.5,ct:"750ML BOTTLE",cs:25.36},
    {name:"CAMPARI",oz:1,ct:"LITER BOTTLE",cs:33.8},
    {name:"COCCHI VDT",oz:1,ct:"750ML BOTTLE",cs:25.36}]},
  {name:"CUCUMBER SHISO GIN",popScore:0.8,glassware:"ROCKS",garnish1:"LIME WHEEL",garnish2:"",ingredients:[
    {name:"BOATYARD GIN",oz:2,ct:"750ML BOTTLE",cs:25.36},
    {name:"ST GERMAIN",oz:0.5,ct:"750ML BOTTLE",cs:25.36},
    {name:"LIME JUICE",oz:0.75,ct:"QUART BOTTLE",cs:32},
    {name:"CANE SYRUP",oz:0.5,ct:"QUART BOTTLE",cs:32},
    {name:"CUCUMBER JUICE",oz:0.25,ct:"QUART BOTTLE",cs:32}]},
  {name:"LYCHEE MARTINI VODKA",popScore:0.8,glassware:"MARTINI",garnish1:"LYCHEE WHOLE",garnish2:"",ingredients:[
    {name:"KETEL ONE",oz:2.5,ct:"LITER BOTTLE",cs:33.8},
    {name:"GIFFARD LICHI LI",oz:0.5,ct:"750ML BOTTLE",cs:25.36},
    {name:"ST GERMAIN",oz:0.25,ct:"750ML BOTTLE",cs:25.36},
    {name:"LEMON JUICE",oz:0.5,ct:"QUART BOTTLE",cs:32},
    {name:"LYCHEE PUREE",oz:1.5,ct:"KG CONTAINER",cs:35.27},
    {name:"WHOLE MILK",oz:2,ct:"QUART BOTTLE",cs:32}]},
  {name:"SPICY MATCHA MARGARITA",popScore:0.8,glassware:"ROCKS",garnish1:"JALAPENO SLICE",garnish2:"",ingredients:[
    {name:"PUEBLO VIEJO BLANCO",oz:2,ct:"LITER BOTTLE",cs:33.8},
    {name:"NARANJA ORANGE LIQUEUR",oz:0.66,ct:"LITER BOTTLE",cs:33.8},
    {name:"CANE SYRUP",oz:0.33,ct:"QUART BOTTLE",cs:32},
    {name:"LIME JUICE",oz:0.75,ct:"QUART BOTTLE",cs:32}]},
const BAR_TYPES      = ["Grill Bar","Pool Bar","Sat 6ft","Sat 12ft","Sat 18ft","Passed Station"];
const SERVICE_OPTS   = ["PASSED","FEATURED","ON REQUEST","STATIONARY"];
const DRINKS_PER_GUEST = 2.2;

// ══ BUILD INPUT ROWS (called after MASTER loads) ═══════════════════════
function buildRows() {
  const cocktailNames = MASTER.map(r => r.name).sort();
  const ct = document.getElementById('cocktailRows');
  ct.innerHTML = '';
  for (let i = 0; i < 8; i++) {
    const tr = document.createElement('tr');
    tr.innerHTML = `
      <td class="rnum">${i+1}</td>
      <td><select id="c${i}">
        <option value="">— Select Cocktail —</option>
        ${cocktailNames.map(n => `<option>${n}</option>`).join('')}
      </select></td>
      <td><select id="s${i}">
        ${SERVICE_OPTS.map((s,si) => `<option${si===0?' selected':''}>${s}</option>`).join('')}
      </select></td>`;
    ct.appendChild(tr);
  }

  const bt = document.getElementById('barRows');
  bt.innerHTML = '';
  for (let j = 0; j < 6; j++) {
    const tr = document.createElement('tr');
    tr.innerHTML = `
      <td class="rnum">${j+1}</td>
      <td><select id="b${j}">
        <option value="">— Select Bar Type —</option>
        ${BAR_TYPES.map(b => `<option>${b}</option>`).join('')}
      </select></td>
      <td><input type="number" id="bq${j}" min="1" placeholder="—" style="text-align:center"></td>`;
    bt.appendChild(tr);
  }
}

// ══ GENERATE ══════════════════════════════════════════════════
function generateAndSwitch() {
  const name     = document.getElementById('i_name').value    || 'Untitled Event';
  const date     = document.getElementById('i_date').value    || '—';
  const time     = document.getElementById('i_time').value    || '—';
  const guests   = parseInt(document.getElementById('i_guests').value) || 0;
  const location = document.getElementById('i_location').value || '—';
  const type     = document.getElementById('i_type').value    || '—';
  const tequila  = document.getElementById('i_tequila').value;

  if (!guests) { alert('Please enter a guest count.'); return; }

  const selected = [];
  for (let i = 0; i < 8; i++) {
    const cname = document.getElementById(`c${i}`).value;
    const svc   = document.getElementById(`s${i}`).value;
    if (cname) selected.push({ name: cname, service: svc, recipe: MASTER.find(r => r.name === cname) });
  }
  if (!selected.length) { alert('Please select at least one cocktail.'); return; }

  const bars = [];
  for (let j = 0; j < 6; j++) {
    const btype = document.getElementById(`b${j}`).value;
    const bqty  = parseInt(document.getElementById(`bq${j}`).value) || 1;
    if (btype) bars.push({ type: btype, qty: bqty });
  }

  const totalDrinks = Math.round(guests * DRINKS_PER_GUEST);
  const totalPop    = selected.reduce((s,c) => s + (c.recipe ? c.recipe.popScore : 0.5), 0);
  selected.forEach(c => {
    const w = totalPop > 0 ? (c.recipe ? c.recipe.popScore : 0.5) / totalPop : 1 / selected.length;
    c.servings = Math.round(totalDrinks * w);
  });

  const beoData = { name, date, time, guests, location, type, tequila, selected, bars, totalDrinks };
  window._lastBEO = beoData;
  renderOutput(beoData);
  switchPane('output');
}

// ══ RENDER OUTPUT ═════════════════════════════════════════════
function renderOutput(d) {
  const now = new Date().toLocaleString('en-US',{month:'short',day:'numeric',year:'numeric',hour:'numeric',minute:'2-digit'});
  document.getElementById('out-meta').textContent = `Generated ${now}`;

  let html = `<div class="out-doc">
    <div class="out-doc-header">
      <div class="left">
        <div class="brand-sm">375 Bar Group · Batch Recipe Sheet</div>
        <h2>${d.name}</h2>
      </div>
      <div class="right">
        <strong>${d.date}</strong><br>
        ${d.time}<br>
        ${d.location}
      </div>
    </div>
    <div class="out-info-bar">
      <div class="out-info-cell"><div class="lbl">Guests</div><div class="val">${d.guests}</div></div>
      <div class="out-info-cell"><div class="lbl">Event Type</div><div class="val">${d.type}</div></div>
      <div class="out-info-cell"><div class="lbl">Tequila Upgrade</div><div class="val">${d.tequila}</div></div>
      <div class="out-info-cell gold-cell"><div class="lbl">Total Drinks</div><div class="val">${d.totalDrinks}</div></div>
    </div>
    <div class="out-body">`;

  if (d.bars.length) {
    html += `<div class="out-section-hdr">Bar Setup</div><div class="bar-chips">`;
    d.bars.forEach(b => {
      html += `<div class="bar-chip">${b.type}<span class="qty">${b.qty}</span></div>`;
    });
    html += `</div>`;
  }

  html += `<div class="out-section-hdr">Batch Recipes <span>${d.selected.length} cocktail${d.selected.length>1?'s':''} · ${d.totalDrinks} total drinks @ ${DRINKS_PER_GUEST} per guest</span></div>`;

  d.selected.forEach(c => {
    const r = c.recipe;
    html += `<div class="cocktail-card">
      <div class="cc-head">
        <span class="cname">${c.name}</span>
        <span class="cmeta">
          <span class="tag">${c.service}</span>
          <span>${c.servings} servings</span>
          ${r ? `<span style="color:var(--text3)">${r.glassware}</span>` : ''}
        </span>
      </div>`;

    if (!r) {
      html += `<div style="padding:12px 20px;font-size:12px;color:var(--red);">⚠ Recipe not found in master list</div></div>`;
      return;
    }

    html += `<table class="ing-tbl"><thead><tr>
      <th>Ingredient</th>
      <th class="r">Per Serving</th>
      <th class="r">Batch Total</th>
      <th class="r">Container</th>
      <th class="r">Bottles</th>
    </tr></thead><tbody>`;

    r.ingredients.forEach(ing => {
      const batchOz  = ing.oz * c.servings;
      const bottlesR = Math.round(batchOz / ing.cs);
      const batchStr = (Math.round(batchOz * 10) / 10) + ' oz';
      html += `<tr>
        <td class="ing-name">${ing.name}</td>
        <td class="r">${ing.oz} oz</td>
        <td class="r">${batchStr}</td>
        <td class="r" style="font-size:11px;color:var(--text3)">${ing.ct}</td>
        <td class="r"><span class="bottles-badge">${bottlesR}</span></td>
      </tr>`;
    });

    html += `</tbody></table>`;

    const garnishes = [r.garnish1, r.garnish2].filter(g => g && g !== '/' && g !== '');
    if (garnishes.length) {
      html += `<div class="garnish-row"><span class="g-lbl">Garnish</span>`;
      garnishes.forEach(g => {
        html += `<div class="garnish-item">${g}<span class="g-count">${c.servings} pcs</span></div>`;
      });
      html += `</div>`;
    }
    html += `</div>`;
  });

  html += `<div class="out-footer">
    <span>Generated ${now}</span>
    <span>375 Bar Group · Confidential</span>
  </div></div></div>`;

  document.getElementById('outputDoc').innerHTML = html;
}

// ══ PDF DOWNLOAD ══════════════════════════════════════════════
function downloadPDF() {
  const { jsPDF } = window.jspdf;
  const doc = new jsPDF({ orientation: 'portrait', unit: 'mm', format: 'letter' });

  // Pull current output data from last generate call
  if (!window._lastBEO) { alert('Please generate the output first.'); return; }
  const d = window._lastBEO;

  const PW = 215.9; // letter width mm
  const PH = 279.4; // letter height mm
  const ML = 16;    // margin left
  const MR = 16;    // margin right
  const CW = PW - ML - MR; // content width
  let y = 0;

  // Colors
  const DARK    = [14, 14, 12];
  const GOLD    = [200, 169, 110];
  const TEXT    = [240, 236, 228];
  const TEXT2   = [160, 152, 128];
  const TEXT3   = [90, 86, 72];
  const SURFACE = [22, 22, 20];
  const SURF2   = [30, 30, 27];
  const SURF3   = [37, 37, 33];
  const GOLDBG  = [28, 24, 16];
  const WHITE   = [255, 255, 255];

  function addPage() {
    doc.addPage();
    y = 16;
  }

  function checkY(needed) {
    if (y + needed > PH - 16) addPage();
  }

  function rect(x, ry, w, h, color) {
    doc.setFillColor(...color);
    doc.rect(x, ry, w, h, 'F');
  }

  function text(str, x, ty, size, color, align, style) {
    doc.setFontSize(size || 10);
    doc.setTextColor(...(color || [240,236,228]));
    doc.setFont('helvetica', style || 'normal');
    doc.text(String(str), x, ty, { align: align || 'left', baseline: 'middle' });
  }

  // ── HEADER ─────────────────────────────────────────────────
  rect(0, 0, PW, 28, DARK);
  text('375 BAR GROUP', ML, 9, 7.5, GOLD, 'left', 'bold');
  text('BATCH RECIPE SHEET', ML, 14, 7.5, TEXT3, 'left', 'normal');
  text(d.name.toUpperCase(), ML, 22, 13, TEXT, 'left', 'bold');
  text(d.date + '  ·  ' + d.time + '  ·  ' + d.location, PW - MR, 22, 8, TEXT2, 'right', 'normal');
  y = 30;

  // ── INFO BAR ───────────────────────────────────────────────
  const infoW = CW / 4;
  const infos = [
    ['GUESTS', d.guests],
    ['EVENT TYPE', d.type],
    ['TEQUILA UPGRADE', d.tequila],
    ['TOTAL DRINKS', d.totalDrinks]
  ];
  infos.forEach((inf, i) => {
    const x = ML + i * infoW;
    const isLast = i === 3;
    rect(x, y, infoW, 14, isLast ? GOLDBG : SURF2);
    text(inf[0], x + 4, y + 4.5, 6, TEXT3, 'left', 'bold');
    text(String(inf[1]), x + 4, y + 10.5, 9, isLast ? GOLD : TEXT, 'left', 'bold');
  });
  y += 16;

  // ── BAR SETUP ──────────────────────────────────────────────
  if (d.bars.length) {
    checkY(20);
    rect(ML, y, CW, 7, SURF3);
    text('BAR SETUP', ML + 3, y + 3.5, 7, GOLD, 'left', 'bold');
    y += 8;
    let bx = ML;
    d.bars.forEach(b => {
      rect(bx, y, 40, 8, SURF2);
      text(b.type, bx + 3, y + 4, 8, TEXT, 'left', 'normal');
      rect(bx + 32, y + 1, 6, 6, [200,169,110]);
      text(String(b.qty), bx + 35, y + 4, 7, [14,14,12], 'center', 'bold');
      bx += 43;
      if (bx > PW - MR - 40) { bx = ML; y += 10; }
    });
    y += 12;
  }

  // ── COCKTAIL CARDS ─────────────────────────────────────────
  rect(ML, y, CW, 7, SURF3);
  text('BATCH RECIPES', ML + 3, y + 3.5, 7, GOLD, 'left', 'bold');
  text(d.selected.length + ' cocktails  ·  ' + d.totalDrinks + ' total drinks  ·  ' + DRINKS_PER_GUEST + ' per guest', PW - MR - 3, y + 3.5, 6, TEXT3, 'right', 'normal');
  y += 9;

  d.selected.forEach(c => {
    const r = c.recipe;
    const ingCount = r ? r.ingredients.length : 0;
    const garnishes = r ? [r.garnish1, r.garnish2].filter(g => g && g !== '/' && g !== '') : [];
    const cardH = 9 + 6 + (ingCount * 7) + (garnishes.length ? 8 : 0) + 4;
    checkY(cardH);

    // Card header
    rect(ML, y, CW, 9, SURF3);
    text(c.name, ML + 3, y + 4.5, 9, TEXT, 'left', 'bold');
    // Service tag
    rect(PW - MR - 48, y + 1.5, 22, 6, GOLDBG);
    text(c.service, PW - MR - 37, y + 4.5, 6.5, GOLD, 'center', 'bold');
    text(c.servings + ' servings', PW - MR - 24, y + 4.5, 7, TEXT2, 'left', 'normal');
    y += 9;

    if (!r) {
      rect(ML, y, CW, 8, SURF2);
      text('⚠ Recipe not found', ML + 3, y + 4, 8, [204,68,68], 'left', 'normal');
      y += 10; return;
    }

    // Column headers
    rect(ML, y, CW, 6, SURF2);
    text('INGREDIENT',    ML + 3,        y + 3, 6, TEXT3, 'left',  'bold');
    text('PER SERVING',   ML + 90,       y + 3, 6, TEXT3, 'right', 'bold');
    text('BATCH TOTAL',   ML + 118,      y + 3, 6, TEXT3, 'right', 'bold');
    text('CONTAINER',     ML + 153,      y + 3, 6, TEXT3, 'right', 'bold');
    text('BOTTLES',       ML + CW - 1,   y + 3, 6, TEXT3, 'right', 'bold');
    y += 6;

    // Ingredients
    r.ingredients.forEach((ing, idx) => {
      const batchOz  = ing.oz * c.servings;
      const bottlesR = Math.round(batchOz / ing.cs);
      const batchStr = (Math.round(batchOz * 10) / 10) + ' oz';
      rect(ML, y, CW, 7, idx % 2 === 0 ? SURF2 : [26,26,23]);
      text(ing.name,      ML + 3,        y + 3.5, 8, TEXT,  'left',  'normal');
      text(ing.oz + ' oz',ML + 90,       y + 3.5, 8, TEXT2, 'right', 'normal');
      text(batchStr,      ML + 118,      y + 3.5, 8, TEXT,  'right', 'normal');
      text(ing.ct,        ML + 153,      y + 3.5, 7, TEXT3, 'right', 'normal');
      // Bottle badge
      rect(ML + CW - 14, y + 1, 13, 5, GOLD);
      text(String(bottlesR), ML + CW - 7.5, y + 3.5, 7, DARK, 'center', 'bold');
      y += 7;
    });

    // Garnishes
    if (garnishes.length) {
      rect(ML, y, CW, 8, GOLDBG);
      text('GARNISH', ML + 3, y + 4, 6.5, GOLD, 'left', 'bold');
      let gx = ML + 22;
      garnishes.forEach(g => {
        text(g, gx, y + 4, 8, TEXT2, 'left', 'normal');
        // Count pill
        const tw = doc.getTextWidth(g) + 4;
        rect(gx + tw + 2, y + 1.5, 18, 5, [40,32,18]);
        text(c.servings + ' pcs', gx + tw + 11, y + 4, 6.5, GOLD, 'center', 'normal');
        gx += tw + 24;
      });
      y += 8;
    }
    y += 4;
  });

  // ── FOOTER ─────────────────────────────────────────────────
  checkY(10);
  const now = new Date().toLocaleString('en-US',{month:'short',day:'numeric',year:'numeric',hour:'numeric',minute:'2-digit'});
  doc.setDrawColor(...TEXT3);
  doc.setLineWidth(0.3);
  doc.line(ML, y, PW - MR, y);
  y += 5;
  text('Generated ' + now, ML, y, 7, TEXT3, 'left', 'normal');
  text('375 Bar Group · Confidential', PW - MR, y, 7, TEXT3, 'right', 'normal');

  // Save
  const fileName = (d.name || 'BEO').replace(/[^a-zA-Z0-9 ]/g, '').trim().replace(/ /g,'_');
  doc.save('375Bar_BEO_' + fileName + '.pdf');
}

// ══ CLEAR FORM ════════════════════════════════════════════════
function clearForm() {
  ['i_name','i_date','i_time','i_location'].forEach(id => document.getElementById(id).value = '');
  document.getElementById('i_guests').value = '';
  document.getElementById('i_type').value = '';
  document.getElementById('i_tequila').value = 'No';
  buildRows();
  switchPane('input');
}

// ══ NAVIGATION ════════════════════════════════════════════════
function switchPane(name) {
  document.querySelectorAll('.pane').forEach(p => p.classList.remove('active'));
  document.querySelectorAll('.nav-tab').forEach(t => t.classList.remove('active'));
  document.getElementById(`pane-${name}`).classList.add('active');
  document.getElementById(`tab-${name}`).classList.add('active');
}
</script>
</body>
</html>
