# Sistema Faccao - codigo completo

Salve este conteudo como `index.html` para rodar no navegador.

```html
<!doctype html>
<html lang="pt-BR">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>FACÇÃO NEXUS</title>
  <style>
    :root {
      --bg:#f0f4f8;--sidebar:#0f172a;--sidebar-hover:#1e293b;--card:#fff;
      --text:#0f172a;--muted:#64748b;--line:#e2e8f0;
      --blue:#64748b;--blue2:#475569;--green:#4b5563;--amber:#6b7280;--red:#991b1b;
      --field:#f8fafc;--sh:0 1px 3px rgba(0,0,0,.10),0 1px 2px -1px rgba(0,0,0,.08);
      --sh-lg:0 10px 25px -5px rgba(0,0,0,.2),0 4px 10px -6px rgba(0,0,0,.1);
      --r:18px;--pill-r:999px;--font:'Segoe UI',system-ui,sans-serif;
    }
    body.dark {
      --bg:#0d1117;--sidebar:#010409;--sidebar-hover:#161b22;--card:#161b22;
      --text:#f4f4f5;--muted:#a1a1aa;--line:#30363d;--field:#111827;--blue:#9ca3af;--blue2:#6b7280;
    }
    *{box-sizing:border-box;margin:0;padding:0}
    body{font-family:var(--font);background:var(--bg);color:var(--text);font-size:14px;line-height:1.5}
    button,input,select,textarea{-webkit-tap-highlight-color:transparent}
    button:focus-visible,input:focus-visible,select:focus-visible,textarea:focus-visible{outline:2px solid var(--blue);outline-offset:2px}

    /* ── LAYOUT ── */
    .shell{display:flex;min-height:100vh}
    aside{width:72px;background:#060910;display:flex;flex-direction:column;flex-shrink:0;position:sticky;top:0;height:100vh;overflow-y:auto;transition:width .18s ease;z-index:20;box-shadow:8px 0 30px rgba(0,0,0,.12)}
    aside:hover{width:236px}
    .brand{padding:18px 14px 14px;border-bottom:1px solid rgba(255,255,255,.07);display:flex;align-items:center;gap:10px}
    .brand-ico{width:38px;height:38px;background:#181f2a;border:1px solid rgba(255,255,255,.08);border-radius:14px;display:flex;align-items:center;justify-content:center;font-size:17px;flex-shrink:0;box-shadow:0 10px 25px rgba(0,0,0,.22)}
    .brand h1{font-size:14px;font-weight:700;color:#fff;letter-spacing:.2px}
    .brand small{font-size:11px;color:#64748b;display:block}
    .brand-text,.nav-label,.footer-label{opacity:0;width:0;overflow:hidden;white-space:nowrap;transition:opacity .12s ease,width .12s ease}
    aside:hover .brand-text,aside:hover .nav-label,aside:hover .footer-label{opacity:1;width:auto}
    nav{padding:10px 8px;flex:1}
    nav button{
      width:100%;display:flex;align-items:center;gap:10px;padding:10px 11px;
      border:0;background:transparent;color:#94a3b8;border-radius:15px;cursor:pointer;
      font:500 13px var(--font);text-align:left;transition:all .15s;margin-bottom:2px;justify-content:center;
    }
    aside:hover nav button{justify-content:flex-start}
    nav button:hover{background:rgba(255,255,255,.07);color:#e2e8f0;transform:translateX(2px)}
    nav button.active{background:rgba(148,163,184,.14);color:#f8fafc;font-weight:700;box-shadow:inset 0 0 0 1px rgba(148,163,184,.16)}
    nav button .ico{font-size:15px;flex-shrink:0;width:20px;text-align:center}
    .sidebar-footer{padding:10px 8px;border-top:1px solid rgba(255,255,255,.07)}
    .sidebar-footer button{
      width:100%;display:flex;align-items:center;gap:8px;padding:8px 10px;
      border:0;border-radius:14px;cursor:pointer;font:500 12px var(--font);
      transition:all .15s;margin-bottom:4px;justify-content:center;
    }
    aside:hover .sidebar-footer button{justify-content:flex-start}
    .btn-theme{background:rgba(255,255,255,.05);color:#94a3b8}
    .btn-theme:hover{color:#e2e8f0;background:rgba(255,255,255,.1)}
    .btn-wipe{background:rgba(220,38,38,.12);color:#fca5a5}
    .btn-wipe:hover{background:rgba(220,38,38,.22)}

    /* ── MAIN ── */
    .content{flex:1;overflow:hidden;display:flex;flex-direction:column}
    main{padding:24px;flex:1;overflow:auto}

    /* ── PAGE HEADER ── */
    .top{display:flex;justify-content:space-between;align-items:flex-start;gap:14px;margin-bottom:20px;flex-wrap:wrap}
    .page-info h2{font-size:clamp(22px,2vw,30px);font-weight:800;letter-spacing:-.3px}
    .page-info p{color:var(--muted);font-size:13px;margin-top:2px}
    .actions{display:flex;gap:8px;flex-wrap:wrap;align-items:center}

    /* ── BUTTONS ── */
    .btn{
      display:inline-flex;align-items:center;justify-content:center;gap:7px;padding:10px 16px;
      border-radius:var(--pill-r);font:700 13px var(--font);cursor:pointer;transition:transform .15s,box-shadow .15s,background .15s,border-color .15s;
      border:1.5px solid transparent;white-space:nowrap;min-height:42px;
    }
    .btn.primary{background:#e5e7eb;color:#111827;border-color:#e5e7eb;box-shadow:0 10px 22px rgba(0,0,0,.12)}
    body.dark .btn.primary{background:#f8fafc;color:#111827;border-color:#f8fafc}
    .btn.primary:hover{background:#f8fafc;transform:translateY(-1px)}
    .btn.secondary{background:var(--card);color:var(--text);border-color:var(--line);box-shadow:var(--sh)}
    .btn.secondary:hover{border-color:var(--blue);transform:translateY(-1px)}
    .btn.danger{background:#fee2e2;color:#991b1b;border-color:#fca5a5}
    .btn.danger:hover{background:#fecaca;transform:translateY(-1px)}
    .btn.mini{padding:6px 10px;font-size:12px;min-height:34px}
    .row-actions{display:flex;gap:7px;align-items:center;white-space:nowrap}
    .inline-filter{display:flex;align-items:center;gap:8px;flex-wrap:wrap;margin-bottom:12px}
    .inline-filter select{width:auto;margin-top:0;min-width:170px}

    /* ── KPI CARDS ── */
    .kpi-grid{display:grid;grid-template-columns:repeat(4,1fr);gap:13px;margin-bottom:20px}
    .kpi{
      background:var(--card);border-radius:var(--r);padding:18px;
      box-shadow:var(--sh);border:1px solid var(--line);position:relative;overflow:hidden;
      transition:transform .16s,box-shadow .16s,border-color .16s;
    }
    .kpi:hover{transform:translateY(-2px);box-shadow:var(--sh-lg);border-color:rgba(148,163,184,.35)}
    .kpi::before{content:'';position:absolute;top:0;left:0;right:0;height:3px;background:#3f4652}
    .kpi-label{font-size:11px;font-weight:700;color:var(--muted);text-transform:uppercase;letter-spacing:.5px}
    .kpi-val{font-size:24px;font-weight:700;margin-top:6px;letter-spacing:-.5px}
    .kpi-ico{position:absolute;right:14px;top:12px;font-size:20px;opacity:.25}
    .kpi-sub{font-size:11px;margin-top:4px;font-weight:600}

    /* ── TABLE ── */
    .panel{background:var(--card);border-radius:var(--r);border:1px solid var(--line);box-shadow:var(--sh);overflow:hidden;margin-bottom:14px}
    .panel-toolbar{padding:11px 16px;border-bottom:1px solid var(--line);display:flex;align-items:center;gap:10px}
    .search-box{
      padding:10px 14px;border:1.5px solid var(--line);border-radius:var(--pill-r);
      font:13px var(--font);background:var(--field);color:var(--text);
      width:min(280px,100%);transition:border .15s,box-shadow .15s;
    }
    .search-box:focus{outline:none;border-color:var(--blue);box-shadow:0 0 0 4px rgba(148,163,184,.14)}
    .rec-count{font-size:12px;color:var(--muted);margin-left:auto}
    .table-wrap{overflow-x:auto}
    table{border-collapse:collapse;width:100%;min-width:580px}
    thead tr{background:#f8fafc}
    body.dark thead tr{background:#0d1117}
    th{padding:9px 13px;text-align:left;font-size:11px;font-weight:700;color:var(--muted);text-transform:uppercase;letter-spacing:.5px;border-bottom:1px solid var(--line);white-space:nowrap}
    td{padding:10px 13px;border-bottom:1px solid var(--line);font-size:13px;vertical-align:middle}
    tbody tr:last-child td{border-bottom:0}
    tbody tr:hover{background:#f8fafc}
    body.dark tbody tr:hover{background:rgba(255,255,255,.035)}
    .empty-state{padding:44px 24px;text-align:center;color:var(--muted)}
    .empty-state .ei{font-size:34px;margin-bottom:10px;opacity:.4}

    /* ── PILLS ── */
    .pill{display:inline-flex;align-items:center;gap:5px;padding:5px 10px;border-radius:var(--pill-r);font-size:11px;font-weight:800;white-space:nowrap}
    .pill::before{content:'●';font-size:7px}
    .pg{background:#d1fae5;color:#065f46} body.dark .pg{background:#064e3b;color:#6ee7b7}
    .pa{background:#fef3c7;color:#92400e} body.dark .pa{background:#78350f;color:#fcd34d}
    .pr{background:#fee2e2;color:#991b1b} body.dark .pr{background:#7f1d1d;color:#fca5a5}
    .pb{background:#dbeafe;color:#1e40af} body.dark .pb{background:#1e3a5f;color:#93c5fd}
    .pz{background:#f1f5f9;color:#475569} body.dark .pz{background:#1e293b;color:#94a3b8}

    /* ── MODAL / FORM ── */
    dialog{border:0;border-radius:22px 0 0 22px;box-shadow:var(--sh-lg);width:min(480px,94vw);background:var(--card);color:var(--text);padding:0;height:100vh;max-height:100vh;overflow-y:auto;position:fixed;inset:0 0 auto auto;transform:none;margin:0}
    dialog::backdrop{background:rgba(0,0,0,.68);backdrop-filter:blur(3px)}
    .modal-head{padding:22px 24px 17px;border-bottom:1px solid var(--line);position:sticky;top:0;background:var(--card);z-index:2}
    .modal-head h2{font-size:19px;font-weight:800}
    .modal-body{padding:20px 24px 24px}
    form{display:grid;grid-template-columns:1fr;gap:13px}
    label{font-size:12px;font-weight:700;color:var(--muted);text-transform:uppercase;letter-spacing:.4px;display:block}
    input,select,textarea{
      display:block;width:100%;margin-top:6px;padding:11px 13px;
      border:1.5px solid var(--line);border-radius:14px;font:14px var(--font);
      background:var(--field);color:var(--text);transition:border .15s,box-shadow .15s,background .15s;
    }
    input:focus,select:focus,textarea:focus{outline:none;border-color:var(--blue);box-shadow:0 0 0 4px rgba(148,163,184,.14)}
    textarea{min-height:92px;resize:vertical}
    .full{grid-column:1/-1}
    .form-actions{grid-column:1/-1;display:flex;justify-content:flex-end;gap:8px;margin-top:4px;border-top:1px solid var(--line);padding-top:14px}

    /* ── CADASTROS ── */
    .cadastro-home{display:grid;grid-template-columns:repeat(2,minmax(0,1fr));gap:14px}
    .cadastro-card{
      background:var(--card);border:1px solid var(--line);border-radius:22px;
      box-shadow:var(--sh);padding:18px;display:grid;gap:14px;min-height:178px;
      position:relative;overflow:hidden;transition:transform .16s,box-shadow .16s,border-color .16s;
    }
    .cadastro-card:hover{transform:translateY(-3px);box-shadow:var(--sh-lg);border-color:rgba(148,163,184,.35)}
    .cadastro-card::before{content:'';position:absolute;top:0;left:0;right:0;height:3px;background:#3f4652}
    .cadastro-card h3{font-size:17px;letter-spacing:-.2px}
    .cadastro-card p{color:var(--muted);font-size:13px}
    .cadastro-meta{display:flex;gap:10px;flex-wrap:wrap;color:var(--muted);font-size:12px;font-weight:700}
    .cadastro-meta strong{color:var(--text);font-size:18px}
    .cadastro-list-head{display:flex;align-items:center;gap:10px;margin-bottom:12px;flex-wrap:wrap}
    .cadastro-tabs{display:flex;gap:7px;flex-wrap:wrap}
    .cadastro-tabs button{background:var(--card);border:1px solid var(--line);color:var(--text);border-radius:var(--pill-r);cursor:pointer;font:800 12px var(--font);padding:8px 12px;min-height:38px;transition:transform .15s,border-color .15s}
    .cadastro-tabs button:hover{transform:translateY(-1px);border-color:var(--blue)}
    .cadastro-tabs button.active{background:rgba(148,163,184,.14);border-color:var(--blue);color:var(--text)}
    .status-filter{display:flex;align-items:center;gap:8px;margin-left:auto}
    .status-filter select{width:auto;margin:0;min-width:130px}
    .muted{color:var(--muted)}

    /* ── PRODUÇÃO ── */
    .prod-grid{display:grid;grid-template-columns:minmax(0,1fr) 390px;gap:14px;align-items:start}
    .prod-grid.single{grid-template-columns:1fr}
    .prod-detail{background:var(--card);border:1px solid var(--line);border-radius:22px;box-shadow:var(--sh);overflow:hidden;position:sticky;top:18px}
    .prod-detail-head{border-bottom:1px solid var(--line);padding:16px 18px}
    .prod-detail-head h3{font-size:18px;margin-bottom:3px}
    .prod-detail-body{display:grid;gap:14px;padding:16px 18px}
    .prod-stats{display:grid;grid-template-columns:repeat(2,minmax(0,1fr));gap:10px}
    .prod-stat{background:var(--field);border:1px solid var(--line);border-radius:16px;padding:11px}
    .prod-stat span{color:var(--muted);display:block;font-size:11px;font-weight:800;text-transform:uppercase}
    .prod-stat strong{display:block;font-size:18px;margin-top:3px}
    .timeline{display:grid;gap:8px}
    .timeline button{align-items:center;background:transparent;border:1px solid var(--line);border-radius:16px;color:var(--text);cursor:pointer;display:flex;font:800 12px var(--font);gap:10px;justify-content:space-between;min-height:40px;padding:8px 11px;text-align:left}
    .timeline button.done{background:rgba(148,163,184,.12);border-color:var(--blue)}
    .timeline-dot{background:#71717a;border-radius:999px;display:inline-block;height:9px;width:9px}
    .timeline button.done .timeline-dot{background:#e5e7eb}
    .prod-actions{display:flex;flex-wrap:wrap;gap:8px}
    .prod-actions .btn{flex:1 1 auto}
    .prod-log{display:grid;gap:8px}
    .prod-log-row{align-items:center;border:1px solid var(--line);border-radius:14px;display:flex;gap:10px;justify-content:space-between;padding:9px 10px}
    .prod-log-row small{color:var(--muted)}
    .op-link{background:transparent;border:0;color:var(--text);cursor:pointer;font:800 13px var(--font);padding:0;text-align:left}
    .op-link:hover{text-decoration:underline}
    .grade-table{border:1px solid var(--line);border-radius:16px;overflow:auto}
    .grade-table table{min-width:0}
    .grade-table th,.grade-table td{text-align:center}
    .grade-table th:first-child,.grade-table td:first-child{text-align:left}

    /* ── TOAST ── */
    #toasts{position:fixed;bottom:20px;right:20px;z-index:9999;display:flex;flex-direction:column;gap:7px;pointer-events:none}
    .toast{
      background:#1e293b;color:#f1f5f9;padding:11px 15px;border-radius:8px;
      font-size:13px;font-weight:500;box-shadow:var(--sh-lg);display:flex;
      align-items:center;gap:9px;animation:tin .22s ease;pointer-events:auto;max-width:320px;
    }
    .toast.ok{border-left:3px solid var(--green)}
    .toast.er{border-left:3px solid var(--red)}
    .toast.nf{border-left:3px solid var(--blue)}
    @keyframes tin{from{opacity:0;transform:translateX(16px)}to{opacity:1;transform:translateX(0)}}

    /* ── BADGE (nav pendência) ── */
    .badge{background:var(--red);color:#fff;border-radius:99px;font-size:10px;font-weight:700;padding:1px 6px;margin-left:auto}

    /* ── RESPONSIVE ── */
    @media(max-width:860px){
      .shell{flex-direction:column}
      aside{width:100%;height:72px;position:fixed;left:0;right:0;bottom:0;top:auto;z-index:30;border-top:1px solid rgba(255,255,255,.09);box-shadow:0 -12px 34px rgba(0,0,0,.28)}
      aside:hover{width:100%}
      .brand-text,.nav-label,.footer-label{opacity:1;width:auto}
      .brand,.sidebar-footer{display:none}
      nav{display:flex;flex-direction:row;overflow-x:auto;padding:8px;gap:6px;scroll-snap-type:x mandatory}
      nav::-webkit-scrollbar{display:none}
      nav button{white-space:nowrap;flex:0 0 auto;width:82px;min-height:56px;padding:7px 8px;flex-direction:column;gap:3px;scroll-snap-align:center;border-radius:18px;font-size:11px}
      nav button:hover{transform:none}
      nav button .ico{width:auto;font-size:18px}
      main{padding:18px 14px 96px}
      .kpi-grid{grid-template-columns:1fr 1fr}
      .cadastro-home{grid-template-columns:1fr}
      .prod-grid{grid-template-columns:1fr}
      .prod-detail{position:static}
      .cadastro-list-head{align-items:stretch;flex-direction:column}
      .status-filter{margin-left:0}
      .status-filter select{width:100%}
      .panel-toolbar{align-items:stretch;flex-direction:column}
      .search-box{width:100%}
      .rec-count{margin-left:0}
      dialog{border-radius:22px 22px 0 0;width:100vw;height:88vh;max-height:88vh;inset:auto 0 0 0}
      form{grid-template-columns:1fr}
      .top{flex-direction:column}
      .actions,.btn{width:100%}
    }
    @media(max-width:620px){
      .kpi-grid{grid-template-columns:1fr}
      .page-info h2{font-size:24px}
      table{min-width:0}
      thead{display:none}
      tbody,tr,td{display:block;width:100%}
      tbody tr{border:1px solid var(--line);border-radius:18px;margin:10px 0;padding:8px 10px;background:var(--card)}
      td{border-bottom:1px solid var(--line);display:flex;justify-content:space-between;gap:14px;padding:9px 2px;text-align:right}
      td:last-child{border-bottom:0}
      td::before{content:attr(data-label);color:var(--muted);font-size:11px;font-weight:800;text-transform:uppercase;text-align:left}
      .row-actions{justify-content:flex-end;white-space:normal}
      .row-actions .btn{width:auto}
      .prod-stats{grid-template-columns:1fr}
      .prod-actions .btn{width:100%}
      .form-actions{flex-direction:column-reverse}
      #toasts{left:12px;right:12px;bottom:86px}
      .toast{max-width:none}
    }
    @media print{
      aside,.actions,.panel-toolbar,.row-actions,dialog,#toasts{display:none!important}
      .shell,.content,main{display:block;height:auto;overflow:visible;padding:0}
      body{background:#fff;color:#111}
      .panel,.kpi{box-shadow:none;border:1px solid #ddd}
      table{min-width:0;font-size:11px}
    }
  </style>
</head>
<body>
<div class="shell">
  <aside>
    <div class="brand">
      <div class="brand-ico">⚙️</div>
      <div class="brand-text"><h1>FACÇÃO NEXUS</h1><small>Gestão industrial</small></div>
    </div>
    <nav id="menu"></nav>
    <div class="sidebar-footer">
      <button class="btn-theme" onclick="alternarTema()"><span>🌙</span><span class="footer-label">Tema</span></button>
      <button class="btn-wipe" onclick="resetar()"><span>🗑️</span><span class="footer-label">Limpar</span></button>
    </div>
  </aside>
  <div class="content"><main id="app"></main></div>
</div>
<dialog id="modal"><div id="modalContent"></div></dialog>
<div id="toasts"></div>

<script>
/* ========== DADOS ========== */
const KEY="sistema_faccao_dados_v1", THEME="sistema_faccao_tema";
const padrao={clientes:[],modelos:[],colaboradores:[],terceiros:[],ops:[],entregas:[],quebras:[],insumosOp:[],estoque:[],movEstoque:[],contas:[],despesas:[],baixas:[],valesClientes:[],valesColaboradores:[],pagamentosColaboradores:[],enviosTerceiros:[],retornosTerceiros:[],pagamentosTerceiros:[],acertos:[],historico:[]};
let db=JSON.parse(localStorage.getItem(KEY)||JSON.stringify(padrao));
let tela="Dashboard";
let filtroStatusOperacao="Todos";
let opSelecionada=null;
let cadastroAtual="";
let filtroStatusCadastro="Todos";
let tema=localStorage.getItem(THEME)||"dark";
const telas=["Dashboard","Cadastros","Produção","Insumos","Estoque","Financeiro","Colaboradores","Terceiros","Acertos","Histórico"];
const ICONS={"Dashboard":"📊","Cadastros":"👥","Produção":"🔧","Insumos":"📦","Estoque":"🗃️","Financeiro":"💳","Colaboradores":"👤","Terceiros":"🔄","Acertos":"✅","Histórico":"📋"};

/* ========== UTILITÁRIOS ========== */
function aplicarTema(){document.body.classList.toggle("dark",tema==="dark");localStorage.setItem(THEME,tema)}
function alternarTema(){tema=tema==="dark"?"light":"dark";aplicarTema()}
function salvar(){localStorage.setItem(KEY,JSON.stringify(db))}
function id(lista){return(db[lista].reduce((m,x)=>Math.max(m,x.id),0)||0)+1}
function hoje(){return new Date().toISOString().slice(0,10)}
function dinheiro(v){return Number(v||0).toLocaleString("pt-BR",{style:"currency",currency:"BRL"})}
function numero(v){return Number(String(v==null?"":v).replace(",",".")||0)||0}
function moeda(v){
  const s=String(v==null?"":v).trim();
  if(!s)return 0;
  const limpo=s.includes(",")?s.replace(/\./g,"").replace(",","."):s;
  return Number(limpo)||0;
}
function nome(lista,itemId){return(db[lista].find(x=>x.id==itemId)||{}).nome||"—"}
function opCodigo(opId){return(db.ops.find(x=>x.id==opId)||{}).codigo||"—"}

function toast(msg,tipo="ok"){
  const el=document.createElement("div");
  el.className=`toast ${tipo}`;
  const ic={ok:"✓",er:"✗",nf:"ℹ"};
  el.innerHTML=`<span>${ic[tipo]||"ℹ"}</span><span>${msg}</span>`;
  toasts.appendChild(el);
  setTimeout(()=>el.remove(),3200);
}

function hist(modulo,acao,obs=""){
  db.historico.unshift({id:id("historico"),data:new Date().toLocaleString("pt-BR"),modulo,acao,obs});
  salvar();
  toast(`${acao}${obs?" · "+obs:""}`, "ok");
}

function resetar(){
  if(confirm("⚠ Apagar TODOS os dados salvos neste navegador? Esta ação não pode ser desfeita.")){
    localStorage.removeItem(KEY);db=JSON.parse(JSON.stringify(padrao));
    toast("Dados apagados.","nf");render();
  }
}

/* ========== CÁLCULOS ========== */
function totaisOp(opId){
  const op=db.ops.find(x=>x.id==opId);
  if(!op)return{entregue:0,quebra:0,saldo:0,valor:0};
  const entregue=db.entregas.filter(x=>x.opId==opId).reduce((s,x)=>s+Number(x.qtd||0),0);
  const quebra=db.quebras.filter(x=>x.opId==opId).reduce((s,x)=>s+Number(x.qtd||0),0);
  const saldo=Number(op.recebido||0)-entregue-quebra;
  const valor=entregue*Number(op.valorPeca||0);
  return{entregue,quebra,saldo,valor};
}
function atualizarConta(opId){
  const total=totaisOp(opId).valor;
  let c=db.contas.find(x=>x.opId==opId);
  if(!c){c={id:id("contas"),opId,valor:total,status:"Não pago"};db.contas.push(c)}
  else c.valor=total;
  atualizarStatusConta(c.id);
}
function saldoConta(cid){
  const c=db.contas.find(x=>x.id==cid);
  const pago=db.baixas.filter(x=>x.contaId==cid).reduce((s,x)=>s+Number(x.valor||0),0);
  return Number(c?.valor||0)-pago;
}
function atualizarStatusConta(cid){
  const c=db.contas.find(x=>x.id==cid);
  if(!c)return;
  const saldo=saldoConta(cid),pago=Number(c.valor||0)-saldo;
  c.status=saldo<=0&&c.valor>0?"Pago":pago>0?"Pago parcial":"Não pago";
}
function atualizarStatusDespesa(despesaId){
  const desp=db.despesas.find(x=>x.id==despesaId);
  const pago=db.baixas.filter(x=>x.despesaId==despesaId).reduce((s,x)=>s+Number(x.valor||0),0);
  desp.status=pago>=desp.valor?"Pago":pago>0?"Pago parcial":"Não pago";
}

function imprimirModulo(){window.print()}
function acoes(lista,itemId){return`<div class="row-actions"><button class="btn secondary mini" onclick="editarRegistro('${lista}',${itemId})">Editar</button><button class="btn danger mini" onclick="excluirRegistro('${lista}',${itemId})">Excluir</button></div>`}
function listaTexto(v){return String(v||"").split(/[,;\n]/).map(x=>x.trim()).filter(Boolean)}
function distribuir(total,partes){
  partes=Math.max(1,partes);
  const base=Math.floor(Number(total||0)/partes),resto=Number(total||0)%partes;
  return Array.from({length:partes},(_,i)=>base+(i<resto?1:0));
}
function quantidadesPorCor(op){
  const cores=listaTexto(op.cores||"Única");
  const mapa={};
  String(op.qtdPorCor||"").split(/\n/).map(l=>l.trim()).filter(Boolean).forEach(l=>{
    const [cor,valor]=l.split(/[:=]/);
    if(cor)mapa[cor.trim().toLowerCase()]=numero(valor);
  });
  if(Object.keys(mapa).length)return cores.map(c=>({cor:c,qtd:mapa[c.toLowerCase()]||0}));
  const partes=distribuir(Number(op.recebido||0),cores.length);
  return cores.map((cor,i)=>({cor,qtd:partes[i]}));
}
function totalPorCor(texto){
  return String(texto||"").split(/\n/).map(l=>l.trim()).filter(Boolean).reduce((s,l)=>{
    const [,valor]=l.split(/[:=]/);
    return s+numero(valor);
  },0);
}
function quantidadesPorTamanho(op){
  const tamanhos=listaTexto(op.tamanhos||op.grade||"Único");
  const mapa={};
  String(op.qtdPorTamanho||"").split(/\n/).map(l=>l.trim()).filter(Boolean).forEach(l=>{
    const [tam,valor]=l.split(/[:=]/);
    if(tam)mapa[tam.trim().toLowerCase()]=numero(valor);
  });
  if(Object.keys(mapa).length)return tamanhos.map(tam=>({tam,qtd:mapa[tam.toLowerCase()]||0}));
  const partes=distribuir(Number(op.recebido||0),tamanhos.length);
  return tamanhos.map((tam,i)=>({tam,qtd:partes[i]}));
}
function totalPorTamanho(texto){
  return String(texto||"").split(/\n/).map(l=>l.trim()).filter(Boolean).reduce((s,l)=>{
    const [,valor]=l.split(/[:=]/);
    return s+numero(valor);
  },0);
}
function distribuirPorPeso(total,pesos){
  const soma=pesos.reduce((s,x)=>s+Number(x||0),0);
  if(!soma)return distribuir(total,pesos.length);
  const brutos=pesos.map(p=>Number(total||0)*Number(p||0)/soma);
  const inteiros=brutos.map(Math.floor);
  let resto=Number(total||0)-inteiros.reduce((s,x)=>s+x,0);
  const ordem=brutos.map((v,i)=>({i,frac:v-Math.floor(v)})).sort((a,b)=>b.frac-a.frac);
  for(let i=0;i<resto;i++)inteiros[ordem[i%ordem.length].i]++;
  return inteiros;
}
function gradeOp(op){
  const tamanhos=listaTexto(op.tamanhos||op.grade||"Único");
  const pesosTamanho=quantidadesPorTamanho(op).map(x=>x.qtd);
  return quantidadesPorCor(op).map(item=>{
    const partes=distribuirPorPeso(item.qtd,pesosTamanho);
    return{cor:item.cor,total:item.qtd,tamanhos:tamanhos.map((tam,i)=>({tam,qtd:partes[i]}))};
  });
}
function gradeHtml(op){
  const grade=gradeOp(op),tamanhos=listaTexto(op.tamanhos||op.grade||"Único");
  const head=`<tr><th>Cor</th>${tamanhos.map(t=>`<th>${t}</th>`).join("")}<th>Total</th></tr>`;
  const body=grade.map(l=>`<tr><td>${l.cor}</td>${l.tamanhos.map(t=>`<td>${t.qtd}</td>`).join("")}<td><strong>${l.total}</strong></td></tr>`).join("");
  return`<div class="grade-table"><table><thead>${head}</thead><tbody>${body}</tbody></table></div>`;
}
function excluirRegistro(lista,itemId){
  if(!confirm("Excluir este registro?"))return;
  db[lista]=db[lista].filter(x=>x.id!==itemId);
  if(["entregas","quebras"].includes(lista))db.ops.forEach(op=>atualizarConta(op.id));
  salvar();hist("FACÇÃO NEXUS","Registro excluído",lista);render();
}
function editarRegistro(lista,itemId){
  const item=db[lista].find(x=>x.id===itemId);if(!item)return;
  const campos=Object.keys(item).filter(k=>k!=="id").map(k=>{
    const v=item[k];
    const type=typeof v==="number"?"number":typeof v==="boolean"?"select":String(v||"").length>60?"textarea":"text";
    const campo={label:k,name:k,type,value:v==null?"":v};
    if(typeof v==="boolean"){campo.options=`<option value="true"${v?" selected":""}>Sim</option><option value="false"${!v?" selected":""}>Não</option>`}
    if(type==="textarea")campo.full=true;
    return campo;
  });
  abrirForm("Editar registro",campos,d=>{
    Object.keys(d).forEach(k=>{
      const atual=item[k];
      if(typeof atual==="number")item[k]=["valorCliente","valorTerceiro","valorFixo","valorPeca","valor","saldo","fixo","comissao","manual","vale","pago","vales","liquido","total"].includes(k)?moeda(d[k]):numero(d[k]);
      else if(typeof atual==="boolean")item[k]=d[k]==="true";
      else item[k]=d[k];
    });
    if(lista==="ops")atualizarConta(item.id);
    if(["entregas","quebras"].includes(lista))atualizarConta(item.opId);
    hist("FACÇÃO NEXUS","Registro editado",lista);
  });
}
function totalValesCliente(clienteId){return db.valesClientes.filter(v=>v.clienteId==clienteId&&Number(v.saldo||0)>0).reduce((s,v)=>s+Number(v.saldo||0),0)}
function totalValesColaborador(colaboradorId){return db.valesColaboradores.filter(v=>v.colaboradorId==colaboradorId&&Number(v.saldo||0)>0).reduce((s,v)=>s+Number(v.saldo||0),0)}
function abaterVales(lista,campo,idPessoa,valor){
  let restante=Number(valor||0),usado=0;
  db[lista].filter(v=>v[campo]==idPessoa&&Number(v.saldo||0)>0).sort((a,b)=>String(a.data||"").localeCompare(String(b.data||""))||a.id-b.id).forEach(v=>{
    if(restante<=0)return;
    const uso=Math.min(restante,Number(v.saldo||0));
    v.saldo=Number(v.saldo||0)-uso;usado+=uso;restante-=uso;
    v.status=v.saldo<=0?"Utilizado":"Parcial";
  });
  return usado;
}

/* ========== PILL COM COR ========== */
function pill(status){
  const s=String(status||"").toLowerCase();
  let cls="pz";
  if(["pago","entregue","finalizada","ativo","completo","sim","boa","resolvida"].some(k=>s===k||s.startsWith(k)))cls="pg";
  else if(["parcial","recebida","pendente","faltando","sobrando"].some(k=>s.includes(k)))cls="pa";
  else if(["não pago","inativo","divergente","não","problema"].some(k=>s.includes(k)))cls="pr";
  else if(["nova","em andamento","em aberto"].some(k=>s.includes(k)))cls="pb";
  return`<span class="pill ${cls}">${status}</span>`;
}

/* ========== UI HELPERS ========== */
function renderMenu(){
  const pend=db.insumosOp.filter(x=>x.bloqueia&&!x.resolvida).length;
  menu.innerHTML=telas.map(t=>{
    const badge=(t==="Insumos"&&pend>0)?`<span class="badge">${pend}</span>`:"";
    return`<button class="${t===tela?"active":""}" onclick="abrir('${t}')"><span class="ico">${ICONS[t]||"•"}</span><span class="nav-label">${t}</span>${badge}</button>`;
  }).join("");
}
function abrir(t){tela=t;render()}

function page(titulo,subtitulo,botoes,conteudo){
  const imprimir=tela==="Produção"?"":btn("Imprimir PDF","imprimirModulo()","secondary");
  app.innerHTML=`<div class="top"><div class="page-info"><h2>${titulo}</h2><p>${subtitulo||""}</p></div><div class="actions">${botoes||""}${imprimir}</div></div>${conteudo}`;
}

let _tid=0;
function tabela(cols,rows){
  if(!rows.length)return`<div class="panel"><div class="empty-state"><div class="ei">📭</div><p>Nenhum registro encontrado.</p></div></div>`;
  const tid="t"+(++_tid);
  const head=cols.map(c=>`<th>${c}</th>`).join("");
  const body=rows.map(r=>`<tr>${r.map((c,i)=>`<td data-label="${cols[i]||""}">${c==null?"":c}</td>`).join("")}</tr>`).join("");
  return`<div class="panel">
    <div class="panel-toolbar">
      <input class="search-box" placeholder="🔍  Pesquisar..." oninput="filtrar('${tid}',this.value)">
      <span class="rec-count" id="${tid}c">${rows.length} registro${rows.length!==1?"s":""}</span>
    </div>
    <div class="table-wrap"><table id="${tid}"><thead><tr>${head}</tr></thead><tbody>${body}</tbody></table></div>
  </div>`;
}
function filtrar(tid,q){
  const tbl=document.getElementById(tid);if(!tbl)return;
  const rows=tbl.querySelectorAll("tbody tr");let n=0;
  rows.forEach(tr=>{const m=tr.textContent.toLowerCase().includes(q.toLowerCase());tr.style.display=m?"":"none";if(m)n++;});
  const el=document.getElementById(tid+"c");if(el)el.textContent=`${n} registro${n!==1?"s":""}`;
}

function btn(txt,fn,cls="primary"){return`<button class="btn ${cls}" onclick="${fn}">${txt}</button>`}
function opts(lista,label="nome"){return db[lista].map(x=>`<option value="${x.id}">${x[label]}</option>`).join("")}

function abrirForm(titulo,campos,aoSalvar){
  modalContent.innerHTML=`
    <div class="modal-head"><h2>${titulo}</h2></div>
    <div class="modal-body">
      <form id="formModal">
        ${campos.map(c=>{
          const w=c.full?"full":"";
          if(c.type==="select")return`<label class="${w}">${c.label}<select name="${c.name}">${c.options}</select></label>`;
          if(c.type==="textarea")return`<label class="${w}">${c.label}<textarea name="${c.name}">${c.value||""}</textarea></label>`;
          const financeiro=c.money||/\(R\$\)/.test(c.label||"")||["valorCliente","valorTerceiro","valorFixo","valorPeca","valor","fixo","comissao","manual","vale","pago","vales"].includes(c.name);
          const tipo=financeiro?"text":(c.type||"text");
          const extra=financeiro?' inputmode="decimal" placeholder="0,00"':(tipo==="number"?' step="any"':"");
          return`<label class="${w}">${c.label}<input type="${tipo}"${extra} name="${c.name}" value="${c.value||""}"></label>`;
        }).join("")}
        <div class="form-actions">
          <button type="button" class="btn secondary" onclick="modal.close()">Cancelar</button>
          <button class="btn primary">💾 Salvar</button>
        </div>
      </form>
    </div>`;
  formModal.onsubmit=e=>{e.preventDefault();aoSalvar(Object.fromEntries(new FormData(formModal).entries()));salvar();modal.close();render()};
  if(titulo==="Nova OP")prepararFormOp();
  modal.showModal();
}

/* ========== RENDER PRINCIPAL ========== */
function render(){
  aplicarTema();renderMenu();
  ({Dashboard:renderDashboard,Cadastros:renderCadastros,"Produção":renderOperacao,Insumos:renderInsumosOp,Estoque:renderEstoque,Financeiro:renderFinanceiro,Colaboradores:renderColaboradores,Terceiros:renderTerceiros,Acertos:renderAcertos,"Histórico":renderHistorico}[tela])();
}

/* ========== DASHBOARD ========== */
function renderDashboard(){
  const receber=db.contas.filter(x=>x.status!=="Pago").reduce((s,x)=>s+saldoConta(x.id),0);
  const pagar=db.despesas.filter(x=>x.status!=="Pago").reduce((s,x)=>s+Number(x.valor||0),0);
  const pend=db.insumosOp.filter(x=>x.bloqueia&&!x.resolvida).length;
  const opsAb=db.ops.filter(x=>x.status!=="Finalizada").length;
  const venc=db.ops.filter(x=>x.status!=="Finalizada"&&x.prazo&&x.prazo<hoje()).length;

  const cards=`<div class="kpi-grid">
    <div class="kpi"><span class="kpi-ico">🔧</span><div class="kpi-label">OPs em aberto</div><div class="kpi-val">${opsAb}</div>${venc>0?`<div class="kpi-sub" style="color:var(--red)">⚠ ${venc} vencida${venc>1?"s":""}</div>`:`<div class="kpi-sub muted">✓ Sem atraso</div>`}</div>
    <div class="kpi"><span class="kpi-ico">💰</span><div class="kpi-label">A receber</div><div class="kpi-val">${dinheiro(receber)}</div></div>
    <div class="kpi"><span class="kpi-ico">💸</span><div class="kpi-label">A pagar</div><div class="kpi-val">${dinheiro(pagar)}</div></div>
    <div class="kpi"><span class="kpi-ico">${pend>0?"🚨":"✅"}</span><div class="kpi-label">Pendências bloqueantes</div><div class="kpi-val" style="${pend>0?"color:var(--red)":""}">${pend}</div></div>
  </div>`;

  const rows=db.ops.slice().reverse().map(op=>{
    const t=totaisOp(op.id);
    const vencida=op.status!=="Finalizada"&&op.prazo&&op.prazo<hoje();
    return[op.codigo,nome("clientes",op.clienteId),nome("modelos",op.modeloId),pill(op.status),t.entregue,t.quebra,t.saldo,dinheiro(t.valor),vencida?`<span class="pill pr">⚠ Vencida</span>`:op.prazo||"—",acoes("ops",op.id)];
  });
  page("Dashboard","Resumo geral dos módulos","",cards+tabela(["OP","Fornecedor","Modelo","Status","Entregue","Quebra","Saldo","Valor","Prazo","Ações"],rows));
}

/* ========== CADASTROS ========== */
const cadastroCfg={
  clientes:{titulo:"Fornecedores",icone:"👥",desc:"Razão social, CNPJ/CPF, contato, endereço e status.",novo:"Fornecedor",lista:"clientes",form:"formCliente",busca:["nome","fantasia","documento","telefone","email","endereco"],cols:["Razão social","Fantasia","CNPJ/CPF","Telefone","E-mail","Status"],row:x=>[x.nome,x.fantasia||"—",x.documento||"—",x.telefone||"—",x.email||"—",pill(x.status||"Ativo")]},
  modelos:{titulo:"Modelos",icone:"🧵",desc:"Valores base por peça, tamanhos e aviamentos usados nas OPs.",novo:"Modelo",lista:"modelos",form:"formModelo",busca:["nome","descricao","tamanhos","aviamentos"],cols:["Modelo","Tamanhos","Receber","Pagar terc.","Status"],row:x=>[x.nome,x.tamanhos||"—",dinheiro(x.valorCliente),dinheiro(x.valorTerceiro),pill(x.status||"Ativo")]},
  colaboradores:{titulo:"Colaboradores",icone:"👤",desc:"Equipe interna, função, documento, telefone e PIX.",novo:"Colaborador",lista:"colaboradores",form:"formColaborador",busca:["nome","documento","funcao","telefone","pixChave"],cols:["Nome","Função","Documento","Telefone","PIX","Status"],row:x=>[x.nome,x.funcao||"—",x.documento||"—",x.telefone||"—",x.pixTipo||"—",pill(x.status||"Ativo")]},
  terceiros:{titulo:"Terceiros",icone:"🔄",desc:"Facções externas, contato, endereço, PIX e status.",novo:"Terceiro",lista:"terceiros",form:"formTerceiro",busca:["nome","fantasia","documento","telefone","endereco","pixChave"],cols:["Razão social","Fantasia","CNPJ/CPF","Telefone","PIX","Status"],row:x=>[x.nome,x.fantasia||"—",x.documento||"—",x.telefone||"—",x.pixTipo||"—",pill(x.status||"Ativo")]},
  estoque:{titulo:"Insumos",icone:"🗃️",desc:"Materiais, código, característica, cor, saldo e unidade.",novo:"Insumo",lista:"estoque",form:"formEstoque",busca:["nome","codigo","caracteristica","cor","unidade"],cols:["Material","Código","Característica","Cor","Estoque","Un.","Status"],row:x=>[x.nome,x.codigo||"—",x.caracteristica||"—",x.cor||"—",`<strong>${x.qtd||0}</strong>`,x.unidade||"—",pill(x.status||"Ativo")]}
};
function normalizarCadastro(lista,item){
  if(!item.status)item.status="Ativo";
  if(lista==="clientes"){item.fantasia=item.fantasia||item.nomeFantasia||"";item.documento=item.documento||item.cnpj||"";item.email=item.email||"";item.endereco=item.endereco||""}
  if(lista==="modelos"){item.valorColaborador=Number(item.valorColaborador||0);item.valorTerceiro=Number(item.valorTerceiro||0);item.descricao=item.descricao||item.obs||"";item.tamanhos=item.tamanhos||"";item.aviamentos=item.aviamentos||""}
  if(lista==="colaboradores"){item.funcao=item.funcao||"";item.documento=item.documento||"";item.pixTipo=item.pixTipo||"";item.pixChave=item.pixChave||""}
  if(lista==="terceiros"){item.fantasia=item.fantasia||"";item.documento=item.documento||"";item.endereco=item.endereco||"";item.pixTipo=item.pixTipo||"";item.pixChave=item.pixChave||""}
  if(lista==="estoque"){item.caracteristica=item.caracteristica||""}
  return item;
}
function dadosCadastro(tipo){
  const cfg=cadastroCfg[tipo];
  return db[cfg.lista].map(x=>normalizarCadastro(cfg.lista,x));
}
function abrirCadastro(tipo){cadastroAtual=tipo;filtroStatusCadastro="Todos";renderCadastros()}
function voltarCadastros(){cadastroAtual="";renderCadastros()}
function setStatusCadastro(v){filtroStatusCadastro=v;renderCadastros()}
function renderCadastros(){
  if(!cadastroAtual){
    const cards=Object.entries(cadastroCfg).map(([tipo,cfg])=>{
      const dados=dadosCadastro(tipo),ativos=dados.filter(x=>(x.status||"Ativo")==="Ativo").length;
      return`<article class="cadastro-card">
        <div><h3>${cfg.icone} ${cfg.titulo}</h3><p>${cfg.desc}</p></div>
        <div class="cadastro-meta"><span><strong>${dados.length}</strong> registros</span><span>${ativos} ativos</span></div>
        <button class="btn primary" onclick="abrirCadastro('${tipo}')">Abrir cadastro</button>
      </article>`;
    }).join("");
    page("Cadastros","Escolha uma base para consultar, filtrar e manter os dados.","",`<div class="cadastro-home">${cards}</div>`);
    return;
  }
  const cfg=cadastroCfg[cadastroAtual],dados=dadosCadastro(cadastroAtual);
  const filtrados=dados.filter(x=>filtroStatusCadastro==="Todos"||(x.status||"Ativo")===filtroStatusCadastro);
  const tabs=`<div class="cadastro-tabs">${Object.entries(cadastroCfg).map(([tipo,c])=>`<button class="${tipo===cadastroAtual?"active":""}" onclick="abrirCadastro('${tipo}')">${c.titulo}</button>`).join("")}</div>`;
  const filtro=`<div class="cadastro-list-head">${tabs}<label class="status-filter">Status<select onchange="setStatusCadastro(this.value)"><option${filtroStatusCadastro==="Todos"?" selected":""}>Todos</option><option${filtroStatusCadastro==="Ativo"?" selected":""}>Ativo</option><option${filtroStatusCadastro==="Inativo"?" selected":""}>Inativo</option></select></label></div>`;
  const rows=filtrados.map(x=>[...cfg.row(x),acoesCadastro(cadastroAtual,x.id)]);
  const b=btn("← Cadastros","voltarCadastros()","secondary")+btn(`+ ${cfg.novo}`,`${cfg.form}()`);
  page(cfg.titulo,cfg.desc,b,filtro+tabela([...cfg.cols,"Ações"],rows));
}
function acoesCadastro(tipo,itemId){
  const cfg=cadastroCfg[tipo],lista=cfg.lista;
  return`<div class="row-actions"><button class="btn secondary mini" onclick="${cfg.form}(${itemId})">Editar</button><button class="btn secondary mini" onclick="alternarStatusCadastro('${tipo}',${itemId})">Status</button><button class="btn danger mini" onclick="excluirRegistro('${lista}',${itemId})">Excluir</button></div>`;
}
function alternarStatusCadastro(tipo,itemId){
  const cfg=cadastroCfg[tipo],item=db[cfg.lista].find(x=>x.id===itemId);if(!item)return;
  item.status=(item.status||"Ativo")==="Ativo"?"Inativo":"Ativo";
  hist("Cadastros","Status atualizado",item.nome);
  salvar();render();
}
function salvarCadastro(lista,itemId,dados,criar){
  const item=itemId?db[lista].find(x=>x.id===itemId):{id:id(lista)};
  criar(item,dados);
  normalizarCadastro(lista,item);
  if(!itemId)db[lista].push(item);
  hist("Cadastros",itemId?"Cadastro editado":"Cadastro criado",item.nome);
}
function formCliente(itemId){
  const x=itemId?normalizarCadastro("clientes",db.clientes.find(i=>i.id===itemId)):{status:"Ativo"};
  abrirForm(itemId?"Editar fornecedor":"Novo fornecedor",[{label:"Razão social / nome",name:"nome",value:x.nome},{label:"Nome fantasia",name:"fantasia",value:x.fantasia},{label:"CNPJ/CPF",name:"documento",value:x.documento},{label:"Telefone",name:"telefone",value:x.telefone},{label:"E-mail",name:"email",value:x.email},{label:"Endereço",name:"endereco",value:x.endereco},{label:"Status",name:"status",type:"select",options:opcoesStatus(x.status)},{label:"Observação",name:"obs",type:"textarea",full:true,value:x.obs}],d=>salvarCadastro("clientes",itemId,d,(i,v)=>Object.assign(i,v)))}
function formModelo(itemId){
  const x=itemId?normalizarCadastro("modelos",db.modelos.find(i=>i.id===itemId)):{status:"Ativo"};
  abrirForm(itemId?"Editar modelo":"Novo modelo",[{label:"Nome",name:"nome",value:x.nome},{label:"Descrição",name:"descricao",type:"textarea",full:true,value:x.descricao},{label:"Tamanhos do modelo",name:"tamanhos",value:x.tamanhos||"P, M, G, GG"},{label:"Valor a receber por peça (R$)",name:"valorCliente",money:true,value:x.valorCliente},{label:"Valor pagar terceiro por peça (R$)",name:"valorTerceiro",money:true,value:x.valorTerceiro},{label:"Status",name:"status",type:"select",options:opcoesStatus(x.status)},{label:"Aviamentos necessários",name:"aviamentos",type:"textarea",full:true,value:x.aviamentos}],d=>salvarCadastro("modelos",itemId,d,(i,v)=>Object.assign(i,{nome:v.nome,descricao:v.descricao,tamanhos:v.tamanhos,valorCliente:moeda(v.valorCliente),valorTerceiro:moeda(v.valorTerceiro),status:v.status,aviamentos:v.aviamentos})))}
function formColaborador(itemId){
  const x=itemId?normalizarCadastro("colaboradores",db.colaboradores.find(i=>i.id===itemId)):{status:"Ativo"};
  abrirForm(itemId?"Editar colaborador":"Novo colaborador",[{label:"Nome",name:"nome",value:x.nome},{label:"Documento",name:"documento",value:x.documento},{label:"Função",name:"funcao",value:x.funcao},{label:"Telefone",name:"telefone",value:x.telefone},{label:"Tipo PIX",name:"pixTipo",type:"select",options:opcoesPix(x.pixTipo)},{label:"Chave PIX",name:"pixChave",value:x.pixChave},{label:"Valor fixo pagamento (R$)",name:"valorFixo",money:true,value:x.valorFixo},{label:"Status",name:"status",type:"select",options:opcoesStatus(x.status)},{label:"Observação",name:"obs",type:"textarea",full:true,value:x.obs}],d=>salvarCadastro("colaboradores",itemId,d,(i,v)=>Object.assign(i,{nome:v.nome,documento:v.documento,funcao:v.funcao,telefone:v.telefone,pixTipo:v.pixTipo,pixChave:v.pixChave,valorFixo:moeda(v.valorFixo),status:v.status,obs:v.obs})))}
function formTerceiro(itemId){
  const x=itemId?normalizarCadastro("terceiros",db.terceiros.find(i=>i.id===itemId)):{status:"Ativo"};
  abrirForm(itemId?"Editar terceiro":"Novo terceiro",[{label:"Razão social",name:"nome",value:x.nome},{label:"Nome fantasia",name:"fantasia",value:x.fantasia},{label:"CNPJ/CPF",name:"documento",value:x.documento},{label:"Telefone",name:"telefone",value:x.telefone},{label:"Endereço",name:"endereco",value:x.endereco},{label:"Tipo PIX",name:"pixTipo",type:"select",options:opcoesPix(x.pixTipo)},{label:"Chave PIX",name:"pixChave",value:x.pixChave},{label:"Status",name:"status",type:"select",options:opcoesStatus(x.status)},{label:"Observação",name:"obs",type:"textarea",full:true,value:x.obs}],d=>salvarCadastro("terceiros",itemId,d,(i,v)=>Object.assign(i,v)))}
function formEstoque(itemId){
  const x=itemId?normalizarCadastro("estoque",db.estoque.find(i=>i.id===itemId)):{status:"Ativo"};
  abrirForm(itemId?"Editar insumo":"Novo insumo",[{label:"Nome",name:"nome",value:x.nome},{label:"Código",name:"codigo",value:x.codigo},{label:"Característica",name:"caracteristica",value:x.caracteristica},{label:"Cor",name:"cor",value:x.cor},{label:"Quantidade",name:"qtd",type:"number",value:x.qtd},{label:"Unidade",name:"unidade",value:x.unidade},{label:"Status",name:"status",type:"select",options:opcoesStatus(x.status)},{label:"Observação",name:"obs",type:"textarea",full:true,value:x.obs}],d=>salvarCadastro("estoque",itemId,d,(i,v)=>Object.assign(i,{nome:v.nome,codigo:v.codigo,caracteristica:v.caracteristica,cor:v.cor,qtd:numero(v.qtd),unidade:v.unidade,status:v.status,obs:v.obs})))}
function opcoesStatus(atual="Ativo"){return["Ativo","Inativo"].map(v=>`<option${v===atual?" selected":""}>${v}</option>`).join("")}
function opcoesPix(atual=""){return["","CPF","CNPJ","E-mail","Telefone","Aleatória"].map(v=>`<option value="${v}"${v===atual?" selected":""}>${v||"Não informado"}</option>`).join("")}

/* ========== OPERACAO ========== */
function renderOperacao(){
  const b=btn("+ Nova OP","formOp()");
  const status=[...new Set(db.ops.map(op=>op.status||"Sem status"))];
  const filtro=`<div class="inline-filter"><label>Status da OP<select onchange="filtroStatusOperacao=this.value;renderOperacao()"><option${filtroStatusOperacao==="Todos"?" selected":""}>Todos</option>${status.map(s=>`<option${filtroStatusOperacao===s?" selected":""}>${s}</option>`).join("")}</select></label></div>`;
  const ops=db.ops.filter(op=>filtroStatusOperacao==="Todos"||op.status===filtroStatusOperacao);
  const abertas=db.ops.filter(op=>op.status!=="Finalizada").length;
  const entregues=db.entregas.reduce((s,x)=>s+Number(x.qtd||0),0);
  const saldoTotal=db.ops.reduce((s,op)=>s+totaisOp(op.id).saldo,0);
  const atrasadas=db.ops.filter(op=>op.status!=="Finalizada"&&op.prazo&&op.prazo<hoje()).length;
  const resumo=`<div class="kpi-grid">
    <div class="kpi"><span class="kpi-ico">🧾</span><div class="kpi-label">OPs abertas</div><div class="kpi-val">${abertas}</div></div>
    <div class="kpi"><span class="kpi-ico">📦</span><div class="kpi-label">Peças entregues</div><div class="kpi-val">${entregues}</div></div>
    <div class="kpi"><span class="kpi-ico">🧮</span><div class="kpi-label">Saldo em produção</div><div class="kpi-val">${saldoTotal}</div></div>
    <div class="kpi"><span class="kpi-ico">⏱️</span><div class="kpi-label">Prazos vencidos</div><div class="kpi-val" style="${atrasadas?"color:var(--red)":""}">${atrasadas}</div></div>
  </div>`;
  const rows=ops.slice().reverse().map(op=>{
    const t=totaisOp(op.id),vencida=op.status!=="Finalizada"&&op.prazo&&op.prazo<hoje();
    return[
      op.id,
      `<button class="op-link" onclick="abrirOpDetalhe(${op.id})">${op.codigo}</button>`,
      nome("clientes",op.clienteId),
      nome("modelos",op.modeloId),
      op.recebido,
      t.entregue,
      t.quebra,
      t.saldo,
      vencida?`<span class="pill pr">Vencida</span>`:(op.prazo||"—"),
      pill(op.status)
    ];
  });
  const lista=filtro+tabela(["ID","OP","Fornecedor","Modelo","Recebido","Entregue","Quebra","Saldo","Prazo","Status"],rows);
  page("Produção / OPs","Acompanhe entrada, andamento, entregas, quebras e fechamento.",b,resumo+`<div class="prod-grid single"><div>${lista}</div></div>`);
}
function abrirOpDetalhe(opId){
  opSelecionada=opId;
  const html=painelProducao();
  if(!html)return;
  modalContent.innerHTML=html;
  modal.showModal();
}
function painelProducao(){
  const op=db.ops.find(x=>x.id===opSelecionada);
  if(!op)return"";
  opSelecionada=op.id;
  const t=totaisOp(op.id),vencida=op.status!=="Finalizada"&&op.prazo&&op.prazo<hoje();
  const entregas=db.entregas.filter(x=>x.opId===op.id).slice().reverse();
  const quebras=db.quebras.filter(x=>x.opId===op.id).slice().reverse();
  const etapas=["Recebida","Em produção","Conferência","Entregue","Finalizada"];
  const idx=Math.max(0,etapas.indexOf(op.status));
  const timeline=etapas.map((etapa,i)=>`<button class="${i<=idx?"done":""}" onclick="mudarStatusOp(${op.id},'${etapa}')"><span><span class="timeline-dot"></span> ${etapa}</span><span>${i<=idx?"✓":""}</span></button>`).join("");
  const log=[...entregas.map(x=>({tipo:"Entrega",data:x.data,qtd:x.qtd,obs:x.obs})),...quebras.map(x=>({tipo:"Quebra",data:x.data||"—",qtd:x.qtd,obs:x.motivo||x.obs}))].slice(0,6);
  return`<aside class="prod-detail">
    <div class="prod-detail-head">
      <h3>${op.codigo}</h3>
      <p class="muted">${nome("clientes",op.clienteId)} · ${nome("modelos",op.modeloId)}</p>
    </div>
    <div class="prod-detail-body">
      <div class="prod-stats">
        <div class="prod-stat"><span>Recebido</span><strong>${op.recebido}</strong></div>
        <div class="prod-stat"><span>Saldo</span><strong>${t.saldo}</strong></div>
        <div class="prod-stat"><span>Entregue</span><strong>${t.entregue}</strong></div>
        <div class="prod-stat"><span>Quebra</span><strong>${t.quebra}</strong></div>
      </div>
      <div>${pill(op.status)} ${vencida?`<span class="pill pr">Prazo vencido</span>`:""}</div>
      <div>
        <strong>Grade de produção</strong>
        ${gradeHtml(op)}
      </div>
      <div class="timeline">${timeline}</div>
      <div class="prod-actions">
        <button class="btn secondary" onclick="formEditarOp(${op.id})">Editar OP</button>
        <button class="btn secondary" onclick="formEntrega(${op.id})">Registrar entrega</button>
        <button class="btn secondary" onclick="formQuebra(${op.id})">Registrar quebra</button>
        <button class="btn primary" onclick="formFinalizarOp(${op.id})">Finalizar</button>
        <button class="btn secondary" onclick="imprimirOp(${op.id})">Imprimir PDF</button>
        <button class="btn secondary" onclick="modal.close()">Fechar</button>
      </div>
      <div class="prod-log">
        <strong>Movimentos recentes</strong>
        ${log.length?log.map(x=>`<div class="prod-log-row"><span>${x.tipo}<br><small>${x.data} · ${x.obs||"sem obs."}</small></span><strong>${x.qtd}</strong></div>`).join(""):`<p class="muted">Nenhuma entrega ou quebra registrada.</p>`}
      </div>
    </div>
  </aside>`;
}
function mudarStatusOp(opId,status){
  const op=db.ops.find(x=>x.id===opId);if(!op)return;
  op.status=status;atualizarConta(op.id);salvar();hist("Produção","Status atualizado",`${op.codigo} · ${status}`);render();
}
function imprimirOp(opId){
  const op=db.ops.find(x=>x.id===opId);if(!op)return;
  const t=totaisOp(op.id);
  const entregas=db.entregas.filter(x=>x.opId===op.id);
  const quebras=db.quebras.filter(x=>x.opId===op.id);
  const linhasEntrega=entregas.length?entregas.map(x=>`<tr><td>${x.data||"—"}</td><td>${x.qtd}</td><td>${x.obs||"—"}</td></tr>`).join(""):`<tr><td colspan="3">Nenhuma entrega registrada.</td></tr>`;
  const linhasQuebra=quebras.length?quebras.map(x=>`<tr><td>${x.data||"—"}</td><td>${x.qtd}</td><td>${x.motivo||"—"}</td></tr>`).join(""):`<tr><td colspan="3">Nenhuma quebra registrada.</td></tr>`;
  const doc=window.open("", "_blank");
  if(!doc){toast("Nao foi possivel abrir a impressao. Verifique o bloqueador de pop-up.","er");return}
  doc.document.write(`<!doctype html><html><head><meta charset="utf-8"><title>${op.codigo}</title><style>
    body{font-family:Arial,sans-serif;color:#111;margin:32px;line-height:1.45}
    h1{margin:0 0 4px;font-size:28px} h2{font-size:17px;margin-top:24px}
    .muted{color:#555}.grid{display:grid;grid-template-columns:repeat(4,1fr);gap:10px;margin:22px 0}
    .box{border:1px solid #ddd;border-radius:10px;padding:12px}.box span{display:block;color:#555;font-size:11px;text-transform:uppercase}.box strong{font-size:22px}
    table{border-collapse:collapse;width:100%;margin-top:8px}th,td{border-bottom:1px solid #ddd;padding:8px;text-align:left}th{font-size:12px;text-transform:uppercase;color:#555}
    @media print{body{margin:18mm}.no-print{display:none}}
  </style></head><body>
    <button class="no-print" onclick="window.print()">Imprimir PDF</button>
    <h1>${op.codigo}</h1>
    <p class="muted">${nome("clientes",op.clienteId)} · ${nome("modelos",op.modeloId)}</p>
    <div class="grid">
      <div class="box"><span>Status</span><strong>${op.status}</strong></div>
      <div class="box"><span>Recebido</span><strong>${op.recebido}</strong></div>
      <div class="box"><span>Entregue</span><strong>${t.entregue}</strong></div>
      <div class="box"><span>Saldo</span><strong>${t.saldo}</strong></div>
    </div>
    <p><strong>Entrada:</strong> ${op.entrada||"—"} &nbsp; <strong>Prazo:</strong> ${op.prazo||"—"} &nbsp; <strong>Valor:</strong> ${dinheiro(t.valor)}</p>
    <p><strong>Grade:</strong> ${op.grade||"—"}</p>
    <h2>Grade de produção</h2>${gradeHtml(op)}
    <p><strong>Observacao:</strong> ${op.obs||"—"}</p>
    <h2>Entregas</h2><table><thead><tr><th>Data</th><th>Quantidade</th><th>Observacao</th></tr></thead><tbody>${linhasEntrega}</tbody></table>
    <h2>Quebras</h2><table><thead><tr><th>Data</th><th>Quantidade</th><th>Motivo</th></tr></thead><tbody>${linhasQuebra}</tbody></table>
  </body></html>`);
  doc.document.close();
  doc.focus();
}
function formEditarOp(opId){
  const op=db.ops.find(x=>x.id===opId);if(!op)return;
  abrirForm(`Editar ${op.codigo}`,[
    {label:"Fornecedor",name:"clienteId",type:"select",options:optsSelecionado("clientes",op.clienteId)},
    {label:"Modelo",name:"modeloId",type:"select",options:optsSelecionado("modelos",op.modeloId)},
    {label:"Quantidade total",name:"recebido",type:"number",value:op.recebido},
    {label:"Grade / tamanhos",name:"tamanhos",value:op.tamanhos||op.grade},
    {label:"Cores",name:"cores",value:op.cores},
    {label:"Quantidade individual por cor",name:"qtdPorCor",type:"textarea",full:true,value:op.qtdPorCor},
    {label:"Quantidade individual por tamanho",name:"qtdPorTamanho",type:"textarea",full:true,value:op.qtdPorTamanho},
    {label:"Entrada",name:"entrada",type:"date",value:op.entrada},
    {label:"Prazo",name:"prazo",type:"date",value:op.prazo},
    {label:"Valor por peça (R$)",name:"valorPeca",money:true,value:op.valorPeca},
    {label:"Status",name:"status",type:"select",options:opcoesStatusOp(op.status)},
    {label:"Observação",name:"obs",type:"textarea",full:true,value:op.obs}
  ],d=>{
    const totalCores=totalPorCor(d.qtdPorCor),totalTamanhos=totalPorTamanho(d.qtdPorTamanho);
    Object.assign(op,{clienteId:Number(d.clienteId),modeloId:Number(d.modeloId),recebido:totalCores||totalTamanhos||Number(d.recebido||0),grade:d.tamanhos,tamanhos:d.tamanhos,cores:d.cores,qtdPorCor:d.qtdPorCor,qtdPorTamanho:d.qtdPorTamanho,entrada:d.entrada,prazo:d.prazo,valorPeca:moeda(d.valorPeca),status:d.status,obs:d.obs});
    opSelecionada=op.id;atualizarConta(op.id);hist("Produção","OP editada",op.codigo);
  });
}
function optsSelecionado(lista,selecionado,label="nome"){
  return db[lista].map(x=>`<option value="${x.id}"${x.id==selecionado?" selected":""}>${x[label]}</option>`).join("");
}
function opcoesStatusOp(atual="Recebida"){
  return["Recebida","Em produção","Conferência","Entregue","Finalizada"].map(v=>`<option${v===atual?" selected":""}>${v}</option>`).join("");
}
function tamanhosModelo(modeloId){
  const m=db.modelos.find(x=>x.id==modeloId);
  return m?.tamanhos||"P, M, G, GG";
}
function prepararFormOp(){
  const modelo=formModal?.elements?.modeloId,tamanhos=formModal?.elements?.tamanhos;
  if(!modelo||!tamanhos)return;
  const aplicar=()=>{const valor=tamanhosModelo(Number(modelo.value));if(valor)tamanhos.value=valor};
  modelo.addEventListener("change",aplicar);
  aplicar();
}
function formOp(){abrirForm("Nova OP",[{label:"Fornecedor",name:"clienteId",type:"select",options:opts("clientes")},{label:"Modelo",name:"modeloId",type:"select",options:opts("modelos")},{label:"Quantidade total",name:"recebido",type:"number"},{label:"Grade / tamanhos",name:"tamanhos",value:tamanhosModelo(db.modelos[0]?.id)},{label:"Cores",name:"cores",value:"Preto, Branco"},{label:"Quantidade individual por cor",name:"qtdPorCor",type:"textarea",full:true,value:"Preto=150\nBranco=150"},{label:"Quantidade individual por tamanho",name:"qtdPorTamanho",type:"textarea",full:true,value:"P=60\nM=90\nG=90\nGG=60"},{label:"Entrada",name:"entrada",type:"date",value:hoje()},{label:"Prazo",name:"prazo",type:"date",value:hoje()},{label:"Valor por peça (R$)",name:"valorPeca",type:"number"},{label:"Observação",name:"obs",type:"textarea",full:true}],d=>{const m=db.modelos.find(x=>x.id==d.modeloId),novoId=id("ops"),codigo=`OP-${String(novoId).padStart(5,"0")}`,valor=moeda(d.valorPeca)||Number(m?.valorCliente||0),totalCores=totalPorCor(d.qtdPorCor),totalTamanhos=totalPorTamanho(d.qtdPorTamanho);db.ops.push({id:novoId,codigo,clienteId:Number(d.clienteId),modeloId:Number(d.modeloId),recebido:totalCores||totalTamanhos||Number(d.recebido||0),grade:d.tamanhos,tamanhos:d.tamanhos,cores:d.cores,qtdPorCor:d.qtdPorCor,qtdPorTamanho:d.qtdPorTamanho,entrada:d.entrada,prazo:d.prazo,valorPeca:valor,status:"Recebida",obs:d.obs});opSelecionada=null;hist("Produção","OP criada",codigo)})}
function formEntrega(opId=null){
  const campos=opId?[{label:"Data",name:"data",type:"date",value:hoje()},{label:"Quantidade",name:"qtd",type:"number"},{label:"Observação",name:"obs",type:"textarea",full:true}]:[{label:"OP",name:"opId",type:"select",options:opts("ops","codigo")},{label:"Data",name:"data",type:"date",value:hoje()},{label:"Quantidade",name:"qtd",type:"number"},{label:"Observação",name:"obs",type:"textarea",full:true}];
  abrirForm("Registrar entrega",campos,d=>{const alvo=Number(opId||d.opId);db.entregas.push({id:id("entregas"),opId:alvo,data:d.data,qtd:Number(d.qtd||0),obs:d.obs});const op=db.ops.find(x=>x.id==alvo),t=totaisOp(op.id);op.status=t.saldo<=0?"Entregue":"Entregue parcial";opSelecionada=op.id;atualizarConta(op.id);hist("Produção","Entrega registrada",`${op.codigo} — ${d.qtd} pç`)});
}
function formQuebra(opId=null){
  const campos=opId?[{label:"Data",name:"data",type:"date",value:hoje()},{label:"Quantidade",name:"qtd",type:"number"},{label:"Motivo",name:"motivo"},{label:"Observação",name:"obs",type:"textarea",full:true}]:[{label:"OP",name:"opId",type:"select",options:opts("ops","codigo")},{label:"Data",name:"data",type:"date",value:hoje()},{label:"Quantidade",name:"qtd",type:"number"},{label:"Motivo",name:"motivo"},{label:"Observação",name:"obs",type:"textarea",full:true}];
  abrirForm("Registrar quebra",campos,d=>{const alvo=Number(opId||d.opId);db.quebras.push({id:id("quebras"),opId:alvo,data:d.data,qtd:Number(d.qtd||0),motivo:d.motivo,obs:d.obs});opSelecionada=alvo;atualizarConta(alvo);hist("Produção","Quebra registrada",`${d.qtd} pç`)});
}
function formFinalizarOp(opId=null){
  const campos=opId?[{label:"Observação final",name:"obs",type:"textarea",full:true}]:[{label:"OP",name:"opId",type:"select",options:opts("ops","codigo")},{label:"Observação final",name:"obs",type:"textarea",full:true}];
  abrirForm("Finalizar OP",campos,d=>{const alvo=Number(opId||d.opId);const pend=db.insumosOp.some(x=>x.opId==alvo&&x.bloqueia&&!x.resolvida);if(pend){modal.close();toast("Existe insumo com pendência bloqueante nesta OP.","er");return}const op=db.ops.find(x=>x.id==alvo);op.status="Finalizada";op.obs=(op.obs||"")+"\nFechamento: "+d.obs;opSelecionada=op.id;atualizarConta(op.id);hist("Produção","OP finalizada",op.codigo)});
}

/* ========== INSUMOS OP ========== */
function renderInsumosOp(){
  const rows=db.insumosOp.map(x=>[opCodigo(x.opId),x.item,x.esperado,x.recebido,pill(x.situacao),x.bloqueia?`<span class="pill pr">Sim</span>`:`<span class="pill pz">Não</span>`,x.resolvida?`<span class="pill pg">Sim</span>`:`<span class="pill pa">Não</span>`,x.obs||"—",acoes("insumosOp",x.id)]);
  page("Insumos","Controle de insumos dentro da facção",btn("+ Adicionar insumo","formInsumoOp()"),tabela(["OP","Item","Esperado","Recebido","Situação","Bloqueia","Resolvida","Obs","Ações"],rows));
}
function formInsumoOp(){abrirForm("Insumo",[{label:"OP",name:"opId",type:"select",options:opts("ops","codigo")},{label:"Item",name:"item"},{label:"Esperado",name:"esperado",type:"number"},{label:"Recebido",name:"recebido",type:"number"},{label:"Situação",name:"situacao",type:"select",options:"<option>Completo</option><option>Faltando</option><option>Sobrando</option><option>Divergente</option>"},{label:"Bloqueia produção?",name:"bloqueia",type:"select",options:'<option value="0">Não</option><option value="1">Sim</option>'},{label:"Pendência resolvida?",name:"resolvida",type:"select",options:'<option value="0">Não</option><option value="1">Sim</option>'},{label:"Observação",name:"obs",type:"textarea",full:true}],d=>{db.insumosOp.push({id:id("insumosOp"),opId:Number(d.opId),item:d.item,esperado:Number(d.esperado||0),recebido:Number(d.recebido||0),situacao:d.situacao,bloqueia:d.bloqueia=="1",resolvida:d.resolvida=="1",obs:d.obs});hist("Insumos","Insumo cadastrado",d.item)})}

/* ========== ESTOQUE ========== */
function renderEstoque(){
  const rows=dadosCadastro("estoque").map(x=>[x.nome,x.codigo||"—",x.caracteristica||"—",x.cor||"—",`<strong>${x.qtd}</strong>`,x.unidade||"—",pill(x.status||"Ativo"),acoesCadastro("estoque",x.id)]);
  page("Estoque interno","Controle manual de insumos",btn("+ Novo insumo","formEstoque()")+btn("Entrada / Retirada","formMovEstoque()","secondary"),tabela(["Material","Código","Característica","Cor","Quantidade","Unidade","Status","Ações"],rows));
}
function formMovEstoque(){abrirForm("Movimentar estoque",[{label:"Insumo",name:"insumoId",type:"select",options:opts("estoque")},{label:"Tipo",name:"tipo",type:"select",options:"<option>Entrada</option><option>Retirada</option>"},{label:"Quantidade",name:"qtd",type:"number"},{label:"Data",name:"data",type:"date",value:hoje()},{label:"Observação",name:"obs",type:"textarea",full:true}],d=>{const item=db.estoque.find(x=>x.id==d.insumoId),antes=Number(item.qtd||0),qtd=Number(d.qtd||0);item.qtd=d.tipo==="Entrada"?antes+qtd:antes-qtd;db.movEstoque.push({id:id("movEstoque"),insumoId:item.id,tipo:d.tipo,qtd,data:d.data,antes,depois:item.qtd,obs:d.obs});hist("Estoque",d.tipo,`${item.nome}: ${qtd}`)})}

/* ========== FINANCEIRO ========== */
function renderFinanceiro(){
  const rows=[...db.contas.map(c=>["Conta OP",opCodigo(c.opId),dinheiro(c.valor),dinheiro(saldoConta(c.id)),pill(c.status),acoes("contas",c.id)]),...db.despesas.map(d=>["Despesa",d.descricao,dinheiro(d.valor),"—",pill(d.status),acoes("despesas",d.id)]),...db.valesClientes.map(v=>["Vale fornecedor",nome("clientes",v.clienteId),dinheiro(v.valor),dinheiro(v.saldo),pill(v.status),acoes("valesClientes",v.id)])];
  page("Financeiro","Contas, despesas, baixas e vales",btn("+ Despesa","formDespesa()")+btn("+ Baixa","formBaixa()","secondary")+btn("+ Vale fornecedor","formValeCliente()","secondary"),tabela(["Tipo","Referência","Valor","Saldo","Status","Ações"],rows));
}
function formDespesa(){abrirForm("Nova despesa",[{label:"Descrição",name:"descricao"},{label:"Fornecedor",name:"fornecedor"},{label:"Data",name:"data",type:"date",value:hoje()},{label:"Valor (R$)",name:"valor",type:"number"},{label:"Observação",name:"obs",type:"textarea",full:true}],d=>{db.despesas.push({id:id("despesas"),descricao:d.descricao,fornecedor:d.fornecedor,data:d.data,valor:moeda(d.valor),status:"Não pago",obs:d.obs});hist("Financeiro","Despesa cadastrada",d.descricao)})}
function formBaixa(){const c=db.contas.map(x=>`<option value="C-${x.id}">Conta ${opCodigo(x.opId)}</option>`).join(""),des=db.despesas.map(x=>`<option value="D-${x.id}">Despesa — ${x.descricao}</option>`).join("");abrirForm("Nova baixa",[{label:"Conta ou despesa",name:"alvo",type:"select",options:c+des},{label:"Data",name:"data",type:"date",value:hoje()},{label:"Valor (R$)",name:"valor",type:"number"},{label:"Observação",name:"obs",type:"textarea",full:true}],d=>{const[t,i]=d.alvo.split("-"),valor=moeda(d.valor);if(t==="C"){db.baixas.push({id:id("baixas"),tipo:"Recebimento",contaId:Number(i),data:d.data,valor,obs:d.obs});atualizarStatusConta(Number(i))}else{db.baixas.push({id:id("baixas"),tipo:"Pagamento",despesaId:Number(i),data:d.data,valor,obs:d.obs});atualizarStatusDespesa(Number(i))}hist("Financeiro","Baixa lançada",dinheiro(valor))})}
function formValeCliente(){abrirForm("Vale fornecedor",[{label:"Fornecedor",name:"clienteId",type:"select",options:opts("clientes")},{label:"Data",name:"data",type:"date",value:hoje()},{label:"Valor (R$)",name:"valor",type:"number"},{label:"Observação",name:"obs",type:"textarea",full:true}],d=>{const valor=moeda(d.valor);db.valesClientes.push({id:id("valesClientes"),clienteId:Number(d.clienteId),data:d.data,valor,saldo:valor,status:"Pendente",obs:d.obs});hist("Financeiro","Vale fornecedor cadastrado",dinheiro(valor))})}

/* ========== COLABORADORES ========== */
function renderColaboradores(){
  const rows=[...db.valesColaboradores.map(v=>["Vale",nome("colaboradores",v.colaboradorId),dinheiro(v.valor),dinheiro(v.saldo),pill(v.status),acoes("valesColaboradores",v.id)]),...db.pagamentosColaboradores.map(p=>["Pagamento",nome("colaboradores",p.colaboradorId),dinheiro(p.fixo+p.comissao),dinheiro(p.liquido),`<span class="pill pg">Pago</span>`,acoes("pagamentosColaboradores",p.id)])];
  page("Colaboradores","Vales e pagamentos",btn("+ Vale","formValeColaborador()")+btn("+ Pagamento","formPagamentoColaborador()","secondary"),tabela(["Tipo","Colaborador","Valor","Saldo / Líquido","Status","Ações"],rows));
}
function formValeColaborador(){abrirForm("Vale colaborador",[{label:"Colaborador",name:"colaboradorId",type:"select",options:opts("colaboradores")},{label:"Data",name:"data",type:"date",value:hoje()},{label:"Valor (R$)",name:"valor",type:"number"},{label:"Observação",name:"obs",type:"textarea",full:true}],d=>{const valor=moeda(d.valor);db.valesColaboradores.push({id:id("valesColaboradores"),colaboradorId:Number(d.colaboradorId),data:d.data,valor,saldo:valor,status:"Pendente",obs:d.obs});hist("Colaboradores","Vale colaborador",dinheiro(valor))})}
function formPagamentoColaborador(){abrirForm("Pagamento colaborador",[{label:"Colaborador",name:"colaboradorId",type:"select",options:db.colaboradores.map(x=>`<option value="${x.id}">${x.nome}${totalValesColaborador(x.id)>0?" — vale pendente "+dinheiro(totalValesColaborador(x.id)):""}</option>`).join("")},{label:"Data",name:"data",type:"date",value:hoje()},{label:"Valor fixo (R$)",name:"fixo",type:"number"},{label:"Comissão manual (R$)",name:"comissao",type:"number"},{label:"Vales descontados (R$)",name:"vales",type:"number"},{label:"Observação",name:"obs",type:"textarea",full:true}],d=>{const colId=Number(d.colaboradorId),col=db.colaboradores.find(x=>x.id==colId),fixo=moeda(d.fixo)||Number(col?.valorFixo||0),comissao=moeda(d.comissao),pendente=totalValesColaborador(colId);let vales=moeda(d.vales);if(pendente>0&&vales<=0)toast("Atenção: este colaborador tem vales pendentes de "+dinheiro(pendente)+".","nf");if(vales>0){const usado=abaterVales("valesColaboradores","colaboradorId",colId,vales);if(usado<vales)toast("Vale descontado limitado ao saldo pendente: "+dinheiro(usado)+".","nf");vales=usado}const liquido=fixo+comissao-vales;db.pagamentosColaboradores.push({id:id("pagamentosColaboradores"),colaboradorId:colId,data:d.data,fixo,comissao,vales,liquido,obs:d.obs});hist("Colaboradores","Pagamento colaborador",dinheiro(liquido))})}

/* ========== TERCEIROS ========== */
function renderTerceiros(){
  const rows=[...db.enviosTerceiros.map(e=>["Envio",nome("terceiros",e.terceiroId),opCodigo(e.opId),e.data,e.qtd,"—","—",acoes("enviosTerceiros",e.id)]),...db.retornosTerceiros.map(r=>["Retorno",`Envio #${r.envioId}`,"—",r.data,r.recebida,r.boa,r.problema,acoes("retornosTerceiros",r.id)]),...db.pagamentosTerceiros.map(p=>["Pagamento",nome("terceiros",p.terceiroId),"—",p.data,"—","—",dinheiro(p.total),acoes("pagamentosTerceiros",p.id)])];
  page("Terceiros","Envios, retornos e pagamentos",btn("+ Envio","formEnvioTerceiro()")+btn("+ Retorno","formRetornoTerceiro()","secondary")+btn("+ Pagamento","formPagamentoTerceiro()","secondary"),tabela(["Tipo","Terceiro / Envio","OP","Data","Qtd","Boa","Problema / Valor","Ações"],rows));
}
function formEnvioTerceiro(){abrirForm("Envio para terceiro",[{label:"OP",name:"opId",type:"select",options:opts("ops","codigo")},{label:"Terceiro",name:"terceiroId",type:"select",options:opts("terceiros")},{label:"Data",name:"data",type:"date",value:hoje()},{label:"Quantidade",name:"qtd",type:"number"},{label:"Observação",name:"obs",type:"textarea",full:true}],d=>{db.enviosTerceiros.push({id:id("enviosTerceiros"),opId:Number(d.opId),terceiroId:Number(d.terceiroId),data:d.data,qtd:Number(d.qtd||0),obs:d.obs});hist("Terceiros","Envio registrado",`${d.qtd} pç`)})}
function formRetornoTerceiro(){const o=db.enviosTerceiros.map(e=>`<option value="${e.id}">Envio #${e.id} — ${opCodigo(e.opId)}</option>`).join("");abrirForm("Retorno de terceiro",[{label:"Envio",name:"envioId",type:"select",options:o},{label:"Data",name:"data",type:"date",value:hoje()},{label:"Recebida",name:"recebida",type:"number"},{label:"Boa",name:"boa",type:"number"},{label:"Problema",name:"problema",type:"number"},{label:"Observação",name:"obs",type:"textarea",full:true}],d=>{db.retornosTerceiros.push({id:id("retornosTerceiros"),envioId:Number(d.envioId),data:d.data,recebida:Number(d.recebida||0),boa:Number(d.boa||0),problema:Number(d.problema||0),obs:d.obs});hist("Terceiros","Retorno registrado",`${d.boa} boas`)})}
function formPagamentoTerceiro(){abrirForm("Pagamento terceiro",[{label:"Terceiro",name:"terceiroId",type:"select",options:opts("terceiros")},{label:"Data",name:"data",type:"date",value:hoje()},{label:"Peças boas",name:"boa",type:"number"},{label:"Valor por peça (R$)",name:"valorPeca",type:"number"},{label:"Valor fixo (R$)",name:"fixo",type:"number"},{label:"Valor manual (R$)",name:"manual",type:"number"},{label:"Observação",name:"obs",type:"textarea",full:true}],d=>{const total=Number(d.boa||0)*moeda(d.valorPeca)+moeda(d.fixo)+moeda(d.manual);db.pagamentosTerceiros.push({id:id("pagamentosTerceiros"),terceiroId:Number(d.terceiroId),data:d.data,boa:Number(d.boa||0),valorPeca:moeda(d.valorPeca),fixo:moeda(d.fixo),manual:moeda(d.manual),total,obs:d.obs});hist("Terceiros","Pagamento terceiro",dinheiro(total))})}

/* ========== ACERTOS ========== */
function renderAcertos(){
  const rows=db.acertos.map(a=>[nome("clientes",a.clienteId),opCodigo(a.opId),a.data,dinheiro(a.vale),dinheiro(a.pago),dinheiro(a.saldo),acoes("acertos",a.id)]);
  page("Acertos quinzenais","Acerto manual por OP",btn("+ Novo acerto","formAcerto()"),tabela(["Fornecedor","OP","Data","Vale","Pago","Saldo","Ações"],rows));
}
function formAcerto(){abrirForm("Novo acerto",[{label:"Fornecedor",name:"clienteId",type:"select",options:db.clientes.map(x=>`<option value="${x.id}">${x.nome}${totalValesCliente(x.id)>0?" — vale disponível "+dinheiro(totalValesCliente(x.id)):""}</option>`).join("")},{label:"OP",name:"opId",type:"select",options:opts("ops","codigo")},{label:"Data",name:"data",type:"date",value:hoje()},{label:"Vale aplicado (R$)",name:"vale",type:"number"},{label:"Pago (R$)",name:"pago",type:"number"},{label:"Observação",name:"obs",type:"textarea",full:true}],d=>{const clienteId=Number(d.clienteId),opId=Number(d.opId);atualizarConta(opId);const conta=db.contas.find(x=>x.opId==opId);let vale=moeda(d.vale);const pago=moeda(d.pago);if(vale>0){const usado=abaterVales("valesClientes","clienteId",clienteId,vale);if(usado<vale)toast("Vale aplicado limitado ao saldo disponível: "+dinheiro(usado)+".","nf");vale=usado;if(vale>0)db.baixas.push({id:id("baixas"),tipo:"Vale fornecedor",contaId:conta.id,data:d.data,valor:vale,obs:d.obs})}if(pago)db.baixas.push({id:id("baixas"),tipo:"Recebimento",contaId:conta.id,data:d.data,valor:pago,obs:d.obs});atualizarStatusConta(conta.id);db.acertos.push({id:id("acertos"),clienteId,opId,data:d.data,vale,pago,saldo:saldoConta(conta.id),obs:d.obs});hist("Financeiro","Acerto quinzenal",opCodigo(opId))})}

/* ========== HISTÓRICO ========== */
function renderHistorico(){
  page("Histórico","Últimas alterações registradas","",tabela(["Data","Módulo","Ação","Observação","Ações"],db.historico.map(h=>[h.data,h.modulo,h.acao,h.obs||"—",acoes("historico",h.id)])));
}

render();
</script>
</body>
</html>

```
