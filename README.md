# drowoooo.github.io
<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>Estudo de Análise Combinatória</title>
<meta name="description" content="Aplicativo web (arquivo único) para estudar combinatória: combinações, arranjos, permutações e princípio da contagem, com listagem, histórico e exportação."/>
<!-- MathJax para fórmulas -->
<script>
  window.MathJax = { tex: { inlineMath: [['$', '$'], ['\\(', '\\)']] }, svg:{fontCache:'global'} };
</script>
<script defer src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-svg.js"></script>
<!-- jsPDF e autotable para exportar PDF -->
<script defer src="https://cdn.jsdelivr.net/npm/jspdf@2.5.1/dist/jspdf.umd.min.js"></script>
<script defer src="https://cdn.jsdelivr.net/npm/jspdf-autotable@3.8.2/dist/jspdf.plugin.autotable.min.js"></script>

<style>
:root{
  --bg: #0b0c10;
  --surface: #111218;
  --muted: #9aa0aa;
  --text: #e8eaf0;
  --accent: #7c3aed; /* roxo */
  --accent-2: #22c55e; /* verde */
  --accent-pink: #ff8fa3; /* salmão/rosa */
  --card: #151827;
  --border: #252a3a;
  --shadow: 0 10px 30px rgba(0,0,0,.25);
  --glass: rgba(255,255,255,0.03);
}
[data-theme="light"]{
  --bg: #f7f7fb;
  --surface:#ffffff;
  --muted:#5f6675;
  --text:#12131a;
  --accent:#6d28d9;
  --accent-2:#16a34a;
  --accent-pink: #ff6f91;
  --card:#ffffff;
  --border:#e5e7eb;
  --shadow: 0 10px 25px rgba(0,0,0,.08);
  --glass: rgba(0,0,0,0.02);
}
*{box-sizing:border-box}
html,body{height:100%}
body{
  margin:0; font-family: ui-sans-serif, system-ui, -apple-system, "Segoe UI", Roboto, Inter, "Helvetica Neue", Arial, "Noto Sans";
  background: var(--bg); color: var(--text); transition: background .35s ease, color .35s ease;
}
.container{ max-width: 1200px; margin: 0 auto; padding: 24px; }
header{ display:flex; gap:16px; align-items:center; justify-content:space-between; margin-bottom: 18px; }
.brand{ display:flex; align-items:center; gap:12px; }
.logo{ width:46px; height:46px; border-radius:12px; background: linear-gradient(135deg,var(--accent), var(--accent-pink)); box-shadow: var(--shadow); display:grid; place-items:center; transform-origin:center; animation:float 4s ease-in-out infinite; }
@keyframes float { 0%{ transform: translateY(0) } 50%{ transform: translateY(-6px) } 100%{ transform: translateY(0) } }
.logo svg{ width:28px; height:28px; filter: drop-shadow(0 6px 10px rgba(0,0,0,0.18)); }

h1{ font-size: clamp(20px, 3vw, 28px); margin:0; letter-spacing:.2px; }
.subtitle{ color: var(--muted); font-size: 14px; margin-top:4px }
.row{ display:flex; flex-wrap:wrap; gap:12px; align-items:center; }
.grow{ flex:1 1 320px }
.toolbar{ display:flex; gap:8px; align-items:center; flex-wrap:wrap }
.btn{
  padding:10px 14px; border-radius:14px; border:1px solid var(--border); background: linear-gradient(180deg,var(--glass), transparent); color:var(--text);
  cursor:pointer; transition: transform .12s cubic-bezier(.2,.9,.2,1), background .18s ease, border-color .18s ease, box-shadow .18s ease; box-shadow: var(--shadow);
  display:inline-flex; align-items:center; gap:8px;
}
.btn:hover{ transform: translateY(-4px) scale(1.01); box-shadow: 0 14px 36px rgba(0,0,0,0.28) }
.btn.accent{ background: linear-gradient(135deg, var(--accent), var(--accent-pink)); border-color: transparent; color: #fff; }
.btn.success{ background: linear-gradient(135deg, var(--accent-2), #34d399); color:#06260f; border-color: transparent; }
.btn.ghost{ background: transparent; border-color: var(--border) }
.btn.small{ padding:8px 10px; border-radius:12px; font-size: 13px; }

.tabs{ display:flex; gap:8px; flex-wrap:wrap; margin-top:10px }
.tab{ padding:10px 14px; border-radius: 999px; border:1px solid var(--border); cursor:pointer; background: var(--surface); color:var(--text); transition: transform .12s ease }
.tab:hover{ transform: translateY(-3px) }
.tab.active{ background: linear-gradient(135deg, var(--accent), var(--accent-pink)); color:#fff; border-color: transparent; box-shadow: 0 8px 24px rgba(0,0,0,0.18); }

.card{ background: var(--card); border:1px solid var(--border); border-radius: 18px; padding: 16px; box-shadow: var(--shadow); transition: transform .18s ease; }
.card:hover{ transform: translateY(-4px) }

label{ display:block; font-weight:600; font-size: 13px; margin-bottom: 6px; color: var(--muted); }
input[type="text"], input[type="number"], textarea, select{
  width:100%; padding:10px 12px; border-radius:12px; border:1px solid var(--border); background: var(--surface); color: var(--text); outline:none;
  transition: box-shadow .12s ease, border-color .12s ease;
}
textarea{ min-height:84px; resize: vertical; }
input[type="text"]:focus, input[type="number"]:focus, textarea:focus, select:focus{ box-shadow: 0 8px 22px rgba(0,0,0,0.25); border-color: var(--accent-pink); }

.pill{ padding:6px 10px; border-radius: 999px; border:1px dashed var(--border); display:inline-flex; align-items:center; gap:8px; margin:4px 6px 0 0; background: linear-gradient(0deg, transparent, rgba(255,255,255,0.01)); }
.pill .x{ width:18px; height:18px; border-radius:6px; background: var(--border); display:inline-grid; place-items:center; cursor:pointer; }

.grid{ display:grid; grid-template-columns: repeat(12, 1fr); gap:12px }
.col-12{ grid-column: span 12 }
.col-8{ grid-column: span 8 }
.col-6{ grid-column: span 6 }
.col-4{ grid-column: span 4 }
.col-3{ grid-column: span 3 }
@media (max-width:960px){ .col-8{grid-column:span 12} .col-6{grid-column:span 12} .col-4{grid-column:span 12} .col-3{grid-column:span 6} }

.help{ font-size: 13px; color: var(--muted); }
.kbd{ font-family: ui-monospace, SFMono-Regular, Menlo, Consolas, monospace; font-size:12px; padding:2px 6px; border:1px solid var(--border); border-radius:6px; background: var(--surface); }

.result-header{ display:flex; align-items:center; justify-content:space-between; gap:10px; flex-wrap:wrap }
.total{ font-weight:700 }

.list{ border:1px solid var(--border); border-radius:14px; overflow:hidden; }
.list-header, .list-item{ display:grid; grid-template-columns: 60px 1fr 56px; gap:12px; padding:10px 12px; align-items:center; }
.list-header{ background: var(--surface); font-size: 12px; color: var(--muted); position: sticky; top:0; z-index:1 }
.list-item{ border-top:1px solid var(--border); background: linear-gradient(0deg, transparent, rgba(255,255,255,0.01)); }
.mono{ font-family: ui-monospace, SFMono-Regular, Menlo, Consolas, monospace; font-size: 13px; }

.pagination{ display:flex; gap:8px; align-items:center; justify-content:center; padding:12px }

.fade-enter{ opacity:0; transform: translateY(6px) }
.fade-enter-active{ opacity:1; transform:none; transition: opacity .28s cubic-bezier(.2,.9,.2,1), transform .28s cubic-bezier(.2,.9,.2,1) }

.chip{ display:inline-flex; align-items:center; gap:6px; border:1px solid var(--border); padding:6px 10px; border-radius:999px; background: var(--surface); font-size:12px; }

.hint{ display:flex; gap:8px; align-items:flex-start; background: linear-gradient(0deg, rgba(255,143,163,.06), transparent); border:1px solid var(--border); padding:12px; border-radius: 14px; }

.footer-note{ color:var(--muted); font-size:12px; text-align:center; padding:18px 0; }

/* House cards */
.house{ border:1px dashed var(--border); border-radius:14px; padding:12px; margin:8px 0; background: var(--surface); }
.house h4{ margin:0 0 8px 0; font-size:14px }
.house .row{ gap:12px; flex-wrap:wrap }
.house .block{ min-width: 220px; flex: 1 1 220px; }
.house .checks label{ display:inline-flex; align-items:center; gap:6px; margin-right:10px; font-weight:500 }

/* Spinner pequeno */
.spinner{
  display:inline-block; width:18px; height:18px; border-radius:50%;
  border:3px solid rgba(255,255,255,0.08); border-top-color: var(--accent-pink); animation:spin .9s linear infinite;
}
@keyframes spin{ to{ transform:rotate(360deg) } }

/* copy tooltip */
.copy-btn{ cursor:pointer; border-radius:8px; padding:6px; display:inline-grid; place-items:center; width:40px; height:36px; border:1px solid var(--border); background:var(--surface) }
.copy-tooltip{ position:relative; }
.copy-tooltip .tip{ position:absolute; top:-36px; right:0; background:var(--card); border:1px solid var(--border); padding:6px 8px; border-radius:8px; font-size:12px; opacity:0; transform:translateY(6px); transition: opacity .18s ease, transform .18s ease; pointer-events:none; }
.copy-tooltip.show .tip{ opacity:1; transform:none; }

/* pesquisa */
.search-input{ padding:8px 10px; border-radius:10px; border:1px solid var(--border); background:var(--surface); }

/* pequenas animações do tema */
.theme-icon{ transition: transform .25s ease, opacity .25s ease; display:inline-block; }
.theme-icon.rotated{ transform: rotate(140deg); opacity:.9; }

/* mobile tweaks */
@media (max-width:680px){
  .toolbar{ gap:6px }
}
</style>
</head>
<body data-theme="dark">
<div class="container">
  <header>
    <div class="brand">
      <div class="logo" aria-hidden="true">
        <!-- ícone simples do logo (calc + pi) -->
        <svg viewBox="0 0 24 24" fill="none" aria-hidden="true">
          <path d="M5 12h14M12 5v14" stroke="white" stroke-width="1.4" stroke-linecap="round" stroke-linejoin="round" />
        </svg>
      </div>
      <div>
        <h1>App de Análise Combinatória</h1>
        <div class="subtitle">Combinações, arranjos, permutações e princípio da contagem • arquivo único</div>
      </div>
    </div>
    <div class="toolbar">
      <!-- botão de tema com SVG de lua simples -->
      <button id="themeBtn" class="btn small" title="Alternar tema" aria-pressed="false">
        <span id="themeIcon" class="theme-icon" style="display:inline-block">
          <!-- simple moon shape -->
          <svg viewBox="0 0 24 24" width="16" height="16" fill="none" aria-hidden="true">
            <path id="moonPath" d="M21 12.3A8.3 8.3 0 1 1 11.7 3 6.8 6.8 0 0 0 21 12.3z" fill="currentColor"></path>
          </svg>
        </span>
        <span id="themeLabel" style="font-size:13px">Tema</span>
      </button>

      <button id="savePreset" class="btn small ghost" title="Salvar preset">💾 Salvar preset</button>
      <select id="presetSelect" class="btn small ghost" style="min-width:160px;">
        <option value="">Carregar preset...</option>
      </select>

      <button id="clearHistory" class="btn small ghost" title="Limpar histórico">🧹 Limpar histórico</button>
      <a id="exportHistory" class="btn small ghost" href="#" download>📦 Exportar histórico (JSON)</a>
      <span class="chip">Limite de listagem: <span id="limitLabel" class="mono">10.000</span></span>
    </div>
  </header>

  <nav class="tabs" role="tablist" aria-label="Módulos">
    <button class="tab active" data-tab="formulas">Elementos & Fórmulas</button>
    <button class="tab" data-tab="contagem">Princípio da Contagem</button>
    <button class="tab" data-tab="historico">Histórico</button>
  </nav>

  <!-- Módulo 1: Elementos & Fórmulas -->
  <section id="tab-formulas" class="fade-enter-active" role="tabpanel" aria-labelledby="Elementos & Fórmulas">
    <div class="grid" style="margin-top:12px">
      <div class="col-8">
        <div class="card">
          <div class="grid">
            <div class="col-12">
              <label>Elementos (separados por vírgula, quebra de linha ou espaço)</label>
              <textarea id="elementsInput" placeholder="Ex.: João, Maria, Pedro
ou
A B C D
ou
1, 2, 3, 4"></textarea>
              <div style="display:flex; gap:8px; align-items:center; margin-top:8px">
                <div class="help">Dica: você pode colar de uma planilha.</div>
                <label style="margin-left:auto"><input type="checkbox" id="removeDup"> Remover duplicatas</label>
              </div>
            </div>
            <div class="col-6">
              <label>Fórmula</label>
              <select id="formulaSelect">
                <option value="comb">Combinação C(n,k) — ordem não importa</option>
                <option value="comb_rep">Combinação com repetição — C(n+k-1, k)</option>
                <option value="arr">Arranjo A(n,k) — ordem importa</option>
                <option value="arr_rep">Arranjo com repetição — n^k</option>
                <option value="perm">Permutação P(n) — todos os elementos</option>
                <option value="perm_rep">Permutação com repetição — n!/∏ m_i!</option>
              </select>
            </div>
            <div class="col-3">
              <label>k (tamanho)</label>
              <input id="kInput" type="number" min="0" step="1" value="2" />
            </div>
            <div class="col-3">
              <label>Exibir por página</label>
              <input id="pageSize" type="number" min="10" step="10" value="200"/>
            </div>
            <div class="col-12">
              <div class="hint">
                <div>💡</div>
                <div>
                  <div id="dynamicText" style="font-weight:600">Selecione uma fórmula para ver a explicação, dica e fórmula em $\\LaTeX$.</div>
                  <div class="help">As fórmulas são renderizadas com MathJax.</div>
                </div>
              </div>
            </div>
            <div class="col-12" style="display:flex; gap:8px; flex-wrap:wrap; margin-top:6px">
              <button id="calcBtn" class="btn accent">Calcular</button>
              <button id="listBtn" class="btn">Listar resultados</button>
              <button id="sampleBtn" class="btn small ghost" title="Mostrar amostra aleatória">🎲 Amostra</button>
              <input id="searchList" class="search-input" placeholder="Pesquisar na listagem..." style="max-width:220px"/>
              <button id="exportCSV" class="btn ghost" disabled>Exportar CSV</button>
              <button id="exportTXT" class="btn ghost" disabled>Exportar TXT</button>
              <button id="exportPDF" class="btn ghost" disabled>Exportar PDF</button>
            </div>
          </div>
        </div>

        <div class="card" style="margin-top:12px">
          <div class="result-header">
            <div>
              <div><span class="total">Total:</span> <span id="totalCount" class="mono">—</span></div>
              <div id="countFormula" class="help mono"></div>
            </div>
            <div class="chip" id="statusWrap"><span id="statusChip">Pronto</span></div>
          </div>
          <div id="listContainer" class="list" style="margin-top:10px; display:none">
            <div class="list-header"><div>#</div><div>Item</div><div>⤓</div></div>
            <div id="listItems"></div>
            <div class="pagination">
              <button id="prevPage" class="btn small" disabled>◀</button>
              <span id="pageInfo" class="mono">—</span>
              <button id="nextPage" class="btn small" disabled>▶</button>
            </div>
          </div>
        </div>
      </div>

      <aside class="col-4">
        <div class="card">
          <h3 style="margin:4px 0 8px 0">Elementos reconhecidos</h3>
          <div id="elementsPreview"></div>
          <div class="help" style="margin-top:8px">Clique no ❌ para remover. Duplicatas aparecem quando você quer permutação com repetição.</div>
        </div>

        <div class="card" style="margin-top:12px">
          <h3 style="margin:4px 0 8px 0">Exemplo rápido</h3>
          <div class="help">Digite <span class="kbd">João, Maria, Pedro</span>, escolha <span class="kbd">C(n,k)</span> e defina <span class="kbd">k=2</span>. O total deve ser <span class="kbd">3</span>: {João,Maria}, {João,Pedro}, {Maria,Pedro}.</div>
        </div>
      </aside>
    </div>
  </section>

  <!-- Módulo 2: Princípio da Contagem -->
  <section id="tab-contagem" class="fade-enter" hidden>
    <div class="grid" style="margin-top:12px">
      <div class="col-8">
        <div class="card">
          <div class="grid">
            <div class="col-3">
              <label>Nº de casas (posições)</label>
              <input id="pc-houses" type="number" min="1" value="4">
            </div>

            <!-- NOVO: modo (Automático / Manual) -->
            <div class="col-4">
              <label>Modo</label>
              <select id="pc-mode">
                <option value="auto">Automático (com restrições)</option>
                <option value="manual">Manual (valores por casa)</option>
              </select>
            </div>

            <!-- Configurações do modo automático -->
            <div class="col-5 auto-field">
              <label>Tipo de elementos</label>
              <select id="pc-type">
                <option value="digits">Dígitos (0–9)</option>
                <option value="letters">Letras (A–Z)</option>
                <option value="custom">Personalizados (abaixo)</option>
              </select>
            </div>
            <div class="col-12 auto-field">
              <label>Elementos personalizados (selecione “Personalizados” acima)</label>
              <input id="pc-custom" type="text" placeholder="Ex.: A,B,C,1,2,3">
            </div>
            <div class="col-12 auto-field">
              <div class="row">
                <label style="margin:6px 0 0 6px"><input type="checkbox" id="pc-norep"> Sem repetição</label>
                <label style="margin:6px 0 0 6px"><input type="checkbox" id="pc-first-not-zero"> Primeira casa ≠ 0</label>
              </div>
            </div>

            <div class="col-12">
              <label>Restrições por casa</label>
              <div id="pc-houses-container"></div>
              <div class="help">Em cada casa você pode marcar: <span class="kbd">par</span>, <span class="kbd">ímpar</span>, <span class="kbd">vogal</span>, <span class="kbd">consoante</span>, além de <em>incluir apenas</em> e <em>excluir</em> elementos específicos (separados por <span class="kbd">|</span>).</div>
            </div>

            <div class="col-12 auto-field">
              <label>Excluir elementos específicos (global, opcional)</label>
              <input id="pc-exclude" type="text" placeholder="Ex.: 0|O|I">
            </div>

            <div class="col-12" style="display:flex; gap:8px; flex-wrap:wrap; margin-top:6px">
              <button id="pc-calc" class="btn accent">Calcular</button>
              <button id="pc-list" class="btn">Listar resultados</button>
              <button id="pc-exportCSV" class="btn ghost" disabled>Exportar CSV</button>
              <button id="pc-exportTXT" class="btn ghost" disabled>Exportar TXT</button>
              <button id="pc-exportPDF" class="btn ghost" disabled>Exportar PDF</button>
            </div>
          </div>
        </div>

        <div class="card" style="margin-top:12px">
          <div class="result-header">
            <div>
              <div><span class="total">Total:</span> <span id="pc-total" class="mono">—</span></div>
              <div class="help mono" id="pc-mult"></div>
            </div>
            <div class="chip">Listagem até <span id="pc-limit-label" class="mono">10.000</span> itens</div>
          </div>

          <div id="pc-listContainer" class="list" style="margin-top:10px; display:none">
            <div class="list-header"><div>#</div><div>Item</div><div>⤓</div></div>
            <div id="pc-listItems"></div>
            <div class="pagination">
              <button id="pc-prev" class="btn small" disabled>◀</button>
              <span id="pc-page" class="mono">—</span>
              <button id="pc-next" class="btn small" disabled>▶</button>
            </div>
          </div>
        </div>
      </div>

      <aside class="col-4">
        <div class="card">
          <h3 style="margin:4px 0 8px 0">Como funciona</h3>
          <div class="help">Defina as posições e as regras. O app calcula o produto (princípio fundamental da contagem) e, se quiser, lista todas as sequências válidas.</div>
        </div>
      </aside>
    </div>
  </section>

  <!-- Histórico -->
  <section id="tab-historico" class="fade-enter" hidden>
    <div class="card" style="margin-top:12px">
      <div class="result-header">
        <div>
          <strong>Histórico de contas</strong>
          <div class="help">Até 200 entradas, armazenadas no seu navegador (localStorage).</div>
        </div>
        <div class="row">
          <label style="margin-top:6px"><input type="checkbox" id="hist-autosave" checked> Salvar automaticamente</label>
        </div>
      </div>
      <div id="historyList" style="margin-top:10px"></div>
    </div>
  </section>

  <footer class="footer-note">Feito para estudos. Atenção: listar resultados pode ser pesado. Use a amostra ou o limite de itens para evitar travamentos.</footer>
</div>

<script>
(function(){
  // ======= Estado global =======
  const state = {
    theme: (localStorage.getItem('comb.theme') || 'dark'),
    limit: parseInt(localStorage.getItem('comb.limit')||'10000',10),
    pageSize: 200,
    list: [],
    page: 1,
    listPC: [],
    pagePC: 1,
    totalPC: 0,
    history: JSON.parse(localStorage.getItem('comb.history')||'[]').slice(0,200),
    autosave: localStorage.getItem('comb.autosave')!== 'false',
    presets: JSON.parse(localStorage.getItem('comb.presets')||'[]')
  };

  document.body.setAttribute('data-theme', state.theme);
  const el = id => document.getElementById(id);
  const $ = sel => document.querySelector(sel);

  // ======= Init UI =======
  el('limitLabel').textContent = state.limit.toLocaleString('pt-BR');
  el('pc-limit-label').textContent = state.limit.toLocaleString('pt-BR');

  // carregar presets no select
  function renderPresets(){
    const sel = el('presetSelect');
    sel.innerHTML = '<option value="">Carregar preset...</option>';
    state.presets.forEach((p, i) => {
      const opt = document.createElement('option');
      opt.value = String(i);
      opt.textContent = p.name || (`Preset ${i+1} — ${p.mode || p.formula || ''}`);
      sel.appendChild(opt);
    });
  }
  renderPresets();

  // ======= Tema =======
  const themeIcon = el('themeIcon'), themeLabel = el('themeLabel');
  function updateThemeUI(){
    const dark = state.theme === 'dark';
    el('themeBtn').setAttribute('aria-pressed', String(!dark));
    themeLabel.textContent = dark ? 'Tema: Escuro' : 'Tema: Claro';
    const moon = el('moonPath');
    // animação simples
    themeIcon.classList.toggle('rotated', !dark);
  }
  function toggleTheme(){
    state.theme = (state.theme==='dark'?'light':'dark');
    document.body.setAttribute('data-theme', state.theme);
    localStorage.setItem('comb.theme', state.theme);
    updateThemeUI();
  }
  el('themeBtn').addEventListener('click', toggleTheme);
  updateThemeUI();

  // ======= Abas =======
  document.querySelectorAll('.tab').forEach(btn=>{
    btn.addEventListener('click', ()=>{
      document.querySelectorAll('.tab').forEach(b=>b.classList.remove('active'));
      btn.classList.add('active');
      const tab = btn.dataset.tab;
      document.querySelectorAll('section[id^="tab-"]').forEach(s=>{
        if(s.id === 'tab-'+tab){ s.hidden=false; s.classList.add('fade-enter-active'); }
        else { s.hidden=true; s.classList.remove('fade-enter-active'); }
      });
    })
  })

  // ======= Utilidades matemáticas =======
  const factCache = {0n:1n,1n:1n};
  function fact(n){ n = BigInt(n); if(factCache[n]!=null) return factCache[n]; let r=1n; for(let i=2n;i<=n;i++) r*=i; factCache[n]=r; return r; }
  function nPk(n,k){ return k>n?0n: fact(n)/fact(n-k); }
  function nCk(n,k){ if(k<0||k>n) return 0n; k=Math.min(k,n-k); let num=1n, den=1n; for(let i=1n;i<=BigInt(k);i++){ num*=BigInt(n)-i+1n; den*=i; } return num/den; }
  function powBig(n,k){ let r=1n; n=BigInt(n); for(let i=0n;i<BigInt(k);i++) r*=n; return r; }

  function uniqCounts(arr){
    const m=new Map();
    for(const x of arr){ m.set(x,(m.get(x)||0)+1); }
    return m;
  }

  // ======= Parse dos elementos =======
  function parseElements(raw){
    if(!raw) return [];
    // divide por vírgula/; / quebra de linha / tab / múltiplos espaços
    const parts = raw.split(/[,;\n\t]+/).map(s=>s.trim()).filter(Boolean);
    if(parts.length>0) return parts;
    // fallback: split por espaços
    return raw.trim().split(/\s+/).filter(Boolean);
  }

  function renderElementsPreview(arr){
    const box = el('elementsPreview'); box.innerHTML='';
    arr.forEach((v,i)=>{
      const span = document.createElement('span'); span.className='pill';
      span.innerHTML = `<span class="mono">${escapeHTML(String(v))}</span> <span class="x" title="Remover" data-i="${i}">❌</span>`;
      box.appendChild(span);
    })
    // evento de remoção (re-declara para garantir listeners atualizados)
    box.querySelectorAll('.x').forEach(btn=>{
      btn.addEventListener('click',(e)=>{
        const i = parseInt(btn.dataset.i,10);
        const arrNow = parseElements(el('elementsInput').value);
        arrNow.splice(i,1);
        el('elementsInput').value = arrNow.join(', ');
        renderElementsPreview(arrNow);
      });
    });
  }

  function escapeHTML(s){ return s.replace(/[&<>"']/g, c=>({"&":"&amp;","<":"&lt;",">":"&gt;","\"":"&quot;","'":"&#39;"}[c])); }

  // ======= Texto dinâmico com ANÁFORAS =======
  const texts = {
    comb: {
      title: 'Combinação sem repetição',
      expl:
        'Quando você precisa formar um grupo sem se importar com a ordem; '+
        'Quando você quer contar subconjuntos distintos de tamanho k; '+
        'Quando a posição não altera o resultado, use combinações. '+
        'Fórmula: $C(n,k)=\\frac{n!}{k!(n-k)!}$.',
      tip: 'Quando formar times, comissões ou escolher questões para uma prova.'
    },
    comb_rep: {
      title: 'Combinação com repetição',
      expl:
        'Quando você pode repetir elementos; '+
        'Quando você monta multiconjuntos (ex.: bolas de sorvete); '+
        'Quando a ordem não importa mas repetições são permitidas. '+
        'Fórmula: $C(n+k-1,k)=\\frac{(n+k-1)!}{k!(n-1)!}$.',
      tip: 'Ex.: combinações de sabores onde mais de uma bola do mesmo sabor é permitida.'
    },
    arr: {
      title: 'Arranjo sem repetição',
      expl:
        'Quando a ordem importa; '+
        'Quando você escolhe k posições distintas entre n; '+
        'Quando cada elemento só pode aparecer uma vez na sequência. '+
        'Fórmula: $A(n,k)=\\frac{n!}{(n-k)!}$.',
      tip: 'Ex.: pódio, senha sem repetição, alocação de prêmios em posições.'
    },
    arr_rep: {
      title: 'Arranjo com repetição',
      expl:
        'Quando a ordem importa e repetições são permitidas; '+
        'Quando cada posição pode receber qualquer elemento do conjunto; '+
        'Quando você quer contar sequências de comprimento k com reposição. '+
        'Fórmula: $n^k$.',
      tip: 'Ex.: senhas numéricas onde dígitos podem se repetir.'
    },
    perm: {
      title: 'Permutação de n distintos',
      expl:
        'Quando você quer ordenar todos os elementos; '+
        'Quando cada posição é ocupada por um elemento diferente; '+
        'Quando o conjunto inteiro é rearranjado. '+
        'Fórmula: $P(n)=n!$.',
      tip: 'Ex.: todas as ordens possíveis de cartas distintas.'
    },
    perm_rep: {
      title: 'Permutação com repetição',
      expl:
        'Quando existem elementos repetidos; '+
        'Quando trocas entre iguais não criam ordens novas; '+
        'Quando o total é n! dividido pelos fatoriais das repetições. '+
        'Fórmula: $\\dfrac{n!}{m_1! m_2! \\cdots}$.',
      tip: 'Ex.: letras da palavra “ARARA”.'
    }
  };

  function updateDynamicText(){
    const key = el('formulaSelect').value; const t = texts[key];
    el('dynamicText').innerHTML = `<div style="font-weight:700">${t.title}</div><div class="help" style="margin-top:6px">${t.expl} — <em>${t.tip}</em></div>`;
    if(window.MathJax) MathJax.typesetPromise?.();
  }
  el('formulaSelect').addEventListener('change', updateDynamicText); updateDynamicText();

  // ======= Cálculo de totais =======
  function calcTotal(elements, k, mode){
    const n = elements.length;
    switch(mode){
      case 'comb': return { total: nCk(n,k), formula: `C(${n},${k})` };
      case 'comb_rep': return { total: nCk(n+k-1,k), formula: `C(${n+k-1},${k})` };
      case 'arr': return { total: nPk(n,k), formula: `A(${n},${k})` };
      case 'arr_rep': return { total: powBig(n,k), formula: `${n}^${k}` };
      case 'perm': return { total: fact(n), formula: `${n}!` };
      case 'perm_rep': {
        const counts = [...uniqCounts(elements).values()];
        let den = 1n; for(const c of counts) den *= fact(c);
        return { total: fact(n)/den, formula: `${n}!/(${counts.map(c=>c+'!').join('·')})` };
      }
    }
  }

  // ======= Geração / geradores =======
  function* combinationsNoRep(arr, k, start=0, prefix=[]){
    if(k===0){ yield prefix.slice(); return; }
    for(let i=start;i<=arr.length-k;i++){
      prefix.push(arr[i]);
      yield* combinationsNoRep(arr, k-1, i+1, prefix);
      prefix.pop();
    }
  }
  function* combinationsWithRep(arr,k, start=0, prefix=[]){
    if(k===0){ yield prefix.slice(); return; }
    for(let i=start;i<arr.length;i++){
      prefix.push(arr[i]);
      yield* combinationsWithRep(arr,k-1,i,prefix);
      prefix.pop();
    }
  }
  function* arrangementsNoRep(arr,k, used=new Array(arr.length).fill(false), prefix=[]){
    if(k===0){ yield prefix.slice(); return; }
    for(let i=0;i<arr.length;i++) if(!used[i]){
      used[i]=true; prefix.push(arr[i]);
      yield* arrangementsNoRep(arr,k-1,used,prefix);
      prefix.pop(); used[i]=false;
    }
  }
  function* arrangementsRep(arr,k,prefix=[]){
    if(k===0){ yield prefix.slice(); return; }
    for(let i=0;i<arr.length;i++){
      prefix.push(arr[i]);
      yield* arrangementsRep(arr,k-1,prefix);
      prefix.pop();
    }
  }
  function* multisetPermutations(items){
    const counts = new Map();
    for(const it of items) counts.set(it, (counts.get(it)||0)+1);
    const unique = [...counts.keys()];
    const n = items.length;
    const res = [];
    function* bt(depth){
      if(depth===n){ yield res.slice(); return; }
      for(const v of unique){
        const c = counts.get(v);
        if(c>0){
          counts.set(v, c-1);
          res.push(v);
          yield* bt(depth+1);
          res.pop();
          counts.set(v, c);
        }
      }
    }
    yield* bt(0);
  }

  // ======= UI e listagem — módulo Elementos & Fórmulas =======
  function currentElements(){
    const raw = el('elementsInput').value;
    let arr = parseElements(raw);
    if(el('removeDup') && el('removeDup').checked){
      arr = [...new Set(arr)];
    }
    renderElementsPreview(arr.slice());
    return arr;
  }

  function setStatus(msg, busy=false){
    const chip = el('statusChip');
    chip.textContent = msg;
    const wrap = el('statusWrap');
    if(busy){
      if(!wrap.querySelector('.spinner')){
        const s = document.createElement('span'); s.className='spinner'; s.style.marginLeft='8px';
        wrap.appendChild(s);
      }
    } else {
      const s = wrap.querySelector('.spinner'); if(s) s.remove();
    }
  }

  function listResults(){
    const elements = currentElements();
    const mode = el('formulaSelect').value;
    let k = parseInt(el('kInput').value, 10);
    if(Number.isNaN(k)) k = 0;

    let gen;
    const n = elements.length;
    if(mode==='comb'){ gen = combinationsNoRep(elements, k); }
    else if(mode==='comb_rep'){ gen = combinationsWithRep(elements, k); }
    else if(mode==='arr'){ gen = arrangementsNoRep(elements, k); }
    else if(mode==='arr_rep'){ gen = arrangementsRep(elements, k); }
    else if(mode==='perm'){ gen = arrangementsNoRep(elements, n); }
    else if(mode==='perm_rep'){ gen = multisetPermutations(elements); }

    state.list = [];
    let count = 0;
    setStatus('Gerando…', true);
    for(const item of gen){
      const text = Array.isArray(item)? ('{'+item.join(', ')+'}') : String(item);
      state.list.push(text);
      count++;
      if(count>=state.limit) { setStatus('Limite atingido'); break; }
    }
    setStatus('Pronto', false);
    state.page = 1;
    updateListUI();
    // Habilitar export
    const has = state.list.length>0;
    el('exportCSV').disabled=!has; el('exportTXT').disabled=!has; el('exportPDF').disabled=!has;
  }

  function updateListUI(){
    const cont = el('listContainer');
    const itemsBox = el('listItems');
    const pageInfo = el('pageInfo');
    const prev = el('prevPage');
    const next = el('nextPage');
    const pageSize = parseInt(el('pageSize').value,10) || state.pageSize; state.pageSize = pageSize;

    const total = state.list.length;
    if(total===0){ cont.style.display='none'; return; }
    cont.style.display='block';

    // aplicar filtro de busca local
    const q = (el('searchList').value || '').trim().toLowerCase();
    const filtered = q ? state.list.filter(it=>it.toLowerCase().includes(q)) : state.list;

    const pages = Math.max(1, Math.ceil(filtered.length / state.pageSize));
    if(state.page>pages) state.page=pages;
    const start = (state.page-1)*state.pageSize;
    const end = Math.min(filtered.length, start + state.pageSize);

    itemsBox.innerHTML='';
    for(let i=start;i<end;i++){
      const actualIndex = state.list.indexOf(filtered[i]); // para índice global
      const row = document.createElement('div'); row.className='list-item fade-enter';
      const idxSpan = document.createElement('div'); idxSpan.className='mono'; idxSpan.textContent = (actualIndex+1).toLocaleString('pt-BR');
      const itemSpan = document.createElement('div'); itemSpan.className='mono'; itemSpan.innerHTML = escapeHTML(filtered[i]);
      const actions = document.createElement('div'); actions.style.display='flex'; actions.style.justifyContent='flex-end';
      // copiar
      const copyWrap = document.createElement('div'); copyWrap.className='copy-tooltip';
      const copyBtn = document.createElement('div'); copyBtn.className='copy-btn'; copyBtn.title='Copiar';
      copyBtn.innerHTML = '📋';
      const tip = document.createElement('div'); tip.className='tip'; tip.textContent='Copiado!';
      copyWrap.appendChild(copyBtn); copyWrap.appendChild(tip);
      copyWrap.addEventListener('click', ()=>{
        navigator.clipboard?.writeText(filtered[i]).then(()=>{ copyWrap.classList.add('show'); setTimeout(()=>copyWrap.classList.remove('show'), 900); });
      });
      actions.appendChild(copyWrap);

      row.appendChild(idxSpan); row.appendChild(itemSpan); row.appendChild(actions);
      itemsBox.appendChild(row);
      // trigger animation
      setTimeout(()=> row.classList.add('fade-enter-active'), 20);
    }

    pageInfo.textContent = `página ${state.page}/${pages} • itens ${start+1}–${end} de ${filtered.length}`;
    prev.disabled = (state.page<=1);
    next.disabled = (state.page>=pages);
  }

  // Eventos Elementos & Fórmulas
  el('calcBtn').addEventListener('click', ()=>{
    const elements = currentElements();
    const mode = el('formulaSelect').value;
    const k = parseInt(el('kInput').value,10) || 0;
    const r = calcTotal(elements, k, mode);
    el('totalCount').textContent = r.total.toString();
    el('countFormula').textContent = r.formula;
    addHistory({type:'formula', inputs:{mode,k,elements}, total:r.total.toString(), desc:`${r.formula} = ${r.total}`});
    if(window.MathJax) MathJax.typesetPromise?.();
  });
  el('listBtn').addEventListener('click', listResults);
  el('sampleBtn').addEventListener('click', ()=>{
    // gera amostra de até 10 itens aleatórios (ou menos se lista vazia) sem criar lista completa
    const elements = currentElements();
    const mode = el('formulaSelect').value;
    let k = parseInt(el('kInput').value, 10); if(Number.isNaN(k)) k=0;
    let gen;
    if(mode==='comb'){ gen = combinationsNoRep(elements, k); }
    else if(mode==='comb_rep'){ gen = combinationsWithRep(elements, k); }
    else if(mode==='arr'){ gen = arrangementsNoRep(elements, k); }
    else if(mode==='arr_rep'){ gen = arrangementsRep(elements, k); }
    else if(mode==='perm'){ gen = arrangementsNoRep(elements, elements.length); }
    else if(mode==='perm_rep'){ gen = multisetPermutations(elements); }
    // coletar alguns elementos (amostra randômica: pegar os primeiros N gerados e randomizar)
    const maxSample = 10;
    const temp = [];
    for(const it of gen){
      temp.push(Array.isArray(it)?('{'+it.join(', ')+'}'):String(it));
      if(temp.length>=1000) break; // segurança
    }
    // random sample
    const sample = [];
    for(let i=0;i<Math.min(maxSample,temp.length);i++){
      const idx = Math.floor(Math.random()*temp.length);
      sample.push(temp.splice(idx,1)[0]);
    }
    state.list = sample;
    state.page = 1;
    updateListUI();
  });

  el('pageSize').addEventListener('change', updateListUI);
  el('prevPage').addEventListener('click', ()=>{ state.page=Math.max(1,state.page-1); updateListUI(); });
  el('nextPage').addEventListener('click', ()=>{ state.page=state.page+1; updateListUI(); });

  el('searchList').addEventListener('input', ()=>{
    state.page = 1;
    updateListUI();
  });

  // Exportações (Fórmulas)
  function download(filename, text){
    const blob = new Blob([text], {type:'text/plain;charset=utf-8'});
    const url = URL.createObjectURL(blob);
    const a = document.createElement('a');
    a.href=url; a.download=filename; a.click();
    setTimeout(()=>URL.revokeObjectURL(url), 1000);
  }
  el('exportCSV').addEventListener('click', ()=>{
    const rows = state.list.map((it,i)=>`${i+1},"${it.replace(/"/g,'""')}"`);
    download('resultados.csv', 'indice,item\n'+rows.join('\n'));
  });
  el('exportTXT').addEventListener('click', ()=>{
    const rows = state.list.map((it,i)=>`${i+1}\t${it}`);
    download('resultados.txt', rows.join('\n'));
  });
  el('exportPDF').addEventListener('click', async ()=>{
    const { jsPDF } = window.jspdf;
    const doc = new jsPDF();
    doc.setFont('helvetica','');
    doc.setFontSize(12);
    doc.text('Resultados', 14, 16);
    const body = state.list.map((it,i)=>[String(i+1), it]);
    // @ts-ignore
    doc.autoTable({ head:[['#','Item']], body, startY: 20, styles:{ font:'helvetica' } });
    doc.save('resultados.pdf');
  });

  // ======= PRINCÍPIO DA CONTAGEM (com Modo Manual e restrições por casa) =======
  function baseSet(){
    const t = el('pc-type').value;
    if(t==='digits') return Array.from({length:10},(_,i)=>String(i));
    if(t==='letters') return Array.from({length:26},(_,i)=>String.fromCharCode(65+i));
    if(t==='custom'){
      return (el('pc-custom').value || '')
        .split(/[,;\s]+/)
        .map(s=>s.trim())
        .filter(Boolean);
    }
    return [];
  }
  function parsePipeList(s){
    return (s||'').split('|').map(x=>x.trim()).filter(Boolean);
  }

  function renderHousesCards(){
    const container = el('pc-houses-container');
    container.innerHTML='';
    const n = Math.max(1, parseInt(el('pc-houses').value,10)||1);
    const mode = el('pc-mode').value;

    for(let i=0;i<n;i++){
      const card = document.createElement('div');
      card.className='house';
      card.dataset.index = String(i);
      const title = `<h4>Casa ${i+1}</h4>`;
      if(mode==='manual'){
        card.innerHTML = `
          ${title}
          <div class="block">
            <label>Valor desta casa (manual)</label>
            <input type="number" class="pc-manual" min="0" value="0">
          </div>
        `;
      } else {
        card.innerHTML = `
          ${title}
          <div class="row">
            <div class="block checks">
              <label><input type="checkbox" class="pc-check" value="par"> Par</label>
              <label><input type="checkbox" class="pc-check" value="impar"> Ímpar</label>
              <label><input type="checkbox" class="pc-check" value="vogal"> Vogal</label>
              <label><input type="checkbox" class="pc-check" value="consoante"> Consoante</label>
            </div>
            <div class="block">
              <label>Incluir apenas (|)</label>
              <input type="text" class="pc-include" placeholder="Ex.: A|B|C">
            </div>
            <div class="block">
              <label>Excluir (|)</label>
              <input type="text" class="pc-exclude" placeholder="Ex.: 0|O|I">
            </div>
          </div>
        `;
      }
      container.appendChild(card);
    }
  }

  function updateModeVisibility(){
    const auto = (el('pc-mode').value==='auto');
    document.querySelectorAll('.auto-field').forEach(n=>{ n.style.display = auto ? '' : 'none'; });
    renderHousesCards();
  }

  function applyRestrictions(base, checks, includeOnlyList, excludeList){
    let arr = base.slice();
    // Paridade
    if(checks.has('par') && !checks.has('impar')){
      arr = arr.filter(x => /^\d+$/.test(x) && (parseInt(x,10)%2===0));
    } else if(checks.has('impar') && !checks.has('par')){
      arr = arr.filter(x => /^\d+$/.test(x) && (parseInt(x,10)%2===1));
    }
    // Letras
    const isLetter = s => /^[A-Za-z]$/.test(s);
    if(checks.has('vogal') && !checks.has('consoante')){
      arr = arr.filter(x => isLetter(x) && 'AEIOUaeiou'.includes(x));
    } else if(checks.has('consoante') && !checks.has('vogal')){
      arr = arr.filter(x => isLetter(x) && !'AEIOUaeiou'.includes(x));
    }
    // Incluir apenas
    if(includeOnlyList.length){
      const set = new Set(includeOnlyList);
      arr = arr.filter(x => set.has(x));
    }
    // Excluir
    if(excludeList.length){
      const set = new Set(excludeList);
      arr = arr.filter(x => !set.has(x));
    }
    return arr;
  }

  function collectAllowedPerHouse(){
    const base = baseSet();
    const globalEx = parsePipeList(el('pc-exclude').value);
    const baseGlobal = globalEx.length ? base.filter(x=>!new Set(globalEx).has(x)) : base;
    const firstNotZero = el('pc-first-not-zero').checked;
    const cards = [...document.querySelectorAll('#pc-houses-container .house')];
    const result = cards.map((card, idx)=>{
      const checks = new Set([...card.querySelectorAll('.pc-check:checked')].map(c=>c.value));
      const includeOnly = parsePipeList(card.querySelector('.pc-include')?.value || '');
      const exclude = parsePipeList(card.querySelector('.pc-exclude')?.value || '');
      let allowed = applyRestrictions(baseGlobal, checks, includeOnly, exclude);
      if(idx===0 && firstNotZero){
        allowed = allowed.filter(x => x!=='0');
      }
      return allowed;
    });
    return result;
  }

  function countNoRep(allowedPerHouse, guardLimit=5_000_000){
    let count = 0;
    const used = new Set();
    let steps = 0;
    function bt(i){
      if(steps>guardLimit) return false;
      if(i===allowedPerHouse.length){ count++; return true; }
      const arr = allowedPerHouse[i];
      for(const x of arr){
        if(!used.has(x)){
          used.add(x);
          steps++;
          const ok = bt(i+1);
          used.delete(x);
          if(steps>guardLimit) return false;
        }
      }
      return true;
    }
    const finished = bt(0);
    return {count, finished, steps};
  }

  function* pcGenWithRep(allowedPerHouse, prefix=[]){
    const i = prefix.length;
    if(i===allowedPerHouse.length){ yield prefix.slice(); return; }
    for(const x of allowedPerHouse[i]){
      prefix.push(x);
      yield* pcGenWithRep(allowedPerHouse, prefix);
      prefix.pop();
    }
  }
  function* pcGenNoRep(allowedPerHouse, prefix=[], used=new Set()){
    const i = prefix.length;
    if(i===allowedPerHouse.length){ yield prefix.slice(); return; }
    for(const x of allowedPerHouse[i]){
      if(!used.has(x)){
        used.add(x);
        prefix.push(x);
        yield* pcGenNoRep(allowedPerHouse, prefix, used);
        prefix.pop();
        used.delete(x);
      }
    }
  }

  function pcCalculate(){
    const mode = el('pc-mode').value;
    const houses = Math.max(1, parseInt(el('pc-houses').value,10)||1);
    let total = 0;
    let factors = [];
    let desc = '';
    if(mode==='manual'){
      const vals = [...document.querySelectorAll('#pc-houses-container .pc-manual')].map(inp=>Math.max(0, parseInt(inp.value,10) || 0));
      total = vals.reduce((a,b)=>a*b,1);
      factors = vals;
      desc = `Manual: ${vals.join(' × ')} = ${total}`;
    } else {
      const noRep = el('pc-norep').checked;
      const perHouse = collectAllowedPerHouse();
      factors = perHouse.map(a=>a.length);
      if(!noRep){
        total = factors.reduce((a,b)=>a*b,1);
        desc = `Automático (com repetição): ${factors.join(' × ')} = ${total}`;
      } else {
        const {count, finished, steps} = countNoRep(perHouse);
        total = count;
        desc = finished ? `Automático (sem repetição): total = ${total}` : `Automático (sem repetição): parcial (limite) = ${total}`;
      }
    }
    el('pc-total').textContent = String(total);
    el('pc-mult').textContent = (factors.length? `Fatores: ${factors.join(' × ')}` : '');
    state.totalPC = total;
    addHistory({type:'pc', inputs: snapshotPCInputs(), total:String(total), desc});
  }

  function pcList(){
    const mode = el('pc-mode').value;
    state.listPC = [];
    state.pagePC = 1;
    let gen;
    if(mode==='manual'){
      el('pc-listContainer').style.display='none';
      alert('No modo MANUAL a listagem de sequências não é definida (não há alfabeto por casa). Use o modo Automático para listar.');
      return;
    } else {
      const perHouse = collectAllowedPerHouse();
      const noRep = el('pc-norep').checked;
      gen = noRep ? pcGenNoRep(perHouse) : pcGenWithRep(perHouse);
    }
    let c=0;
    setStatus('Gerando…', true);
    for(const seq of gen){
      const text = seq.join('');
      state.listPC.push(text);
      c++;
      if(c>=state.limit) break;
    }
    setStatus('Pronto', false);
    updatePCListUI();
    const has = state.listPC.length>0;
    el('pc-exportCSV').disabled=!has; el('pc-exportTXT').disabled=!has; el('pc-exportPDF').disabled=!has;
  }

  function updatePCListUI(){
    const cont = el('pc-listContainer');
    const itemsBox = el('pc-listItems');
    const pageInfo = el('pc-page');
    const prev = el('pc-prev');
    const next = el('pc-next');

    const total = state.listPC.length;
    if(total===0){ cont.style.display='none'; return; }
    cont.style.display='block';

    const pages = Math.max(1, Math.ceil(total / state.pageSize));
    if(state.pagePC>pages) state.pagePC=pages;
    const start = (state.pagePC-1)*state.pageSize;
    const end = Math.min(total, start + state.pageSize);

    itemsBox.innerHTML='';
    for(let i=start;i<end;i++){
      const row = document.createElement('div'); row.className='list-item fade-enter';
      row.innerHTML = `<div class="mono">${(i+1).toLocaleString('pt-BR')}</div><div class="mono">${escapeHTML(state.listPC[i])}</div><div style="display:flex;justify-content:flex-end"><div class="copy-btn" title="Copiar">${'📋'}</div></div>`;
      const copy = row.querySelector('.copy-btn');
      copy.addEventListener('click', ()=>{ navigator.clipboard?.writeText(state.listPC[i]).then(()=>{ copy.textContent='✔'; setTimeout(()=>copy.textContent='📋',800); }); });
      itemsBox.appendChild(row);
      setTimeout(()=> row.classList.add('fade-enter-active'), 20);
    }
    pageInfo.textContent = `página ${state.pagePC}/${pages} • itens ${start+1}–${end} de ${total}`;
    prev.disabled = (state.pagePC<=1);
    next.disabled = (state.pagePC>=pages);
  }

  // Exportações (PC)
  el('pc-exportCSV').addEventListener('click', ()=>{
    const rows = state.listPC.map((it,i)=>`${i+1},"${it.replace(/"/g,'""')}"`);
    download('pc_resultados.csv', 'indice,item\n'+rows.join('\n'));
  });
  el('pc-exportTXT').addEventListener('click', ()=>{
    const rows = state.listPC.map((it,i)=>`${i+1}\t${it}`);
    download('pc_resultados.txt', rows.join('\n'));
  });
  el('pc-exportPDF').addEventListener('click', async ()=>{
    const { jsPDF } = window.jspdf;
    const doc = new jsPDF();
    doc.setFont('helvetica','');
    doc.setFontSize(12);
    doc.text('Princípio da Contagem - Resultados', 14, 16);
    const body = state.listPC.map((it,i)=>[String(i+1), it]);
    // @ts-ignore
    doc.autoTable({ head:[['#','Item']], body, startY: 20, styles:{ font:'helvetica' } });
    doc.save('pc_resultados.pdf');
  });

  function snapshotPCInputs(){
    const mode = el('pc-mode').value;
    const houses = parseInt(el('pc-houses').value,10)||1;
    const base = (mode==='auto') ? el('pc-type').value : null;
    const obj = {mode,houses,base};
    if(mode==='auto'){
      obj.norep = el('pc-norep').checked;
      obj.firstNotZero = el('pc-first-not-zero').checked;
      obj.custom = el('pc-custom').value;
      obj.globalExclude = el('pc-exclude').value;
      obj.perHouse = [...document.querySelectorAll('#pc-houses-container .house')].map(card=>{
        return {
          checks: [...card.querySelectorAll('.pc-check:checked')].map(x=>x.value),
          include: card.querySelector('.pc-include')?.value || '',
          exclude: card.querySelector('.pc-exclude')?.value || ''
        };
      });
    } else {
      obj.manualVals = [...document.querySelectorAll('#pc-houses-container .pc-manual')].map(i=>i.value);
    }
    return obj;
  }

  // Eventos PC
  el('pc-mode').addEventListener('change', updateModeVisibility);
  el('pc-houses').addEventListener('input', renderHousesCards);
  ['pc-type','pc-custom','pc-norep','pc-first-not-zero'].forEach(id=>{
    const n = el(id); if(n) n.addEventListener('input', ()=>{}); // ganchos
  });

  el('pc-calc').addEventListener('click', pcCalculate);
  el('pc-list').addEventListener('click', pcList);
  el('pc-prev').addEventListener('click', ()=>{ state.pagePC=Math.max(1,state.pagePC-1); updatePCListUI(); });
  el('pc-next').addEventListener('click', ()=>{ state.pagePC=state.pagePC+1; updatePCListUI(); });

  updateModeVisibility();

  // ======= Histórico =======
  function addHistory(entry){
    const ts = new Date().toISOString();
    const e = {...entry, ts};
    if(state.autosave){
      state.history.unshift(e);
      state.history = state.history.slice(0,200);
      localStorage.setItem('comb.history', JSON.stringify(state.history));
    }
    renderHistory();
  }

  function renderHistory(){
    el('hist-autosave').checked = state.autosave;
    const box = el('historyList');
    if(!box) return;
    box.innerHTML='';
    if(state.history.length===0){
      box.innerHTML = '<div class="help">Vazio</div>';
      return;
    }
    const frag = document.createDocumentFragment();
    state.history.forEach((h,i)=>{
      const div = document.createElement('div');
      div.className='mono';
      const short = h.type==='formula' ? h.desc : (h.desc || 'PC');
      div.textContent = `${new Date(h.ts).toLocaleString()} — ${short}`;
      frag.appendChild(div);
    });
    box.appendChild(frag);
  }
  renderHistory();

  el('hist-autosave').addEventListener('change', (e)=>{
    state.autosave = e.target.checked;
    localStorage.setItem('comb.autosave', state.autosave? 'true':'false');
  });
  el('clearHistory').addEventListener('click', ()=>{
    if(confirm('Limpar histórico salvo no navegador?')){
      state.history = [];
      localStorage.removeItem('comb.history');
      renderHistory();
    }
  });
  el('exportHistory').addEventListener('click', (ev)=>{
    const data = JSON.stringify(state.history, null, 2);
    const blob = new Blob([data], {type:'application/json'});
    const url = URL.createObjectURL(blob);
    ev.target.href = url;
    ev.target.download = 'historico_combinatoria.json';
    setTimeout(()=>URL.revokeObjectURL(url), 1000);
  });

  // ======= Presets (salvar/carregar) =======
  el('savePreset').addEventListener('click', ()=>{
    const name = prompt('Nome do preset (ex.: "comb-joao")');
    if(!name) return;
    const preset = {
      name,
      elements: el('elementsInput').value,
      formula: el('formulaSelect').value,
      k: el('kInput').value,
      pageSize: el('pageSize').value,
      modePC: el('pc-mode').value
    };
    state.presets.push(preset);
    localStorage.setItem('comb.presets', JSON.stringify(state.presets));
    renderPresets();
    alert('Preset salvo!');
  });
  el('presetSelect').addEventListener('change', (e)=>{
    const v = e.target.value;
    if(v==='') return;
    const p = state.presets[parseInt(v,10)];
    if(!p) return;
    el('elementsInput').value = p.elements||'';
    el('formulaSelect').value = p.formula||'comb';
    el('kInput').value = p.k||'2';
    el('pageSize').value = p.pageSize||'200';
    if(p.modePC) el('pc-mode').value = p.modePC;
    updateDynamicText(); renderElementsPreview(parseElements(el('elementsInput').value));
    updateModeVisibility();
    alert('Preset carregado');
  });

  // inicializações finais
  renderPresets();
  renderElementsPreview(parseElements(el('elementsInput').value || ''));
})();
</script>
</body>
</html>
