<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
<meta name="apple-mobile-web-app-title" content="FinControl">
<title>FinControl</title>
<style>
:root{--bg:#080c12;--surf:#0f1420;--surf2:#161d2e;--surf3:#1c2438;--bord:#1e2a42;--bord2:#253352;--txt:#e8eef8;--muted:#5a7099;--muted2:#8399bb;--blue:#4d9fff;--green:#2ecc71;--red:#e74c3c;--orange:#f39c12;--purple:#9b59b6;--pessoal:#4d9fff;--impacto:#2ecc71;--mandacaru:#f39c12;}
*{margin:0;padding:0;box-sizing:border-box;-webkit-tap-highlight-color:transparent;}
html,body{height:100%;background:var(--bg);color:var(--txt);font-family:-apple-system,BlinkMacSystemFont,system-ui,sans-serif;}
input,select,textarea,button{font-family:inherit;}
input[type=date],input[type=month]{color-scheme:dark;}
#app{display:flex;flex-direction:column;height:100vh;max-width:480px;margin:0 auto;}
#topbar{background:var(--surf);border-bottom:1px solid var(--bord);padding:12px 16px 8px;flex-shrink:0;position:sticky;top:0;z-index:40;}
#topbar-row1{display:flex;align-items:center;justify-content:space-between;}
#topbar-title{font-weight:800;font-size:17px;}
#topbar-sub{font-size:11px;color:var(--muted2);font-weight:500;margin-top:2px;}
#cart-tabs{display:flex;gap:6px;padding:8px 12px;background:var(--surf);border-bottom:1px solid var(--bord);overflow-x:auto;flex-shrink:0;-webkit-overflow-scrolling:touch;}
#cart-tabs::-webkit-scrollbar{display:none;}
.cart-tab{padding:5px 12px;border-radius:20px;font-size:11px;font-weight:700;cursor:pointer;white-space:nowrap;border:1.5px solid var(--bord2);color:var(--muted2);transition:all .2s;flex-shrink:0;}
.cart-tab.t-all{border-color:var(--muted);color:var(--txt);background:rgba(255,255,255,.06);}
.cart-tab.t-pessoal{border-color:var(--pessoal);color:var(--pessoal);background:rgba(77,159,255,.12);}
.cart-tab.t-impacto{border-color:var(--impacto);color:var(--impacto);background:rgba(46,204,113,.12);}
.cart-tab.t-mandacaru{border-color:var(--mandacaru);color:var(--mandacaru);background:rgba(243,156,18,.12);}
#content{flex:1;overflow-y:auto;padding:12px 12px 90px;-webkit-overflow-scrolling:touch;}
#bottomnav{position:fixed;bottom:0;left:50%;transform:translateX(-50%);width:100%;max-width:480px;background:var(--surf);border-top:1px solid var(--bord);display:flex;z-index:40;padding-bottom:env(safe-area-inset-bottom);}
.bnav{flex:1;padding:8px 2px 6px;text-align:center;cursor:pointer;border-top:2px solid transparent;}
.bnav.active{border-top-color:var(--blue);}
.bnav .ni{font-size:18px;}
.bnav .nl{font-size:8px;font-weight:700;margin-top:2px;color:var(--muted);letter-spacing:.02em;}
.bnav.active .nl{color:var(--blue);}
.cards{display:grid;grid-template-columns:1fr 1fr;gap:8px;margin-bottom:12px;}
.card{background:var(--surf);border:1px solid var(--bord);border-radius:12px;padding:12px;position:relative;overflow:hidden;}
.card::before{content:'';position:absolute;top:0;left:0;right:0;height:2px;background:var(--c,var(--blue));}
.card-lbl{font-size:9px;color:var(--muted2);font-weight:700;text-transform:uppercase;letter-spacing:.07em;margin-bottom:5px;}
.card-val{font-family:monospace;font-size:15px;font-weight:800;}
.card-sub{font-size:9px;color:var(--muted);margin-top:2px;}
.box{background:var(--surf);border:1px solid var(--bord);border-radius:12px;overflow:hidden;margin-bottom:12px;}
.box-hdr{padding:11px 14px;border-bottom:1px solid var(--bord);font-weight:700;font-size:13px;display:flex;align-items:center;justify-content:space-between;gap:8px;}
.box-body{padding:12px;}
.frow{display:grid;grid-template-columns:1fr 1fr;gap:8px;margin-bottom:8px;}
.frow.full{grid-template-columns:1fr;}
.fg label{display:block;font-size:9px;font-weight:800;color:var(--muted2);text-transform:uppercase;letter-spacing:.06em;margin-bottom:4px;}
.fg input,.fg select,.fg textarea{width:100%;background:var(--surf2);border:1px solid var(--bord2);border-radius:8px;padding:9px 11px;color:var(--txt);font-size:13px;outline:none;-webkit-appearance:none;}
.fg input:focus,.fg select:focus,.fg textarea:focus{border-color:var(--blue);}
.tipo-row{display:flex;gap:8px;margin-bottom:10px;}
.tipo-btn{flex:1;padding:9px;border-radius:8px;border:none;cursor:pointer;font-weight:800;font-size:13px;}
.tipo-rec{background:rgba(46,204,113,.15);color:var(--green);outline:1.5px solid var(--green);}
.tipo-desp{background:rgba(231,76,60,.15);color:var(--red);outline:1.5px solid var(--red);}
.tipo-off{background:transparent;color:var(--muted);outline:1px solid var(--bord2);}
.btn-add{width:100%;padding:12px;border-radius:10px;border:none;cursor:pointer;background:var(--green);color:#000;font-weight:800;font-size:14px;margin-top:6px;}
.btn-blue{width:100%;padding:11px;border-radius:10px;border:none;cursor:pointer;background:var(--blue);color:#000;font-weight:800;font-size:13px;margin-top:8px;}
.btn-sm{padding:5px 11px;border-radius:7px;border:none;cursor:pointer;font-size:11px;font-weight:700;}
.btn-ok{background:var(--green);color:#000;}
.btn-cancel2{background:var(--surf2);border:1px solid var(--bord2)!important;color:var(--muted2);}
.btn-del{background:rgba(231,76,60,.12);border:none;color:var(--red);border-radius:6px;padding:4px 7px;cursor:pointer;font-size:13px;flex-shrink:0;}
.btn-edit{background:rgba(77,159,255,.12);border:none;color:var(--blue);border-radius:6px;padding:4px 7px;cursor:pointer;font-size:13px;flex-shrink:0;}
.btn-launch{background:rgba(77,159,255,.12);border:1.5px solid var(--blue);color:var(--blue);border-radius:7px;padding:3px 9px;cursor:pointer;font-size:10px;font-weight:800;white-space:nowrap;flex-shrink:0;}
.btn-launch:disabled{opacity:.3;cursor:not-allowed;}
.badge{padding:2px 7px;border-radius:20px;font-size:9px;font-weight:800;text-transform:uppercase;}
.badge-rec{background:rgba(46,204,113,.15);color:var(--green);}
.badge-desp{background:rgba(231,76,60,.15);color:var(--red);}
.item-row{padding:9px 12px;border-bottom:1px solid var(--bord);display:flex;align-items:center;gap:8px;}
.item-row:last-child{border-bottom:none;}
.item-desc{font-size:13px;font-weight:700;white-space:nowrap;overflow:hidden;text-overflow:ellipsis;}
.item-meta{font-size:10px;color:var(--muted2);margin-top:1px;}
.item-val{font-family:monospace;font-weight:800;font-size:13px;white-space:nowrap;}
.empty{padding:28px;text-align:center;color:var(--muted);font-size:13px;}
.empty-icon{font-size:28px;margin-bottom:6px;}
.bar-wrap{display:flex;align-items:flex-end;gap:3px;height:95px;overflow-x:auto;padding-bottom:2px;}
.bgrp{display:flex;flex-direction:column;align-items:center;gap:2px;min-width:20px;flex:1;}
.bars2{display:flex;gap:2px;align-items:flex-end;height:78px;}
.bar2{width:7px;border-radius:3px 3px 0 0;min-height:2px;}
.bar-lbl{font-size:7px;color:var(--muted);}
.cat-row{display:flex;align-items:center;gap:8px;margin-bottom:7px;}
.cat-dot{width:7px;height:7px;border-radius:50%;flex-shrink:0;}
.cat-name{flex:1;font-size:11px;}
.cat-track{flex:2;height:4px;background:var(--surf2);border-radius:2px;overflow:hidden;}
.cat-fill{height:100%;border-radius:2px;}
.cat-amt{font-size:10px;font-family:monospace;color:var(--muted2);min-width:68px;text-align:right;}
.toggle{width:32px;height:17px;border-radius:9px;border:none;cursor:pointer;position:relative;transition:background .2s;flex-shrink:0;}
.toggle.on{background:var(--green);}
.toggle.off{background:var(--bord2);}
.toggle::after{content:'';position:absolute;top:2px;width:13px;height:13px;border-radius:50%;background:#fff;transition:left .2s;}
.toggle.on::after{left:17px;}
.toggle.off::after{left:2px;}
.alert{border-radius:12px;padding:11px 13px;margin-bottom:12px;font-size:12px;line-height:1.6;}
.alert-blue{background:rgba(77,159,255,.08);border:1px solid rgba(77,159,255,.3);color:var(--blue);}
.alert-green{background:rgba(46,204,113,.08);border:1px solid rgba(46,204,113,.3);color:var(--green);}
.alert strong{display:block;font-size:13px;font-weight:800;margin-bottom:2px;}
.voz-box{background:linear-gradient(135deg,#0d1525,var(--surf));border:1px solid var(--bord2);border-radius:12px;padding:12px;margin-bottom:12px;}
.voz-title{font-size:10px;font-weight:800;color:var(--muted2);text-transform:uppercase;letter-spacing:.08em;margin-bottom:8px;}
.voz-row{display:flex;align-items:flex-start;gap:8px;margin-bottom:8px;}
.voz-input{flex:1;background:var(--surf2);border:1px solid var(--bord2);border-radius:8px;padding:9px 11px;color:var(--txt);font-size:13px;outline:none;resize:none;}
.voz-input:focus{border-color:var(--blue);}
.voz-btn{width:40px;height:40px;border-radius:50%;border:none;cursor:pointer;font-size:18px;background:var(--surf2);color:var(--muted2);flex-shrink:0;display:flex;align-items:center;justify-content:center;}
.voz-btn.thinking{background:rgba(243,156,18,.15);color:var(--orange);}
.voz-exs{display:flex;flex-wrap:wrap;gap:5px;}
.voz-ex{background:var(--surf2);border:1px solid var(--bord2);padding:4px 9px;border-radius:16px;font-size:10px;color:var(--muted2);cursor:pointer;}
.voz-prev{background:rgba(77,159,255,.08);border:1px solid rgba(77,159,255,.3);border-radius:10px;padding:10px 12px;margin-top:10px;}
.voz-prev-title{font-size:9px;font-weight:800;color:var(--blue);text-transform:uppercase;letter-spacing:.06em;margin-bottom:7px;}
.voz-flds{display:flex;flex-wrap:wrap;gap:5px;margin-bottom:8px;}
.vf{background:var(--surf);border:1px solid var(--bord);border-radius:6px;padding:4px 8px;}
.vf strong{font-size:8px;color:var(--muted2);text-transform:uppercase;display:block;margin-bottom:1px;}
.vf span{font-size:12px;font-weight:700;}
.voz-acts{display:flex;gap:7px;}
.cart-badge{display:inline-block;padding:1px 6px;border-radius:10px;font-size:9px;font-weight:800;text-transform:uppercase;}
.cb-pessoal{background:rgba(77,159,255,.15);color:var(--pessoal);}
.cb-impacto{background:rgba(46,204,113,.15);color:var(--impacto);}
.cb-mandacaru{background:rgba(243,156,18,.15);color:var(--mandacaru);}
#toast{position:fixed;bottom:80px;left:50%;transform:translateX(-50%) translateY(16px);background:var(--green);color:#000;padding:9px 18px;border-radius:20px;font-size:12px;font-weight:800;opacity:0;transition:all .3s;z-index:999;pointer-events:none;white-space:nowrap;}
#toast.show{opacity:1;transform:translateX(-50%) translateY(0);}
.month-filter{background:var(--surf2);border:1px solid var(--bord2);color:var(--txt);padding:4px 7px;border-radius:6px;font-size:11px;}
</style>
</head>
<body>
<div id="app">
  <div id="topbar">
    <div id="topbar-row1">
      <div id="topbar-title">💰 FinControl</div>
      <div id="topbar-year" style="font-size:11px;color:var(--muted2);font-weight:600;"></div>
    </div>
    <div id="topbar-sub"></div>
  </div>
  <div id="cart-tabs"></div>
  <div id="content"></div>
  <nav id="bottomnav">
    <div class="bnav active" id="nav-dashboard" onclick="go('dashboard')"><div class="ni">📊</div><div class="nl">Dashboard</div></div>
    <div class="bnav" id="nav-fixos" onclick="go('fixos')"><div class="ni">📌</div><div class="nl">Fixos</div></div>
    <div class="bnav" id="nav-lancamentos" onclick="go('lancamentos')"><div class="ni">➕</div><div class="nl">Lançar</div></div>
    <div class="bnav" id="nav-relatorios" onclick="go('relatorios')"><div class="ni">📈</div><div class="nl">Relatórios</div></div>
  </nav>
</div>
<div id="toast"></div>
<script>
  const NOW=new Date(),TODAY=NOW.toISOString().split('T')[0],CUR_MONTH=TODAY.slice(0,7),YEAR=NOW.getFullYear().toString();
const MESES=['Jan','Fev','Mar','Abr','Mai','Jun','Jul','Ago','Set','Out','Nov','Dez'];
const COLORS=['#4d9fff','#2ecc71','#f39c12','#9b59b6','#e74c3c','#1abc9c','#e67e22','#3498db'];
const brl=v=>new Intl.NumberFormat('pt-BR',{style:'currency',currency:'BRL'}).format(v||0);
const CARTEIRAS=[{id:'pessoal',nome:'Pessoal',emoji:'👤',cor:'var(--pessoal)'},{id:'impacto',nome:'Impacto Representações',emoji:'🏢',cor:'var(--impacto)'},{id:'mandacaru',nome:'Mandacaru',emoji:'🌵',cor:'var(--mandacaru)'}];
const CATS=['Salário','Freela','Pró-labore','Distribuição de Lucros','Vendas','Serviços Prestados','Investimentos','Outros Recebimentos','Alimentação','Moradia','Transporte','Saúde','Educação','Lazer','Vestuário','Fornecedores','Folha de Pagamento','Marketing','Impostos','Infraestrutura','Outros'];
const FORMAS=['PIX','Dinheiro','Cartão Débito','Cartão Crédito','Boleto','TED/DOC','Outros'];
let db={pessoal:[],impacto:[],mandacaru:[],fixos:[]};
try{['pessoal','impacto','mandacaru','fixos'].forEach(k=>{const v=localStorage.getItem('fc3_'+k);if(v)db[k]=JSON.parse(v);});}catch(e){}
if(!db.pessoal.length&&!db.impacto.length){
  db.pessoal=[{id:1,data:CUR_MONTH+'-01',desc:'Salário',valor:5000,tipo:'Receita',cat:'Salário',forma:'PIX',carteira:'pessoal'},{id:2,data:CUR_MONTH+'-05',desc:'Aluguel',valor:1500,tipo:'Despesa',cat:'Moradia',forma:'Boleto',carteira:'pessoal'}];
  db.impacto=[{id:10,data:CUR_MONTH+'-02',desc:'Faturamento',valor:12000,tipo:'Receita',cat:'Serviços Prestados',forma:'PIX',carteira:'impacto'},{id:11,data:CUR_MONTH+'-08',desc:'Fornecedor',valor:3000,tipo:'Despesa',cat:'Fornecedores',forma:'Boleto',carteira:'impacto'}];
  db.mandacaru=[{id:20,data:CUR_MONTH+'-03',desc:'Receita',valor:8000,tipo:'Receita',cat:'Vendas',forma:'PIX',carteira:'mandacaru'}];
  db.fixos=[{id:200,desc:'Salário',valor:5000,tipo:'Receita',cat:'Salário',forma:'PIX',carteira:'pessoal',ativo:true,lancados:[]},{id:201,desc:'Aluguel',valor:1500,tipo:'Despesa',cat:'Moradia',forma:'Boleto',carteira:'pessoal',ativo:true,lancados:[]},{id:202,desc:'Pró-labore Impacto',valor:3000,tipo:'Receita',cat:'Pró-labore',forma:'PIX',carteira:'impacto',ativo:true,lancados:[]},{id:203,desc:'Pró-labore Mandacaru',valor:2000,tipo:'Receita',cat:'Pró-labore',forma:'PIX',carteira:'mandacaru',ativo:true,lancados:[]}];
  save();
}
function save(){try{['pessoal','impacto','mandacaru','fixos'].forEach(k=>localStorage.setItem('fc3_'+k,JSON.stringify(db[k])));}catch(e){}}
let page='dashboard',carteira='all',vozPending={},vozTexto={p:'',i:'',m:''},filtroMes={p:CUR_MONTH,i:CUR_MONTH,m:CUR_MONTH};
const tipoState={p:'Receita',i:'Receita',m:'Receita',fx:'Receita'};
let editingFixo=null;
function go(p){page=p;document.querySelectorAll('.bnav').forEach(n=>n.classList.remove('active'));const el=document.getElementById('nav-'+p);if(el)el.classList.add('active');document.getElementById('topbar-sub').textContent={dashboard:'Dashboard',fixos:'Lançamentos Fixos',lancamentos:'Novo Lançamento',relatorios:'Relatórios'}[p]||'';renderCartTabs();render();}
function setCarteira(c){carteira=c;renderCartTabs();render();}
function renderCartTabs(){const tabs=document.getElementById('cart-tabs');const show=['dashboard','lancamentos','relatorios'].includes(page);tabs.style.display=show?'flex':'none';if(!show)return;const opts=[{id:'all',nome:'Geral',emoji:'🌐'},...CARTEIRAS];tabs.innerHTML=opts.map(c=>`<div class="cart-tab ${carteira===c.id?'t-'+c.id:''}" onclick="setCarteira('${c.id}')">${c.emoji} ${c.id==='all'?'Geral':c.nome.split(' ')[0]}</div>`).join('');}
function render(){const c=document.getElementById('content');if(page==='dashboard')c.innerHTML=renderDashboard();else if(page==='fixos')c.innerHTML=renderFixos();else if(page==='lancamentos')c.innerHTML=renderLancamentos();else c.innerHTML=renderRelatorios();}
function getAllData(){if(carteira==='all')return[...db.pessoal,...db.impacto,...db.mandacaru];return db[carteira]||[];}
function getCart(id){return CARTEIRAS.find(c=>c.id===id)||CARTEIRAS[0];}
function cartBadge(cid){const c=getCart(cid);return`<span class="cart-badge cb-${cid}">${c.emoji} ${c.nome.split(' ')[0]}</span>`;}
function card(label,val,color,sub=''){const cs={green:'var(--green)',red:'var(--red)',blue:'var(--blue)',orange:'var(--orange)',pessoal:'var(--pessoal)',impacto:'var(--impacto)',mandacaru:'var(--mandacaru)'};const c=cs[color]||cs.blue;return`<div class="card" style="--c:${c}"><div class="card-lbl">${label}</div><div class="card-val" style="color:${c}">${val}</div>${sub?`<div class="card-sub">${sub}</div>`:''}</div>`;}
function badge(tipo){return`<span class="badge ${tipo==='Receita'?'badge-rec':'badge-desp'}">${tipo}</span>`;}
function barChart(data){const yd=data.filter(i=>i.data.startsWith(YEAR));const vals=MESES.map((_,idx)=>{const m=String(idx+1).padStart(2,'0');const r=yd.filter(i=>i.data.slice(5,7)===m&&i.tipo==='Receita').reduce((s,i)=>s+i.valor,0);const d=yd.filter(i=>i.data.slice(5,7)===m&&i.tipo==='Despesa').reduce((s,i)=>s+i.valor,0);return{r,d,m:MESES[idx]};});const max=Math.max(...vals.map(v=>Math.max(v.r,v.d)),1);return`<div class="bar-wrap">${vals.map(v=>`<div class="bgrp"><div class="bars2"><div class="bar2" style="height:${Math.max(v.r/max*73,2)}px;background:var(--green)"></div><div class="bar2" style="height:${Math.max(v.d/max*73,2)}px;background:var(--red)"></div></div><div class="bar-lbl">${v.m}</div></div>`).join('')}</div><div style="display:flex;gap:10px;margin-top:6px;font-size:10px;color:var(--muted2)"><span>🟢 Receita</span><span>🔴 Despesa</span></div>`;}
function catBars(data){const cats={};data.filter(i=>i.tipo==='Despesa').forEach(i=>{const c=i.cat||'Outros';cats[c]=(cats[c]||0)+i.valor;});const sorted=Object.entries(cats).sort((a,b)=>b[1]-a[1]).slice(0,5);const max=sorted[0]?.[1]||1;if(!sorted.length)return'<div style="color:var(--muted);font-size:12px">Sem despesas ainda.</div>';return sorted.map(([c,v],i)=>`<div class="cat-row"><div class="cat-dot" style="background:${COLORS[i]}"></div><div class="cat-name">${c}</div><div class="cat-track"><div class="cat-fill" style="width:${(v/max*100).toFixed(0)}%;background:${COLORS[i]}"></div></div><div class="cat-amt">${brl(v)}</div></div>`).join('');}
function renderDashboard(){const all=getAllData();const rec=all.filter(i=>i.tipo==='Receita').reduce((s,i)=>s+i.valor,0);const desp=all.filter(i=>i.tipo==='Despesa').reduce((s,i)=>s+i.valor,0);const saldo=rec-desp;const pendentes=db.fixos.filter(f=>f.ativo&&(!f.lancados||!f.lancados.includes(CUR_MONTH)));const recent=[...all].sort((a,b)=>b.data.localeCompare(a.data)).slice(0,6);let resumo='';if(carteira==='all'){resumo='<div class="box"><div class="box-hdr">🗂️ Por Carteira</div>';CARTEIRAS.forEach(c=>{const d=db[c.id]||[];const r=d.filter(i=>i.tipo==='Receita').reduce((s,i)=>s+i.valor,0);const dp=d.filter(i=>i.tipo==='Despesa').reduce((s,i)=>s+i.valor,0);const sl=r-dp;resumo+=`<div class="item-row" onclick="setCarteira('${c.id}')" style="cursor:pointer"><div style="flex:1"><div class="item-desc">${c.emoji} ${c.nome}</div><div class="item-meta">Rec: ${brl(r)} · Desp: ${brl(dp)}</div></div><div class="item-val" style="color:${sl>=0?'var(--green)':'var(--red)'}">${sl>=0?'+':''}${brl(sl)}</div></div>`;});resumo+='</div>';}return(pendentes.length?`<div class="alert alert-blue" onclick="go('fixos')"><strong>📌 ${pendentes.length} fixo(s) pendente(s) este mês</strong>Toque para lançar →</div>`:'')+`<div class="cards">${card('Receita Total',brl(rec),'green',all.filter(i=>i.tipo==='Receita').length+' lançamentos')}${card('Despesa Total',brl(desp),'red',all.filter(i=>i.tipo==='Despesa').length+' lançamentos')}${card('Saldo / Lucro',brl(saldo),saldo>=0?'green':'red')}${card('% Economia',rec>0?((saldo/rec)*100).toFixed(1)+'%':'0%','orange')}</div>`+resumo+`<div class="box"><div class="box-hdr">📅 Evolução ${YEAR}</div><div class="box-body">${barChart(all)}</div></div><div class="box"><div class="box-hdr">🏷️ Top Despesas</div><div class="box-body">${catBars(all)}</div></div><div class="box"><div class="box-hdr">🕐 Últimos Lançamentos</div>${recent.length?recent.map(i=>`<div class="item-row"><div style="flex:1;min-width:0"><div class="item-desc">${i.desc}</div><div class="item-meta">${i.data.split('-').reverse().join('/')} · ${cartBadge(i.carteira||'pessoal')}</div></div>${badge(i.tipo)}<div class="item-val" style="color:${i.tipo==='Receita'?'var(--green)':'var(--red)'}">${i.tipo==='Receita'?'+':'-'}${brl(i.valor)}</div></div>`).join(''):'<div class="empty"><div class="empty-icon">📭</div>Sem lançamentos.</div>'}</div>`;}
function renderFixos(){const pendentes=db.fixos.filter(f=>f.ativo&&(!f.lancados||!f.lancados.includes(CUR_MONTH)));return(pendentes.length?`<div class="alert alert-blue"><strong>📌 ${pendentes.length} fixo(s) pendente(s) este mês</strong><button class="btn-blue" onclick="lancarTodos()">⚡ Lançar Todos Agora</button></div>`:`<div class="alert alert-green"><strong>✅ Todos os fixos já foram lançados este mês!</strong></div>`)+`<div class="box"><div class="box-hdr">${editingFixo?'✏️ Editando Fixo':'➕ Novo Fixo'}${editingFixo?'<button class="btn-sm btn-cancel2" onclick="cancelarEditFixo()">✖ Cancelar</button>':''}</div><div class="box-body"><div class="tipo-row"><button id="btn-rec-fx" class="tipo-btn ${(editingFixo?editingFixo.tipo:tipoState.fx)==='Receita'?'tipo-rec':'tipo-off'}" onclick="setTipoForm('fx','Receita')">✅ Receita</button><button id="btn-desp-fx" class="tipo-btn ${(editingFixo?editingFixo.tipo:tipoState.fx)==='Despesa'?'tipo-desp':'tipo-off'}" onclick="setTipoForm('fx','Despesa')">❌ Despesa</button></div><div class="frow full"><div class="fg"><label>Descrição</label><input type="text" id="fx-desc" placeholder="Ex: Salário, Aluguel…" value="${editingFixo?editingFixo.desc:''}"></div></div><div class="frow"><div class="fg"><label>Valor (R$)</label><input type="number" id="fx-valor" placeholder="0,00" step="0.01" inputmode="decimal" value="${editingFixo?editingFixo.valor:''}"></div><div class="fg"><label>Carteira</label><select id="fx-carteira">${CARTEIRAS.map(c=>`<option value="${c.id}" ${editingFixo&&editingFixo.carteira===c.id?'selected':''}>${c.emoji} ${c.nome.split(' ')[0]}</option>`).join('')}</select></div></div><div class="frow"><div class="fg"><label>Categoria</label><select id="fx-cat"><option value="">Selecione…</option>${CATS.map(c=>`<option ${editingFixo&&editingFixo.cat===c?'selected':''}>${c}</option>`).join('')}</select></div><div class="fg"><label>Forma</label><select id="fx-forma">${FORMAS.map(f=>`<option ${editingFixo&&editingFixo.forma===f?'selected':''}>${f}</option>`).join('')}</select></div></div><button class="btn-add" onclick="editingFixo?salvarEditFixo():addFixo()">${editingFixo?'💾 Salvar Alterações':'📌 Adicionar Fixo'}</button></div></div>`+CARTEIRAS.map(c=>{const fixos=db.fixos.filter(f=>f.carteira===c.id);return`<div class="box"><div class="box-hdr" style="color:${c.cor}">${c.emoji} ${c.nome}</div>${fixos.length?fixos.map(f=>{const jaLancou=f.lancados&&f.lancados.includes(CUR_MONTH);return`<div class="item-row"><div style="flex:1;min-width:0"><div class="item-desc">${!f.ativo?'⏸ ':''}${f.desc}</div><div class="item-meta">${f.cat||'—'} · ${f.forma}</div></div>${badge(f.tipo)}<div class="item-val" style="color:${f.tipo==='Receita'?'var(--green)':'var(--red)'}">${f.tipo==='Receita'?'+':'-'}${brl(f.valor)}</div><button class="btn-launch" onclick="lancarFixo(${f.id})" ${jaLancou?'disabled':''}>${jaLancou?'✅':'Lançar'}</button><button class="btn-edit" onclick="editarFixo(${f.id})">✏️</button><button class="toggle ${f.ativo?'on':'off'}" onclick="toggleFixo(${f.id})"></button><button class="btn-del" onclick="delFixo(${f.id})">🗑</button></div>`;}).join(''):'<div class="empty" style="padding:14px">Nenhum fixo. Adicione acima!</div>'}</div>`;}).join('');}
function addFixo(){const desc=document.getElementById('fx-desc')?.value?.trim();const valor=parseFloat(document.getElementById('fx-valor')?.value);const cartId=document.getElementById('fx-carteira')?.value;const cat=document.getElementById('fx-cat')?.value;const forma=document.getElementById('fx-forma')?.value;if(!desc||!valor||valor<=0){toast('⚠️ Preencha descrição e valor!','#f39c12');return;}db.fixos.push({id:Date.now(),desc,valor,tipo:tipoState.fx,cat,forma,carteira:cartId,ativo:true,lancados:[]});save();toast('📌 Fixo adicionado!');render();}
function editarFixo(id){editingFixo=db.fixos.find(f=>f.id===id);if(editingFixo){tipoState.fx=editingFixo.tipo;render();window.scrollTo(0,0);}}
function cancelarEditFixo(){editingFixo=null;render();}
function salvarEditFixo(){if(!editingFixo)return;const desc=document.getElementById('fx-desc')?.value?.trim();const valor=parseFloat(document.getElementById('fx-valor')?.value);const cartId=document.getElementById('fx-carteira')?.value;const cat=document.getElementById('fx-cat')?.value;const forma=document.getElementById('fx-forma')?.value;if(!desc||!valor||valor<=0){toast('⚠️ Preencha descrição e valor!','#f39c12');return;}const f=db.fixos.find(x=>x.id===editingFixo.id);if(f)Object.assign(f,{desc,valor,tipo:tipoState.fx,cat,forma,carteira:cartId});editingFixo=null;save();toast('💾 Fixo atualizado!');render();}
function lancarFixo(id){const f=db.fixos.find(x=>x.id===id);if(!f)return;if(!f.lancados)f.lancados=[];if(f.lancados.includes(CUR_MONTH)){toast('⚠️ Já lançado este mês!','#f39c12');return;}db[f.carteira].push({id:Date.now(),data:TODAY,desc:f.desc,valor:f.valor,tipo:f.tipo,cat:f.cat,forma:f.forma,carteira:f.carteira});f.lancados.push(CUR_MONTH);save();toast('✅ '+f.desc+' lançado!');render();}
function lancarTodos(){const pend=db.fixos.filter(f=>f.ativo&&(!f.lancados||!f.lancados.includes(CUR_MONTH)));if(!pend.length){toast('✅ Nada pendente!');return;}pend.forEach(f=>{if(!f.lancados)f.lancados=[];db[f.carteira].push({id:Date.now()+Math.random(),data:TODAY,desc:f.desc,valor:f.valor,tipo:f.tipo,cat:f.cat,forma:f.forma,carteira:f.carteira});f.lancados.push(CUR_MONTH);});save();toast('⚡ '+pend.length+' fixos lançados!');render();}
function toggleFixo(id){const f=db.fixos.find(x=>x.id===id);if(f){f.ativo=!f.ativo;save();render();}}
function delFixo(id){if(confirm('Excluir este fixo?')){db.fixos=db.fixos.filter(x=>x.id!==id);save();render();}}
function renderLancamentos(){const cartId=carteira==='all'?'pessoal':carteira;const pfxMap={pessoal:'p',impacto:'i',mandacaru:'m'};const pfx=pfxMap[cartId]||'p';const data=db[cartId]||[];const filtro=filtroMes[pfx];const rec=data.filter(i=>i.tipo==='Receita').reduce((s,i)=>s+i.valor,0);const desp=data.filter(i=>i.tipo==='Despesa').reduce((s,i)=>s+i.valor,0);const saldo=rec-desp;const rows=[...data].sort((a,b)=>b.data.localeCompare(a.data)).filter(i=>!filtro||i.data.startsWith(filtro));const cartInfo=getCart(cartId);const exs={pessoal:['Paguei 200 reais mercado','Recebi salário 5 mil','Gastei 80 reais gasolina'],impacto:['Recebi 10 mil de serviços','Paguei fornecedor 3 mil','Folha pagamento 5000'],mandacaru:['Venda 8 mil produtos','Paguei imposto 1500','Recebi comissão 2000']};return`<div style="margin-bottom:10px;padding:8px 12px;background:var(--surf);border:1px solid var(--bord);border-radius:10px;font-size:13px;font-weight:800;color:${cartInfo.cor}">${cartInfo.emoji} ${cartInfo.nome}${carteira==='all'?'<span style="font-size:10px;color:var(--muted2);font-weight:400;margin-left:6px">← selecione uma carteira acima</span>':''}</div><div class="cards">${card('Receita',brl(rec),'green')}${card('Despesa',brl(desp),'red')}${card('Saldo',brl(saldo),saldo>=0?'green':'red')}${card('Economia',rec>0?((saldo/rec)*100).toFixed(1)+'%':'0%','orange')}</div><div class="voz-box"><div class="voz-title">🎙️ Lançamento por Voz</div><div style="font-size:11px;color:var(--muted2);margin-bottom:8px">📱 Toque no campo → use 🎤 do teclado → fale → toque 🤖</div><div class="voz-row"><textarea class="voz-input" id="voz-txt-${pfx}" rows="2" placeholder="Digite ou use ditado (🎤)…" oninput="vozTexto['${pfx}']=this.value">${vozTexto[pfx]||''}</textarea><button class="voz-btn${vozPending[pfx+'_t']?' thinking':''}" onclick="interpretarVoz('${pfx}','${cartId}')">${vozPending[pfx+'_t']?'⏳':'🤖'}</button></div><div class="voz-exs">${(exs[cartId]||exs.pessoal).map(ex=>`<span class="voz-ex" onclick="usarExemplo('${pfx}','${cartId}','${ex.replace(/'/g,"\\'")}')">💬 ${ex}</span>`).join('')}</div>${vozPending[pfx]?`<div class="voz-prev"><div class="voz-prev-title">✨ Confirme:</div><div class="voz-flds"><div class="vf"><strong>Tipo</strong><span style="color:${vozPending[pfx].tipo==='Receita'?'var(--green)':'var(--red)'}">${vozPending[pfx].tipo}</span></div><div class="vf"><strong>Descrição</strong><span>${vozPending[pfx].desc}</span></div><div class="vf"><strong>Valor</strong><span style="font-family:monospace">${brl(vozPending[pfx].valor)}</span></div><div class="vf"><strong>Categoria</strong><span>${vozPending[pfx].cat}</span></div><div class="vf"><strong>Forma</strong><span>${vozPending[pfx].forma}</span></div></div><div class="voz-acts"><button class="btn-sm btn-ok" onclick="confirmarVoz('${pfx}','${cartId}')">✅ Confirmar</button><button class="btn-sm btn-cancel2" onclick="cancelarVoz('${pfx}')">✖</button></div></div>`:''}</div><div class="box" style="margin-bottom:12px"><div class="box-hdr">➕ Novo Lançamento</div><div class="box-body"><div class="tipo-row"><button id="btn-rec-${pfx}" class="tipo-btn tipo-rec" onclick="setTipoForm('${pfx}','Receita')">✅ Receita</button><button id="btn-desp-${pfx}" class="tipo-btn tipo-off" onclick="setTipoForm('${pfx}','Despesa')">❌ Despesa</button></div><div class="frow"><div class="fg"><label>Data</label><input type="date" id="f-data-${pfx}" value="${TODAY}"></div><div class="fg"><label>Valor (R$)</label><input type="number" id="f-valor-${pfx}" placeholder="0,00" step="0.01" inputmode="decimal"></div></div><div class="frow full"><div class="fg"><label>Descrição</label><input type="text" id="f-desc-${pfx}" placeholder="Descrição…"></div></div><div class="frow"><div class="fg"><label>Categoria</label><select id="f-cat-${pfx}"><option value="">Selecione…</option>${CATS.map(c=>`<option>${c}</option>`).join('')}</select></div><div class="fg"><label>Forma</label><select id="f-forma-${pfx}">${FORMAS.map(f=>`<option>${f}</option>`).join('')}</select></div></div><button class="btn-add" onclick="addManual('${cartId}','${pfx}')">➕ Adicionar</button></div></div><div class="box"><div class="box-hdr">📋 Lançamentos<input type="month" class="month-filter" value="${filtro}" onchange="filtroMes['${pfx}']=this.value;render()"></div>${rows.length?rows.map(i=>`<div class="item-row"><div style="flex:1;min-width:0"><div class="item-desc">${i.desc}</div><div class="item-meta">${i.data.split('-').reverse().join('/')} · ${i.cat||'—'}</div></div>${badge(i.tipo)}<div class="item-val" style="color:${i.tipo==='Receita'?'var(--green)':'var(--red)'}">${i.tipo==='Receita'?'+':'-'}${brl(i.valor)}</div><button class="btn-del" onclick="del('${cartId}',${i.id})">🗑</button></div>`).join(''):'<div class="empty"><div class="empty-icon">📭</div>Sem lançamentos.</div>'}</div>`;}
function renderRelatorios(){const cartsToShow=carteira==='all'?CARTEIRAS:[getCart(carteira)];return cartsToShow.map(c=>`<div class="box"><div class="box-hdr" style="color:${c.cor}">${c.emoji} ${c.nome} – ${YEAR}</div><div class="box-body">${barChart(db[c.id]||[])}</div></div><div class="box"><div class="box-hdr">🏷️ Despesas ${c.nome.split(' ')[0]}</div><div class="box-body">${catBars(db[c.id]||[])}</div></div>`).join('');}
function setTipoForm(pfx,tipo){tipoState[pfx]=tipo;const rec=document.getElementById('btn-rec-'+pfx);const desp=document.getElementById('btn-desp-'+pfx);if(!rec||!desp)return;rec.className='tipo-btn '+(tipo==='Receita'?'tipo-rec':'tipo-off');desp.className='tipo-btn '+(tipo==='Despesa'?'tipo-desp':'tipo-off');}
function addManual(cartId,pfx){const data=document.getElementById('f-data-'+pfx)?.value;const valor=parseFloat(document.getElementById('f-valor-'+pfx)?.value);const desc=document.getElementById('f-desc-'+pfx)?.value?.trim();const cat=document.getElementById('f-cat-'+pfx)?.value;const forma=document.getElementById('f-forma-'+pfx)?.value;if(!data||!desc||!valor||valor<=0){toast('⚠️ Preencha data, descrição e valor!','#f39c12');return;}db[cartId].push({id:Date.now(),data,desc,valor,tipo:tipoState[pfx],cat,forma,carteira:cartId});save();toast('✅ Lançamento adicionado!');render();}
function del(cartId,id){db[cartId]=db[cartId].filter(i=>i.id!==id);save();render();}
function usarExemplo(pfx,cartId,texto){vozTexto[pfx]=texto;render();setTimeout(()=>interpretarVoz(pfx,cartId),100);}
async function interpretarVoz(pfx,cartId){const texto=vozTexto[pfx]||'';if(!texto.trim()){toast('⚠️ Escreva algo primeiro','#f39c12');return;}const prompt='Assistente financeiro. Usuário disse: "'+texto+'"\nResponda APENAS JSON:\n{"tipo":"Receita ou Despesa","desc":"descrição curta","valor":número,"cat":"de: '+CATS.join(', ')+'","forma":"PIX/Dinheiro/Cartão Débito/Cartão Crédito/Boleto/Outros","data":"'+TODAY+'"}\nReceber/salário/venda=Receita; pagar/gastar=Despesa. mil=1000.';vozPending[pfx+'_t']=true;render();try{const res=await fetch('https://api.anthropic.com/v1/messages',{method:'POST',headers:{'Content-Type':'application/json'},body:JSON.stringify({model:'claude-sonnet-4-20250514',max_tokens:300,messages:[{role:'user',content:prompt}]})});const d=await res.json();const txt=d.content?.[0]?.text||'';vozPending[pfx]=JSON.parse(txt.replace(/```json|```|\n/g,'').trim());}catch(e){toast('⚠️ Erro. Tente novamente.','#e74c3c');}vozPending[pfx+'_t']=false;render();}
function confirmarVoz(pfx,cartId){const d=vozPending[pfx];if(!d)return;db[cartId].push({id:Date.now(),data:d.data||TODAY,desc:d.desc,valor:Number(d.valor),tipo:d.tipo,cat:d.cat,forma:d.forma,carteira:cartId});save();delete vozPending[pfx];vozTexto[pfx]='';toast('✅ Lançamento adicionado!');render();}
function cancelarVoz(pfx){delete vozPending[pfx];vozTexto[pfx]='';render();}
function toast(msg,color='var(--green)'){const t=document.getElementById('toast');t.textContent=msg;t.style.background=color;t.classList.add('show');setTimeout(()=>t.classList.remove('show'),2500);}
document.getElementById('topbar-year').textContent=YEAR;
go('dashboard');
</script>
</body>
</html>
