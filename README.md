[index.html](https://github.com/user-attachments/files/26481666/index.html)
<!DOCTYPE html>
<html lang="it">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>CFD Regime Profiler — Lab Dashboard</title>
<link href="https://fonts.googleapis.com/css2?family=JetBrains+Mono:wght@300;400;500;700&family=Syne:wght@400;600;800&display=swap" rel="stylesheet">
<style>
  :root {
    --bg: #0a0c0f;
    --bg2: #111318;
    --bg3: #181c23;
    --border: #1e2530;
    --border2: #2a3340;
    --text: #c8d4e0;
    --text2: #6a7d90;
    --text3: #3a4a58;
    --accent: #00d4aa;
    --accent2: #0099cc;
    --warn: #f0a020;
    --danger: #e04050;
    --green: #30d080;
    --purple: #9060f0;
    --mono: 'JetBrains Mono', monospace;
    --sans: 'Syne', sans-serif;
  }

  * { margin: 0; padding: 0; box-sizing: border-box; }

  body {
    background: var(--bg);
    color: var(--text);
    font-family: var(--mono);
    font-size: 13px;
    min-height: 100vh;
    overflow-x: hidden;
  }

  /* GRID NOISE BACKGROUND */
  body::before {
    content: '';
    position: fixed;
    inset: 0;
    background-image:
      linear-gradient(var(--border) 1px, transparent 1px),
      linear-gradient(90deg, var(--border) 1px, transparent 1px);
    background-size: 40px 40px;
    opacity: 0.3;
    pointer-events: none;
    z-index: 0;
  }

  .app { position: relative; z-index: 1; }

  /* HEADER */
  header {
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 20px 32px;
    border-bottom: 1px solid var(--border);
    background: rgba(10,12,15,0.95);
    position: sticky;
    top: 0;
    z-index: 100;
    backdrop-filter: blur(8px);
  }

  .logo {
    font-family: var(--sans);
    font-weight: 800;
    font-size: 16px;
    letter-spacing: -0.02em;
    color: #fff;
  }

  .logo span { color: var(--accent); }

  .header-meta {
    display: flex;
    align-items: center;
    gap: 20px;
  }

  .status-dot {
    width: 6px; height: 6px;
    border-radius: 50%;
    background: var(--green);
    box-shadow: 0 0 8px var(--green);
    animation: pulse 2s infinite;
  }

  @keyframes pulse {
    0%,100% { opacity: 1; }
    50% { opacity: 0.4; }
  }

  .clock { color: var(--text2); font-size: 11px; }

  /* MAIN LAYOUT */
  .main {
    display: grid;
    grid-template-columns: 1fr 1fr 1fr;
    grid-template-rows: auto auto auto;
    gap: 1px;
    background: var(--border);
    min-height: calc(100vh - 61px);
  }

  /* SECTIONS */
  .section {
    background: var(--bg);
    padding: 0;
    display: flex;
    flex-direction: column;
  }

  .section.full-width {
    grid-column: 1 / -1;
  }

  .section.two-col {
    grid-column: span 2;
  }

  .section-header {
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 14px 20px;
    border-bottom: 1px solid var(--border);
    background: var(--bg2);
  }

  .section-title {
    font-family: var(--sans);
    font-size: 11px;
    font-weight: 600;
    letter-spacing: 0.12em;
    text-transform: uppercase;
    color: var(--text2);
    display: flex;
    align-items: center;
    gap: 8px;
  }

  .section-title::before {
    content: '';
    width: 3px; height: 12px;
    background: var(--accent);
    border-radius: 2px;
  }

  .section-body {
    padding: 16px 20px;
    flex: 1;
    overflow-y: auto;
    max-height: 320px;
  }

  .section-body::-webkit-scrollbar { width: 3px; }
  .section-body::-webkit-scrollbar-track { background: transparent; }
  .section-body::-webkit-scrollbar-thumb { background: var(--border2); border-radius: 2px; }

  /* ADD BUTTON */
  .btn-add {
    background: none;
    border: 1px solid var(--border2);
    color: var(--text2);
    font-family: var(--mono);
    font-size: 11px;
    padding: 4px 10px;
    border-radius: 3px;
    cursor: pointer;
    transition: all 0.15s;
  }
  .btn-add:hover { border-color: var(--accent); color: var(--accent); }

  /* CARDS */
  .card {
    background: var(--bg2);
    border: 1px solid var(--border);
    border-radius: 4px;
    padding: 12px 14px;
    margin-bottom: 8px;
    position: relative;
    transition: border-color 0.15s;
  }
  .card:hover { border-color: var(--border2); }
  .card:last-child { margin-bottom: 0; }

  .card-top {
    display: flex;
    align-items: center;
    justify-content: space-between;
    margin-bottom: 8px;
  }

  .card-symbol {
    font-family: var(--sans);
    font-weight: 600;
    font-size: 13px;
    color: #fff;
  }

  .badge {
    font-size: 9px;
    font-weight: 500;
    padding: 2px 7px;
    border-radius: 2px;
    text-transform: uppercase;
    letter-spacing: 0.08em;
  }

  .badge-progress { background: rgba(0,212,170,0.12); color: var(--accent); border: 1px solid rgba(0,212,170,0.25); }
  .badge-demo { background: rgba(144,96,240,0.12); color: var(--purple); border: 1px solid rgba(144,96,240,0.25); }
  .badge-live { background: rgba(48,208,128,0.12); color: var(--green); border: 1px solid rgba(48,208,128,0.25); }
  .badge-fail { background: rgba(224,64,80,0.12); color: var(--danger); border: 1px solid rgba(224,64,80,0.25); }
  .badge-review { background: rgba(240,160,32,0.12); color: var(--warn); border: 1px solid rgba(240,160,32,0.25); }
  .badge-pass { background: rgba(48,208,128,0.12); color: var(--green); border: 1px solid rgba(48,208,128,0.25); }

  .card-meta {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 8px;
  }

  .meta-item { }
  .meta-label { font-size: 9px; color: var(--text3); text-transform: uppercase; letter-spacing: 0.08em; margin-bottom: 2px; }
  .meta-value { font-size: 12px; color: var(--text); }
  .meta-value.good { color: var(--green); }
  .meta-value.bad { color: var(--danger); }
  .meta-value.warn { color: var(--warn); }

  .card-note {
    margin-top: 8px;
    padding-top: 8px;
    border-top: 1px solid var(--border);
    font-size: 11px;
    color: var(--text2);
    line-height: 1.5;
  }

  .card-actions {
    position: absolute;
    top: 10px;
    right: 10px;
    display: none;
    gap: 4px;
  }
  .card:hover .card-actions { display: flex; }

  .btn-icon {
    background: var(--bg3);
    border: 1px solid var(--border2);
    color: var(--text2);
    width: 22px; height: 22px;
    border-radius: 3px;
    cursor: pointer;
    font-size: 11px;
    display: flex; align-items: center; justify-content: center;
    transition: all 0.15s;
  }
  .btn-icon:hover { color: var(--danger); border-color: var(--danger); }

  /* HURST MATRIX */
  .hurst-table {
    width: 100%;
    border-collapse: collapse;
  }
  .hurst-table th {
    font-size: 9px;
    text-transform: uppercase;
    letter-spacing: 0.1em;
    color: var(--text3);
    padding: 6px 10px;
    text-align: center;
    border-bottom: 1px solid var(--border);
  }
  .hurst-table td {
    padding: 8px 10px;
    text-align: center;
    border-bottom: 1px solid var(--border);
    font-size: 12px;
  }
  .hurst-table tr:last-child td { border-bottom: none; }
  .hurst-table td:first-child { text-align: left; color: #fff; font-weight: 500; }

  .hurst-cell {
    border-radius: 3px;
    padding: 3px 8px;
    font-size: 11px;
    font-weight: 500;
    display: inline-block;
    min-width: 44px;
  }
  .h-trend { background: rgba(0,212,170,0.12); color: var(--accent); }
  .h-mean  { background: rgba(224,64,80,0.12); color: var(--danger); }
  .h-neutral { background: rgba(240,160,32,0.12); color: var(--warn); }
  .h-empty { color: var(--text3); }

  /* PIPELINE */
  .pipeline-item {
    display: flex;
    align-items: center;
    gap: 12px;
    padding: 10px 0;
    border-bottom: 1px solid var(--border);
  }
  .pipeline-item:last-child { border-bottom: none; }
  .pipeline-rank { color: var(--text3); font-size: 11px; min-width: 20px; }
  .pipeline-info { flex: 1; }
  .pipeline-name { color: var(--text); font-size: 12px; margin-bottom: 2px; }
  .pipeline-sub { color: var(--text2); font-size: 10px; }

  /* AI AGENT */
  .agent-section {
    grid-column: 1 / -1;
    background: var(--bg);
    border-top: 1px solid var(--border);
  }

  .agent-header {
    padding: 14px 20px;
    background: var(--bg2);
    border-bottom: 1px solid var(--border);
    display: flex;
    align-items: center;
    gap: 10px;
  }

  .agent-label {
    font-family: var(--sans);
    font-size: 11px;
    font-weight: 600;
    letter-spacing: 0.12em;
    text-transform: uppercase;
    color: var(--accent);
  }

  .agent-body {
    padding: 16px 20px;
    display: flex;
    gap: 12px;
    align-items: flex-start;
  }

  .agent-input {
    flex: 1;
    background: var(--bg2);
    border: 1px solid var(--border2);
    border-radius: 4px;
    color: var(--text);
    font-family: var(--mono);
    font-size: 13px;
    padding: 10px 14px;
    resize: none;
    height: 60px;
    transition: border-color 0.15s;
    outline: none;
  }
  .agent-input:focus { border-color: var(--accent); }
  .agent-input::placeholder { color: var(--text3); }

  .agent-send {
    background: var(--accent);
    border: none;
    color: #000;
    font-family: var(--sans);
    font-weight: 600;
    font-size: 12px;
    padding: 10px 20px;
    border-radius: 4px;
    cursor: pointer;
    height: 60px;
    white-space: nowrap;
    transition: opacity 0.15s;
  }
  .agent-send:hover { opacity: 0.85; }
  .agent-send:disabled { opacity: 0.4; cursor: not-allowed; }

  .agent-log {
    padding: 0 20px 16px;
    max-height: 120px;
    overflow-y: auto;
  }
  .log-entry {
    font-size: 11px;
    padding: 4px 0;
    border-bottom: 1px solid var(--border);
    display: flex;
    gap: 10px;
  }
  .log-time { color: var(--text3); min-width: 50px; }
  .log-msg { color: var(--text2); }
  .log-msg.ok { color: var(--green); }
  .log-msg.err { color: var(--danger); }

  .insight-card {
    background: var(--bg2);
    border: 1px solid var(--border);
    border-left: 3px solid var(--accent);
    border-radius: 4px;
    padding: 12px 16px;
    margin-bottom: 8px;
    display: flex;
    gap: 14px;
    align-items: flex-start;
  }
  .insight-card.warn { border-left-color: var(--warn); }
  .insight-card.danger { border-left-color: var(--danger); }
  .insight-card.purple { border-left-color: var(--purple); }
  .insight-card.green { border-left-color: var(--green); }

  .insight-icon { font-size: 16px; min-width: 20px; margin-top: 1px; }
  .insight-body { flex: 1; }
  .insight-title { color: #fff; font-size: 12px; font-weight: 500; margin-bottom: 4px; }
  .insight-text { color: var(--text2); font-size: 11px; line-height: 1.6; }
  .insight-tag {
    display: inline-block;
    font-size: 9px;
    padding: 1px 6px;
    border-radius: 2px;
    margin-top: 6px;
    text-transform: uppercase;
    letter-spacing: 0.08em;
  }
  .tag-opportunity { background: rgba(0,212,170,0.1); color: var(--accent); }
  .tag-warning { background: rgba(240,160,32,0.1); color: var(--warn); }
  .tag-action { background: rgba(144,96,240,0.1); color: var(--purple); }
  .tag-stop { background: rgba(224,64,80,0.1); color: var(--danger); }
  .tag-info { background: rgba(100,120,140,0.1); color: var(--text2); }

  .insights-loading {
    display: flex; align-items: center; gap: 10px;
    color: var(--text2); font-size: 11px; padding: 16px 0;
  }
  .spinner {
    width: 14px; height: 14px;
    border: 2px solid var(--border2);
    border-top-color: var(--accent);
    border-radius: 50%;
    animation: spin 0.8s linear infinite;
  }
  @keyframes spin { to { transform: rotate(360deg); } }

  .insights-meta {
    font-size: 10px; color: var(--text3);
    padding: 8px 0 0;
    border-top: 1px solid var(--border);
    margin-top: 4px;
  }


  .modal-overlay {
    display: none;
    position: fixed;
    inset: 0;
    background: rgba(0,0,0,0.75);
    z-index: 999;
    align-items: center;
    justify-content: center;
    backdrop-filter: blur(4px);
  }
  .modal-overlay.open { display: flex; }

  .modal {
    background: var(--bg2);
    border: 1px solid var(--border2);
    border-radius: 6px;
    padding: 24px;
    width: 480px;
    max-width: 95vw;
  }

  .modal-title {
    font-family: var(--sans);
    font-weight: 600;
    font-size: 14px;
    color: #fff;
    margin-bottom: 20px;
  }

  .form-group { margin-bottom: 14px; }
  .form-label { font-size: 10px; text-transform: uppercase; letter-spacing: 0.08em; color: var(--text2); margin-bottom: 5px; display: block; }
  .form-input, .form-select, .form-textarea {
    width: 100%;
    background: var(--bg3);
    border: 1px solid var(--border2);
    border-radius: 3px;
    color: var(--text);
    font-family: var(--mono);
    font-size: 12px;
    padding: 8px 10px;
    outline: none;
    transition: border-color 0.15s;
  }
  .form-input:focus, .form-select:focus, .form-textarea:focus { border-color: var(--accent); }
  .form-select { appearance: none; cursor: pointer; }
  .form-textarea { resize: vertical; min-height: 60px; }

  .form-row { display: grid; grid-template-columns: 1fr 1fr; gap: 12px; }

  .modal-actions { display: flex; gap: 10px; justify-content: flex-end; margin-top: 20px; }

  .btn-cancel {
    background: none;
    border: 1px solid var(--border2);
    color: var(--text2);
    font-family: var(--mono);
    font-size: 12px;
    padding: 8px 16px;
    border-radius: 3px;
    cursor: pointer;
  }
  .btn-save {
    background: var(--accent);
    border: none;
    color: #000;
    font-family: var(--sans);
    font-weight: 600;
    font-size: 12px;
    padding: 8px 20px;
    border-radius: 3px;
    cursor: pointer;
  }

  .empty-state {
    color: var(--text3);
    font-size: 11px;
    text-align: center;
    padding: 24px 0;
    line-height: 1.8;
  }

  /* COUNTER BADGES */
  .count-badge {
    background: var(--bg3);
    border: 1px solid var(--border2);
    color: var(--text3);
    font-size: 10px;
    padding: 1px 6px;
    border-radius: 10px;
  }
</style>
</head>
<body>
<div class="app">

<header>
  <div class="logo">CFD <span>Regime</span> Profiler</div>
  <div class="header-meta">
    <div class="status-dot" id="api-dot" style="background:var(--warn);box-shadow:0 0 8px var(--warn)"></div>
    <div class="clock" id="clock">--:--:--</div>
    <button class="btn-add" onclick="toggleApiPanel()" id="api-btn" style="font-size:10px; color:var(--warn); border-color:rgba(240,160,32,0.4)">⚙ API Key</button>
  </div>
</header>

<!-- API KEY PANEL -->
<div id="api-panel" style="display:none; background:var(--bg2); border-bottom:1px solid var(--border); padding:12px 32px; flex-direction:row; align-items:center; gap:12px; position:relative; z-index:99;">
  <span style="font-size:10px; color:var(--text2); text-transform:uppercase; letter-spacing:0.08em; white-space:nowrap;">Anthropic API Key</span>
  <input id="api-key-input" type="password"
    placeholder="sk-ant-api03-..."
    style="flex:1; background:var(--bg3); border:1px solid var(--border2); border-radius:3px; color:var(--text); font-family:var(--mono); font-size:12px; padding:7px 12px; outline:none; min-width:200px;"
    oninput="saveApiKey(this.value)">
  <span style="font-size:10px; color:var(--text3); white-space:nowrap;">Salvata solo nel browser</span>
  <a href="https://console.anthropic.com/settings/keys" target="_blank"
    style="font-size:10px; color:var(--accent); text-decoration:none; white-space:nowrap;">Ottieni key →</a>
  <button class="btn-add" onclick="toggleApiPanel()" style="font-size:10px">✕ Chiudi</button>
</div>

<div class="main">

  <!-- BACKTEST IN CORSO -->
  <section class="section">
    <div class="section-header">
      <div class="section-title">Backtest in corso <span class="count-badge" id="cnt-bt">0</span></div>
      <button class="btn-add" onclick="openModal('backtest')">+ Aggiungi</button>
    </div>
    <div class="section-body" id="list-backtest">
      <div class="empty-state">Nessun backtest in corso.<br>Clicca + per aggiungere.</div>
    </div>
  </section>

  <!-- STRATEGIE IN DEMO -->
  <section class="section">
    <div class="section-header">
      <div class="section-title">Demo live <span class="count-badge" id="cnt-demo">0</span></div>
      <button class="btn-add" onclick="openModal('demo')">+ Aggiungi</button>
    </div>
    <div class="section-body" id="list-demo">
      <div class="empty-state">Nessuna strategia in demo.<br>Clicca + per aggiungere.</div>
    </div>
  </section>

  <!-- PIPELINE -->
  <section class="section">
    <div class="section-header">
      <div class="section-title">Pipeline <span class="count-badge" id="cnt-pipe">0</span></div>
      <button class="btn-add" onclick="openModal('pipeline')">+ Aggiungi</button>
    </div>
    <div class="section-body" id="list-pipeline">
      <div class="empty-state">Pipeline vuota.</div>
    </div>
  </section>

  <!-- RISULTATI BACKTEST -->
  <section class="section two-col">
    <div class="section-header">
      <div class="section-title">Risultati backtest <span class="count-badge" id="cnt-res">0</span></div>
      <button class="btn-add" onclick="openModal('risultato')">+ Aggiungi</button>
    </div>
    <div class="section-body" id="list-risultati">
      <div class="empty-state">Nessun risultato ancora.<br>Aggiungi dopo ogni backtest completato.</div>
    </div>
  </section>

  <!-- HURST MATRIX -->
  <section class="section">
    <div class="section-header">
      <div class="section-title">Regime Hurst</div>
      <button class="btn-add" onclick="openModal('hurst')">Aggiorna</button>
    </div>
    <div class="section-body" style="padding:0; max-height:320px;">
      <table class="hurst-table" id="hurst-table">
        <thead>
          <tr>
            <th style="text-align:left">Strumento</th>
            <th>M5</th><th>M15</th><th>M30</th><th>H1</th>
          </tr>
        </thead>
        <tbody id="hurst-body">
          <tr><td colspan="5" style="text-align:center; color:var(--text3); padding:20px; font-size:11px;">Nessun dato Hurst.<br>Inserisci dopo la profilazione Python.</td></tr>
        </tbody>
      </table>
    </div>
  </section>

  <!-- INSIGHTS -->
  <section class="section full-width" id="insights-section" style="border-top:1px solid var(--border)">
    <div class="section-header">
      <div class="section-title">Insights <span class="count-badge" id="cnt-insights">0</span></div>
      <button class="btn-add" id="insights-btn" onclick="generateInsights()">⟳ Analizza</button>
    </div>
    <div class="section-body" id="insights-body" style="max-height:260px;">
      <div class="empty-state">Nessun insight ancora.<br>Clicca "Analizza" dopo aver inserito dati nelle altre sezioni.</div>
    </div>
  </section>

  <!-- AI AGENT -->
  <section class="section full-width agent-section">
    <div class="agent-header">
      <div class="status-dot" style="background:var(--accent); box-shadow:0 0 8px var(--accent)"></div>
      <div class="agent-label">Agente AI — aggiorna la dashboard in linguaggio naturale</div>
    </div>
    <div class="agent-body">
      <textarea class="agent-input" id="agent-input" placeholder='Es: "Ho finito il backtest DAX TF Simple M5, profit factor 1.45, 92 trade, max DD 8%, passa" oppure "Aggiungi Gold M15 alla pipeline, da testare con Reversal BB"'></textarea>
      <button class="agent-send" id="agent-btn" onclick="sendToAgent()">Aggiorna →</button>
    </div>
    <div class="agent-log" id="agent-log"></div>
  </section>

</div>

<!-- MODALS -->
<div class="modal-overlay" id="modal-overlay" onclick="closeModalOutside(event)">
  <div class="modal" id="modal-content"></div>
</div>

<script>
// ===================== DATA STORE =====================
const KEYS = { backtest:'crp_backtest', demo:'crp_demo', pipeline:'crp_pipeline', risultati:'crp_risultati', hurst:'crp_hurst', log:'crp_log' };

function load(k) { try { return JSON.parse(localStorage.getItem(KEYS[k])) || []; } catch(e) { return []; } }
function save(k, d) { localStorage.setItem(KEYS[k], JSON.stringify(d)); }
function uid() { return Date.now().toString(36) + Math.random().toString(36).slice(2,5); }

// ===================== RENDER =====================
function render() {
  renderBacktest();
  renderDemo();
  renderPipeline();
  renderRisultati();
  renderHurst();
}

function renderBacktest() {
  const data = load('backtest');
  const el = document.getElementById('list-backtest');
  document.getElementById('cnt-bt').textContent = data.length;
  if(!data.length) { el.innerHTML = '<div class="empty-state">Nessun backtest in corso.<br>Clicca + per aggiungere.</div>'; return; }
  el.innerHTML = data.map(d => `
    <div class="card">
      <div class="card-top">
        <div class="card-symbol">${d.symbol} <span style="color:var(--text2);font-weight:400;font-size:11px">${d.tf}</span></div>
        <span class="badge badge-progress">${d.stato}</span>
      </div>
      <div class="card-meta">
        <div class="meta-item"><div class="meta-label">Strategia</div><div class="meta-value">${d.strategia}</div></div>
        <div class="meta-item"><div class="meta-label">Iniziato</div><div class="meta-value">${d.data}</div></div>
        <div class="meta-item"><div class="meta-label">Tipo</div><div class="meta-value">${d.tipo||'—'}</div></div>
      </div>
      ${d.note ? `<div class="card-note">${d.note}</div>` : ''}
      <div class="card-actions">
        <button class="btn-icon" onclick="deleteItem('backtest','${d.id}')">✕</button>
      </div>
    </div>`).join('');
}

function renderDemo() {
  const data = load('demo');
  const el = document.getElementById('list-demo');
  document.getElementById('cnt-demo').textContent = data.length;
  if(!data.length) { el.innerHTML = '<div class="empty-state">Nessuna strategia in demo.</div>'; return; }
  el.innerHTML = data.map(d => `
    <div class="card">
      <div class="card-top">
        <div class="card-symbol">${d.symbol} <span style="color:var(--text2);font-weight:400;font-size:11px">${d.tf}</span></div>
        <span class="badge badge-demo">DEMO</span>
      </div>
      <div class="card-meta">
        <div class="meta-item"><div class="meta-label">EA</div><div class="meta-value">${d.ea}</div></div>
        <div class="meta-item"><div class="meta-label">Dal</div><div class="meta-value">${d.data}</div></div>
        <div class="meta-item"><div class="meta-label">Trade</div><div class="meta-value">${d.trade||'—'}</div></div>
        <div class="meta-item"><div class="meta-label">P&L</div><div class="meta-value ${parseFloat(d.pnl)>=0?'good':'bad'}">${d.pnl||'—'}</div></div>
        <div class="meta-item"><div class="meta-label">Win%</div><div class="meta-value">${d.winrate||'—'}</div></div>
        <div class="meta-item"><div class="meta-label">Broker</div><div class="meta-value">${d.broker||'Skilling'}</div></div>
      </div>
      ${d.note ? `<div class="card-note">${d.note}</div>` : ''}
      <div class="card-actions">
        <button class="btn-icon" onclick="deleteItem('demo','${d.id}')">✕</button>
      </div>
    </div>`).join('');
}

function renderPipeline() {
  const data = load('pipeline');
  const el = document.getElementById('list-pipeline');
  document.getElementById('cnt-pipe').textContent = data.length;
  if(!data.length) { el.innerHTML = '<div class="empty-state">Pipeline vuota.</div>'; return; }
  el.innerHTML = data.map((d,i) => `
    <div class="pipeline-item">
      <div class="pipeline-rank">${String(i+1).padStart(2,'0')}</div>
      <div class="pipeline-info">
        <div class="pipeline-name">${d.symbol} ${d.tf} — ${d.strategia}</div>
        <div class="pipeline-sub">${d.motivo||''}</div>
      </div>
      <span class="badge ${d.priorita==='alta'?'badge-live':d.priorita==='media'?'badge-review':'badge-progress'}">${d.priorita||'—'}</span>
      <button class="btn-icon" onclick="deleteItem('pipeline','${d.id}')">✕</button>
    </div>`).join('');
}

function renderRisultati() {
  const data = load('risultati');
  const el = document.getElementById('list-risultati');
  document.getElementById('cnt-res').textContent = data.length;
  if(!data.length) { el.innerHTML = '<div class="empty-state">Nessun risultato ancora.</div>'; return; }
  el.innerHTML = data.map(d => {
    const pf = parseFloat(d.pf);
    const pfClass = pf >= 1.5 ? 'good' : pf >= 1.3 ? 'warn' : 'bad';
    const esito = d.esito === 'pass' ? 'badge-pass' : d.esito === 'review' ? 'badge-review' : 'badge-fail';
    return `<div class="card">
      <div class="card-top">
        <div class="card-symbol">${d.symbol} <span style="color:var(--text2);font-weight:400;font-size:11px">${d.tf} · ${d.strategia}</span></div>
        <span class="badge ${esito}">${d.esito?.toUpperCase()||'—'}</span>
      </div>
      <div class="card-meta">
        <div class="meta-item"><div class="meta-label">Profit Factor</div><div class="meta-value ${pfClass}">${d.pf||'—'}</div></div>
        <div class="meta-item"><div class="meta-label">Trade</div><div class="meta-value">${d.trade||'—'}</div></div>
        <div class="meta-item"><div class="meta-label">Max DD</div><div class="meta-value ${parseFloat(d.dd)>15?'bad':parseFloat(d.dd)>8?'warn':'good'}">${d.dd||'—'}%</div></div>
        <div class="meta-item"><div class="meta-label">Win%</div><div class="meta-value">${d.winrate||'—'}</div></div>
        <div class="meta-item"><div class="meta-label">Hurst</div><div class="meta-value">${d.hurst||'—'}</div></div>
        <div class="meta-item"><div class="meta-label">Data</div><div class="meta-value">${d.data||'—'}</div></div>
      </div>
      ${d.note ? `<div class="card-note">${d.note}</div>` : ''}
      <div class="card-actions">
        <button class="btn-icon" onclick="deleteItem('risultati','${d.id}')">✕</button>
      </div>
    </div>`;
  }).join('');
}

function renderHurst() {
  const data = load('hurst');
  const tbody = document.getElementById('hurst-body');
  if(!data.length) {
    tbody.innerHTML = '<tr><td colspan="5" style="text-align:center;color:var(--text3);padding:20px;font-size:11px;">Nessun dato Hurst.<br>Inserisci dopo la profilazione Python.</td></tr>';
    return;
  }
  const tfs = ['M5','M15','M30','H1'];
  tbody.innerHTML = data.map(row => `
    <tr>
      <td>${row.symbol}</td>
      ${tfs.map(tf => {
        const v = row[tf];
        if(!v) return `<td><span class="hurst-cell h-empty">—</span></td>`;
        const n = parseFloat(v);
        const cls = n > 0.55 ? 'h-trend' : n < 0.45 ? 'h-mean' : 'h-neutral';
        return `<td><span class="hurst-cell ${cls}">${v}</span></td>`;
      }).join('')}
    </tr>`).join('');
}

// ===================== DELETE =====================
function deleteItem(section, id) {
  const data = load(section).filter(d => d.id !== id);
  save(section, data);
  render();
  addLog(`Elemento rimosso da ${section}.`);
}

// ===================== MODAL =====================
const modalTemplates = {
  backtest: () => `
    <div class="modal-title">Backtest in corso</div>
    <div class="form-row">
      <div class="form-group"><label class="form-label">Simbolo</label><input class="form-input" id="f-symbol" placeholder="DAX, GOLD, NQ..."></div>
      <div class="form-group"><label class="form-label">Timeframe</label><select class="form-select" id="f-tf"><option>M5</option><option>M10</option><option>M15</option><option>M30</option><option>H1</option></select></div>
    </div>
    <div class="form-group"><label class="form-label">Strategia</label><input class="form-input" id="f-strategia" placeholder="UA_TF_Simple, Reversal_BB..."></div>
    <div class="form-row">
      <div class="form-group"><label class="form-label">Stato</label><input class="form-input" id="f-stato" value="In corso"></div>
      <div class="form-group"><label class="form-label">Tipo</label><select class="form-select" id="f-tipo"><option>Trend Following</option><option>Mean Reversion</option><option>BIAS</option><option>Breakout</option></select></div>
    </div>
    <div class="form-group"><label class="form-label">Data inizio</label><input class="form-input" id="f-data" type="date" value="${new Date().toISOString().slice(0,10)}"></div>
    <div class="form-group"><label class="form-label">Note</label><textarea class="form-textarea" id="f-note" placeholder="Pattern usati, parametri, osservazioni..."></textarea></div>
    <div class="modal-actions">
      <button class="btn-cancel" onclick="closeModal()">Annulla</button>
      <button class="btn-save" onclick="saveBacktest()">Salva</button>
    </div>`,

  demo: () => `
    <div class="modal-title">Strategia in demo</div>
    <div class="form-row">
      <div class="form-group"><label class="form-label">Simbolo</label><input class="form-input" id="f-symbol" placeholder="DAX, GOLD..."></div>
      <div class="form-group"><label class="form-label">Timeframe</label><select class="form-select" id="f-tf"><option>M5</option><option>M10</option><option>M15</option><option>M30</option><option>H1</option></select></div>
    </div>
    <div class="form-group"><label class="form-label">Nome EA</label><input class="form-input" id="f-ea" placeholder="DAX_TF_Simple, Gold_Reversal..."></div>
    <div class="form-row">
      <div class="form-group"><label class="form-label">Dal</label><input class="form-input" id="f-data" type="date" value="${new Date().toISOString().slice(0,10)}"></div>
      <div class="form-group"><label class="form-label">Broker</label><input class="form-input" id="f-broker" value="Skilling"></div>
    </div>
    <div class="form-row">
      <div class="form-group"><label class="form-label">P&L attuale</label><input class="form-input" id="f-pnl" placeholder="+120 / -45..."></div>
      <div class="form-group"><label class="form-label">N. trade</label><input class="form-input" id="f-trade" placeholder="12"></div>
    </div>
    <div class="form-group"><label class="form-label">Win rate</label><input class="form-input" id="f-winrate" placeholder="55%"></div>
    <div class="form-group"><label class="form-label">Note</label><textarea class="form-textarea" id="f-note" placeholder="Osservazioni..."></textarea></div>
    <div class="modal-actions">
      <button class="btn-cancel" onclick="closeModal()">Annulla</button>
      <button class="btn-save" onclick="saveDemo()">Salva</button>
    </div>`,

  pipeline: () => `
    <div class="modal-title">Aggiungi alla pipeline</div>
    <div class="form-row">
      <div class="form-group"><label class="form-label">Simbolo</label><input class="form-input" id="f-symbol" placeholder="DAX, GOLD, NQ..."></div>
      <div class="form-group"><label class="form-label">Timeframe</label><select class="form-select" id="f-tf"><option>M5</option><option>M10</option><option>M15</option><option>M30</option><option>H1</option></select></div>
    </div>
    <div class="form-group"><label class="form-label">Strategia</label><input class="form-input" id="f-strategia" placeholder="UA_Reversal_BB, UA_TrendDeveloper..."></div>
    <div class="form-group"><label class="form-label">Priorità</label><select class="form-select" id="f-priorita"><option value="alta">Alta</option><option value="media" selected>Media</option><option value="bassa">Bassa</option></select></div>
    <div class="form-group"><label class="form-label">Motivo / note</label><textarea class="form-textarea" id="f-motivo" placeholder="Hurst 0.38 su M15, da testare con Reversal_BB..."></textarea></div>
    <div class="modal-actions">
      <button class="btn-cancel" onclick="closeModal()">Annulla</button>
      <button class="btn-save" onclick="savePipeline()">Salva</button>
    </div>`,

  risultato: () => `
    <div class="modal-title">Risultato backtest</div>
    <div class="form-row">
      <div class="form-group"><label class="form-label">Simbolo</label><input class="form-input" id="f-symbol" placeholder="DAX, GOLD..."></div>
      <div class="form-group"><label class="form-label">Timeframe</label><select class="form-select" id="f-tf"><option>M5</option><option>M10</option><option>M15</option><option>M30</option><option>H1</option></select></div>
    </div>
    <div class="form-group"><label class="form-label">Strategia</label><input class="form-input" id="f-strategia" placeholder="UA_TF_Simple..."></div>
    <div class="form-row">
      <div class="form-group"><label class="form-label">Profit Factor</label><input class="form-input" id="f-pf" placeholder="1.45"></div>
      <div class="form-group"><label class="form-label">N. trade</label><input class="form-input" id="f-trade" placeholder="92"></div>
    </div>
    <div class="form-row">
      <div class="form-group"><label class="form-label">Max DD %</label><input class="form-input" id="f-dd" placeholder="8.5"></div>
      <div class="form-group"><label class="form-label">Win rate</label><input class="form-input" id="f-winrate" placeholder="52%"></div>
    </div>
    <div class="form-row">
      <div class="form-group"><label class="form-label">Hurst medio</label><input class="form-input" id="f-hurst" placeholder="0.62"></div>
      <div class="form-group"><label class="form-label">Esito</label><select class="form-select" id="f-esito"><option value="pass">PASS</option><option value="review">REVIEW</option><option value="fail">FAIL</option></select></div>
    </div>
    <div class="form-group"><label class="form-label">Data</label><input class="form-input" id="f-data" type="date" value="${new Date().toISOString().slice(0,10)}"></div>
    <div class="form-group"><label class="form-label">Note</label><textarea class="form-textarea" id="f-note" placeholder="Osservazioni, prossimi step..."></textarea></div>
    <div class="modal-actions">
      <button class="btn-cancel" onclick="closeModal()">Annulla</button>
      <button class="btn-save" onclick="saveRisultato()">Salva</button>
    </div>`,

  hurst: () => `
    <div class="modal-title">Aggiorna regime Hurst</div>
    <div class="form-group"><label class="form-label">Simbolo</label><input class="form-input" id="f-symbol" placeholder="GOLD, DAX, NQ..."></div>
    <div class="form-row">
      <div class="form-group"><label class="form-label">M5</label><input class="form-input" id="f-m5" placeholder="0.62"></div>
      <div class="form-group"><label class="form-label">M15</label><input class="form-input" id="f-m15" placeholder="0.58"></div>
    </div>
    <div class="form-row">
      <div class="form-group"><label class="form-label">M30</label><input class="form-input" id="f-m30" placeholder="0.44"></div>
      <div class="form-group"><label class="form-label">H1</label><input class="form-input" id="f-h1" placeholder="0.39"></div>
    </div>
    <div class="modal-actions">
      <button class="btn-cancel" onclick="closeModal()">Annulla</button>
      <button class="btn-save" onclick="saveHurst()">Salva</button>
    </div>`
};

let currentModal = null;

function openModal(type) {
  currentModal = type;
  document.getElementById('modal-content').innerHTML = modalTemplates[type]();
  document.getElementById('modal-overlay').classList.add('open');
}

function closeModal() {
  document.getElementById('modal-overlay').classList.remove('open');
  currentModal = null;
}

function closeModalOutside(e) {
  if(e.target === document.getElementById('modal-overlay')) closeModal();
}

// ===================== SAVE FUNCTIONS =====================
function g(id) { const el = document.getElementById(id); return el ? el.value.trim() : ''; }

function saveBacktest() {
  const d = { id:uid(), symbol:g('f-symbol'), tf:g('f-tf'), strategia:g('f-strategia'), stato:g('f-stato'), tipo:g('f-tipo'), data:g('f-data'), note:g('f-note') };
  if(!d.symbol) return;
  const arr = load('backtest'); arr.push(d); save('backtest', arr);
  closeModal(); render(); addLog(`Backtest aggiunto: ${d.symbol} ${d.tf} — ${d.strategia}`, 'ok');
}

function saveDemo() {
  const d = { id:uid(), symbol:g('f-symbol'), tf:g('f-tf'), ea:g('f-ea'), data:g('f-data'), broker:g('f-broker'), pnl:g('f-pnl'), trade:g('f-trade'), winrate:g('f-winrate'), note:g('f-note') };
  if(!d.symbol) return;
  const arr = load('demo'); arr.push(d); save('demo', arr);
  closeModal(); render(); addLog(`Demo aggiunta: ${d.ea} su ${d.symbol} ${d.tf}`, 'ok');
}

function savePipeline() {
  const d = { id:uid(), symbol:g('f-symbol'), tf:g('f-tf'), strategia:g('f-strategia'), priorita:g('f-priorita'), motivo:g('f-motivo') };
  if(!d.symbol) return;
  const arr = load('pipeline'); arr.push(d); save('pipeline', arr);
  closeModal(); render(); addLog(`Pipeline: ${d.symbol} ${d.tf} — ${d.strategia} [${d.priorita}]`, 'ok');
}

function saveRisultato() {
  const d = { id:uid(), symbol:g('f-symbol'), tf:g('f-tf'), strategia:g('f-strategia'), pf:g('f-pf'), trade:g('f-trade'), dd:g('f-dd'), winrate:g('f-winrate'), hurst:g('f-hurst'), esito:g('f-esito'), data:g('f-data'), note:g('f-note') };
  if(!d.symbol) return;
  const arr = load('risultati'); arr.push(d); save('risultati', arr);
  closeModal(); render(); addLog(`Risultato: ${d.symbol} ${d.tf} PF=${d.pf} → ${d.esito?.toUpperCase()}`, 'ok');
}

function saveHurst() {
  const symbol = g('f-symbol').toUpperCase();
  if(!symbol) return;
  const arr = load('hurst');
  const idx = arr.findIndex(r => r.symbol === symbol);
  const row = { symbol, M5:g('f-m5'), M15:g('f-m15'), M30:g('f-m30'), H1:g('f-h1') };
  if(idx >= 0) arr[idx] = row; else arr.push(row);
  save('hurst', arr);
  closeModal(); render(); addLog(`Hurst aggiornato: ${symbol}`, 'ok');
}

// ===================== AI AGENT =====================
async function sendToAgent() {
  const input = document.getElementById('agent-input');
  const btn = document.getElementById('agent-btn');
  const text = input.value.trim();
  if(!text) return;

  btn.disabled = true;
  btn.textContent = '...';

  const apiKey = getApiKey();
  if(!apiKey) {
    addLog('API key mancante — clicca ⚙ API Key in alto', 'err');
    btn.disabled = false; btn.textContent = 'Aggiorna →';
    toggleApiPanel();
    return;
  }

  const context = {
    backtest: load('backtest'),
    demo: load('demo'),
    pipeline: load('pipeline'),
    risultati: load('risultati'),
    hurst: load('hurst')
  };

  const prompt = `Sei l'agente della dashboard "CFD Regime Profiler" di Andrea, trader di CFD.
La dashboard ha 5 sezioni: backtest (in corso), demo (live su MT4), pipeline (prossimi da testare), risultati (backtest completati), hurst (matrice Hurst per strumento/timeframe).

Stato attuale:
${JSON.stringify(context, null, 2)}

Messaggio di Andrea: "${text}"

Analizza il messaggio e rispondi con un JSON con questa struttura esatta:
{
  "azione": "descrizione breve di cosa hai fatto",
  "sezione": "backtest|demo|pipeline|risultati|hurst|nessuna",
  "operazione": "aggiungi|aggiorna|elimina|nessuna",
  "dati": { ... oggetto con i campi rilevanti estratti dal messaggio ... }
}

Campi disponibili per sezione:
- backtest: symbol, tf, strategia, stato, tipo, data, note
- demo: symbol, tf, ea, data, broker, pnl, trade, winrate, note
- pipeline: symbol, tf, strategia, priorita (alta/media/bassa), motivo
- risultati: symbol, tf, strategia, pf, trade, dd, winrate, hurst, esito (pass/review/fail), data, note
- hurst: symbol, M5, M15, M30, H1

Rispondi SOLO con il JSON, niente altro.`;

  try {
    const resp = await fetch('https://api.anthropic.com/v1/messages', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
        'x-api-key': apiKey,
        'anthropic-version': '2023-06-01',
        'anthropic-dangerous-direct-browser-access': 'true'
      },
      body: JSON.stringify({
        model: 'claude-sonnet-4-20250514',
        max_tokens: 1000,
        messages: [{ role: 'user', content: prompt }]
      })
    });

    const data = await resp.json();
    if(data.error) throw new Error(data.error.message);
    const raw = data.content[0].text.trim();
    const clean = raw.replace(/```json|```/g,'').trim();
    const result = JSON.parse(clean);

    if(result.sezione !== 'nessuna' && result.operazione === 'aggiungi' && result.dati) {
      const arr = load(result.sezione);
      arr.push({ id: uid(), data: new Date().toISOString().slice(0,10), ...result.dati });
      save(result.sezione, arr);
      render();
    }

    addLog(result.azione || 'Fatto.', 'ok');
    input.value = '';

  } catch(e) {
    addLog('Errore agente: ' + e.message, 'err');
  }

  btn.disabled = false;
  btn.textContent = 'Aggiorna →';
}

// Invia con Ctrl+Enter
document.addEventListener('DOMContentLoaded', () => {
  document.getElementById('agent-input').addEventListener('keydown', e => {
    if(e.key === 'Enter' && e.ctrlKey) sendToAgent();
  });
});

// ===================== INSIGHTS =====================
async function generateInsights() {
  const btn = document.getElementById('insights-btn');
  const body = document.getElementById('insights-body');

  const backtest = load('backtest');
  const demo = load('demo');
  const pipeline = load('pipeline');
  const risultati = load('risultati');
  const hurst = load('hurst');

  const totDati = backtest.length + demo.length + risultati.length + hurst.length;
  if(totDati === 0) {
    body.innerHTML = '<div class="empty-state">Inserisci prima dei dati nelle altre sezioni.</div>';
    return;
  }

  btn.disabled = true;
  btn.textContent = '...';
  body.innerHTML = '<div class="insights-loading"><div class="spinner"></div>Analisi in corso...</div>';

  const apiKey = getApiKey();
  if(!apiKey) {
    body.innerHTML = '<div class="empty-state">API key mancante.<br>Clicca ⚙ API Key in alto per inserirla.</div>';
    btn.disabled = false; btn.textContent = '⟳ Analizza';
    toggleApiPanel();
    return;
  }

  const prompt = `Sei un analista quantitativo esperto di trading sistematico CFD. Stai analizzando il laboratorio di Andrea, trader italiano esperto che usa il metodo Unger Academy combinato con l'esponente di Hurst per classificare il regime di mercato.

METODOLOGIA:
- Hurst > 0.55 = trending → strategie Trend Following (UA_Mirrored_TF, UA_TrendDeveloper, UA_brkHighsNLowsN)
- Hurst 0.45-0.55 = neutro/random → BIAS o evitare
- Hurst < 0.45 = mean reverting → strategie Reversal (UA_Reversal_BB, UA_LevelFader)
- Criteri pass backtest: Profit Factor > 1.3, minimo 100 trade, Max DD accettabile < 20%

DATI ATTUALI DEL LAB:
Backtest in corso: ${JSON.stringify(backtest)}
Strategie in demo: ${JSON.stringify(demo)}
Risultati backtest completati: ${JSON.stringify(risultati)}
Pipeline (da testare): ${JSON.stringify(pipeline)}
Matrice Hurst: ${JSON.stringify(hurst)}

Analizza tutto e produci da 3 a 6 insights concreti e utili. Pensa a:
- EA che funzionano su più strumenti (opportunità di diversificazione)
- EA in demo che stanno sottoperformando rispetto al backtest (possibile regime change)
- Strumenti con Hurst favorevole ma nessuna strategia ancora testata (gap da colmare)
- Risultati fail o review che suggeriscono riformulazione
- Pattern tra strumenti simili (es. Gold e Silver si comportano spesso allo stesso modo)
- Prossima cosa concreta da fare in ordine di priorità

Rispondi SOLO con questo JSON, niente altro:
{
  "insights": [
    {
      "tipo": "opportunity|warning|action|stop|info",
      "titolo": "Titolo breve e diretto",
      "testo": "Spiegazione concreta, max 2 righe, dati specifici dove disponibili",
      "tag": "opportunità|attenzione|da fare|stop|info"
    }
  ],
  "aggiornato": "timestamp ISO"
}`;

  try {
    const resp = await fetch('https://api.anthropic.com/v1/messages', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
        'x-api-key': apiKey,
        'anthropic-version': '2023-06-01',
        'anthropic-dangerous-direct-browser-access': 'true'
      },
      body: JSON.stringify({
        model: 'claude-sonnet-4-20250514',
        max_tokens: 1500,
        messages: [{ role: 'user', content: prompt }]
      })
    });

    const data = await resp.json();
    if(data.error) throw new Error(data.error.message);
    const raw = data.content[0].text.trim().replace(/```json|```/g,'').trim();
    const result = JSON.parse(raw);

    // Salva insights
    save('insights', result.insights || []);
    save('insights_ts', new Date().toISOString());

    renderInsights();
    addLog(`Insights aggiornati — ${result.insights?.length || 0} osservazioni`, 'ok');

  } catch(e) {
    body.innerHTML = '<div class="empty-state">Errore analisi: ' + e.message + '</div>';
    addLog('Errore insights: ' + e.message, 'err');
  }

  btn.disabled = false;
  btn.textContent = '⟳ Analizza';
}

function renderInsights() {
  const insights = load('insights');
  const ts = localStorage.getItem('crp_insights_ts');
  const body = document.getElementById('insights-body');
  const cnt = document.getElementById('cnt-insights');

  cnt.textContent = insights.length;

  if(!insights.length) {
    body.innerHTML = '<div class="empty-state">Nessun insight ancora.<br>Clicca "Analizza" dopo aver inserito dati nelle altre sezioni.</div>';
    return;
  }

  const iconMap = { opportunity:'◆', warning:'▲', action:'→', stop:'✕', info:'○' };
  const classMap = { opportunity:'', warning:'warn', action:'purple', stop:'danger', info:'' };
  const tagClassMap = { 'opportunità':'tag-opportunity', 'attenzione':'tag-warning', 'da fare':'tag-action', 'stop':'tag-stop', 'info':'tag-info' };

  const tsStr = ts ? new Date(ts).toLocaleString('it-IT') : '';

  body.innerHTML = insights.map(ins => `
    <div class="insight-card ${classMap[ins.tipo]||''}">
      <div class="insight-icon">${iconMap[ins.tipo]||'○'}</div>
      <div class="insight-body">
        <div class="insight-title">${ins.titolo}</div>
        <div class="insight-text">${ins.testo}</div>
        ${ins.tag ? `<span class="insight-tag ${tagClassMap[ins.tag]||'tag-info'}">${ins.tag}</span>` : ''}
      </div>
    </div>`).join('') +
    (tsStr ? `<div class="insights-meta">Ultimo aggiornamento: ${tsStr}</div>` : '');
}


function addLog(msg, type='') {
  const logs = load('log');
  const now = new Date();
  const time = now.toTimeString().slice(0,5);
  logs.unshift({ time, msg, type });
  if(logs.length > 30) logs.pop();
  save('log', logs);
  renderLog();
}

function renderLog() {
  const logs = load('log');
  const el = document.getElementById('agent-log');
  if(!logs.length) { el.innerHTML = ''; return; }
  el.innerHTML = logs.slice(0,8).map(l =>
    `<div class="log-entry"><span class="log-time">${l.time}</span><span class="log-msg ${l.type||''}">${l.msg}</span></div>`
  ).join('');
}

// ===================== API KEY =====================
function getApiKey() {
  return localStorage.getItem('crp_api_key') || '';
}

function saveApiKey(val) {
  localStorage.setItem('crp_api_key', val.trim());
  updateApiStatus();
}

function updateApiStatus() {
  const key = getApiKey();
  const dot = document.getElementById('api-dot');
  const btn = document.getElementById('api-btn');
  if(key && key.startsWith('sk-ant')) {
    dot.style.background = 'var(--green)';
    dot.style.boxShadow = '0 0 8px var(--green)';
    btn.style.color = 'var(--green)';
    btn.style.borderColor = 'rgba(48,208,128,0.4)';
  } else {
    dot.style.background = 'var(--warn)';
    dot.style.boxShadow = '0 0 8px var(--warn)';
    btn.style.color = 'var(--warn)';
    btn.style.borderColor = 'rgba(240,160,32,0.4)';
  }
}

function toggleApiPanel() {
  const panel = document.getElementById('api-panel');
  const input = document.getElementById('api-key-input');
  const isOpen = panel.style.display === 'flex';
  panel.style.display = isOpen ? 'none' : 'flex';
  if(!isOpen) { input.value = getApiKey(); setTimeout(()=>input.focus(),50); }
}

// ===================== CLOCK =====================
function updateClock() {
  document.getElementById('clock').textContent = new Date().toLocaleTimeString('it-IT');
}
setInterval(updateClock, 1000);
updateClock();

// ===================== INIT =====================
render();
renderLog();
renderInsights();
updateApiStatus();
</script>
</body>
</html>
