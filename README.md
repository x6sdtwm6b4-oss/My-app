<!DOCTYPE html>

<html lang="ar" dir="rtl">
<head>
<meta charset="UTF-8"/>
<meta name="viewport" content="width=device-width,initial-scale=1.0,viewport-fit=cover,user-scalable=no"/>
<meta name="apple-mobile-web-app-capable" content="yes"/>
<meta name="apple-mobile-web-app-status-bar-style" content="default"/>
<meta name="apple-mobile-web-app-title" content="تقويم الورديات"/>
<meta name="mobile-web-app-capable" content="yes"/>
<meta name="theme-color" content="#f0f4ff"/>
<link rel="apple-touch-icon" href="icons/apple-touch-icon.png"/>
<link rel="icon" type="image/png" sizes="192x192" href="icons/icon-192.png"/>
<link rel="manifest" href="manifest.json"/>
<title>تقويم الورديات</title>
<style>
/* ── Cairo font embedded as base64 subset (Arabic + Latin) ── */
@font-face{font-family:'Cairo';font-weight:400 900;font-style:normal;font-display:swap;
  src:local('Cairo'),local('Cairo-Regular'),
      url('data:font/woff2;base64,') format('woff2');}
*{box-sizing:border-box;margin:0;padding:0;-webkit-tap-highlight-color:transparent;}
:root{
  --safe-t:env(safe-area-inset-top,0px);
  --safe-b:env(safe-area-inset-bottom,0px);
  --bg:#f0f4ff;--white:#ffffff;--text:#1a2035;--muted:#64748b;--dim:#94a3b8;
  --border:#e2ecff;--shadow:0 2px 8px rgba(59,130,246,.08);
}
html,body{height:100%;height:100dvh;overflow:hidden;background:var(--bg);
  font-family:'Cairo',-apple-system,'Segoe UI',Tahoma,sans-serif;
  direction:rtl;color:var(--text);overscroll-behavior:none;-webkit-overflow-scrolling:touch;}
button,input,select,textarea{font-family:inherit;}
::-webkit-scrollbar{display:none;}

#app{display:flex;flex-direction:column;height:100%;height:100dvh;
padding-top:var(–safe-t);padding-bottom:var(–safe-b);max-width:430px;margin:0 auto;}

/* ── HEADER ── */
#header{background:var(–white);border-bottom:1px solid var(–border);
padding:10px 12px 8px;flex-shrink:0;box-shadow:var(–shadow);}
.hrow{display:flex;align-items:center;gap:6px;margin-bottom:6px;}
.hrow:last-child{margin-bottom:0;}
.scroll-x{display:flex;gap:4px;overflow-x:auto;-webkit-overflow-scrolling:touch;}
.scroll-x::-webkit-scrollbar{display:none;}
.logo-box{width:32px;height:32px;border-radius:10px;display:flex;align-items:center;
justify-content:center;font-size:15px;flex-shrink:0;}
.logo-title{font-weight:900;font-size:14px;color:var(–text);}
.logo-sub{font-size:9px;color:var(–dim);margin-top:1px;}
.sec-btn,.grp-btn,.team-btn{border:none;border-radius:9px;padding:5px 13px;cursor:pointer;
flex-shrink:0;font-family:inherit;font-weight:800;font-size:12px;transition:all .2s;}
.grp-btn{border-radius:8px;padding:4px 10px;font-size:11px;background:transparent;
border:1.5px solid var(–border);color:var(–dim);}
.grp-btn.active{font-weight:700;}
.team-btn{border-radius:8px;padding:4px 8px;font-size:11px;background:transparent;
border:1.5px solid var(–border);color:var(–dim);}
.team-btn.active{font-weight:800;}
.team-grp{display:flex;flex-shrink:0;position:relative;}
.members-icon{background:transparent;border-radius:0 8px 8px 0;padding:4px 6px;cursor:pointer;font-size:10px;}
.popup{position:absolute;top:calc(100% + 4px);right:0;z-index:300;background:var(–white);
border-radius:14px;padding:12px 14px;box-shadow:0 10px 32px rgba(0,0,0,.15);
border:1px solid var(–border);min-width:160px;display:none;}
.popup.open{display:block;}
.popup-title{font-weight:700;font-size:12px;margin-bottom:7px;padding-bottom:5px;border-bottom:1px solid var(–bg);}
.popup-member{display:flex;align-items:center;gap:8px;padding:5px 0;border-bottom:1px solid var(–bg);}
.popup-member:last-of-type{border-bottom:none;}
.popup-avatar{width:24px;height:24px;border-radius:50%;display:flex;align-items:center;
justify-content:center;font-size:10px;font-weight:800;flex-shrink:0;}
.popup-count{font-size:10px;color:var(–dim);text-align:center;padding-top:6px;border-top:1px solid var(–bg);}
.nav-btns{display:flex;align-items:center;gap:4px;}
.nav-btn{background:#f8faff;border:1.5px solid var(–border);border-radius:9px;
width:30px;height:30px;cursor:pointer;display:flex;align-items:center;
justify-content:center;font-size:16px;color:var(–muted);}
.nav-btn.today{background:#eff6ff;border-color:#bfdbfe;color:#3b82f6;font-size:13px;}
.month-label{text-align:center;min-width:80px;cursor:pointer;border-radius:8px;
padding:2px 4px;transition:background .2s;}
.month-label.range-on{background:#eff6ff;border:1px solid #bfdbfe;}
.month-label .ml{font-weight:800;font-size:13px;}
.month-label .my{font-size:9px;color:var(–dim);}

/* ── CONTENT ── */
#content{flex:1;overflow-y:auto;overflow-x:hidden;-webkit-overflow-scrolling:touch;
overscroll-behavior-y:contain;}

/* ── PAINT BAR ── */
.paint-bar{display:flex;gap:4px;padding:3px 10px 4px;overflow-x:auto;align-items:center;}
.paint-bar::-webkit-scrollbar{display:none;}
.paint-btn,.shift-pick{border-radius:8px;padding:4px 9px;cursor:pointer;
font-family:inherit;font-weight:700;font-size:10px;flex-shrink:0;border:1.5px solid;}
.paint-btn{background:white;border-color:var(–border);color:var(–muted);}
.paint-btn.on{background:#1e3a5f;border-color:#3b82f6;color:#60a5fa;}
.day-adj{width:24px;height:24px;border-radius:7px;cursor:pointer;font-weight:800;
font-size:13px;flex-shrink:0;display:flex;align-items:center;justify-content:center;}
.share-btn{background:#f0fdf4;border:1.5px solid #86efac;border-radius:8px;padding:4px 9px;
cursor:pointer;color:#16a34a;font-weight:700;font-size:10px;flex-shrink:0;}
.hint{color:var(–dim);font-size:10px;flex-shrink:0;}

/* ── HIJRI + STATS ── */
.hs-bar{padding:1px 10px 3px;display:flex;align-items:center;justify-content:space-between;gap:4px;}
.hs-hijri{font-size:9px;color:var(–dim);flex-shrink:0;}
.hs-chips{display:flex;gap:3px;overflow-x:auto;}
.stat-chip{flex-shrink:0;border-radius:5px;padding:1px 5px;border:1px solid;
display:flex;align-items:center;gap:2px;}
.stat-chip .se{font-size:9px;}
.stat-chip .sn{font-weight:800;font-size:10px;}

/* ── RANGE PANEL ── */
.range-panel{margin:3px 8px 3px;background:var(–white);border-radius:12px;
padding:10px 12px;box-shadow:var(–shadow);border:1.5px solid #bfdbfe;}
.range-title{font-weight:700;font-size:12px;color:#3b82f6;margin-bottom:8px;}
.range-grid{display:grid;grid-template-columns:1fr 1fr;gap:8px;margin-bottom:7px;}
.range-label{font-size:10px;color:var(–muted);font-weight:600;margin-bottom:4px;}
.range-hijri{font-size:9px;color:var(–dim);text-align:center;margin-top:3px;}
.range-stats{display:flex;gap:5px;flex-wrap:wrap;}
.range-days{font-size:10px;color:var(–dim);margin-bottom:5px;}
.rstat{border-radius:8px;padding:4px 10px;border:1px solid;display:flex;align-items:center;gap:4px;}

/* ── CALENDAR ── */
.cal-card{background:var(–white);margin:2px 8px 6px;border-radius:16px;
padding:10px 8px;box-shadow:var(–shadow);}
.day-headers{display:grid;grid-template-columns:repeat(7,1fr);gap:2px;margin-bottom:4px;}
.dh{text-align:center;font-size:10px;font-weight:700;color:var(–dim);padding:2px 0;}
.days-grid{display:grid;grid-template-columns:repeat(7,1fr);gap:2px;}
.day-cell{aspect-ratio:1;border-radius:8px;display:flex;align-items:center;
justify-content:center;flex-direction:column;gap:1px;cursor:pointer;
position:relative;border:1px solid;user-select:none;-webkit-user-select:none;
touch-action:none;transition:transform .08s;}
.day-cell:active{transform:scale(.85)!important;opacity:.8;}
.day-cell.today{border-width:2px;transform:scale(1.06);}
.day-cell.hl{outline:2.5px solid #f59e0b;outline-offset:0;}
.day-cell.ov{border-style:dashed;}
.dc-num{font-size:11px;font-weight:600;line-height:1;}
.dc-num.bold{font-weight:900;}
.dc-emo{font-size:8px;line-height:1;}
.note-dot{position:absolute;bottom:1px;left:50%;transform:translateX(-50%);
width:3px;height:3px;border-radius:50%;background:#f59e0b;}
.ov-dot{position:absolute;top:1px;right:1px;width:3px;height:3px;border-radius:50%;}

/* ── LEGEND ── */
.legend{display:flex;gap:4px;flex-wrap:wrap;padding:0 8px 7px;justify-content:center;}
.leg{background:var(–bg);border-radius:6px;padding:3px 7px;border:1px solid;
display:flex;align-items:center;gap:2px;}
.leg-txt{font-size:9px;font-weight:700;}

/* ── NOTES PANEL ── */
.notes-panel{margin:0 8px 10px;border-radius:16px;border:2px solid;background:var(–white);
padding:16px;min-height:130px;}
.np-header{display:flex;align-items:center;gap:6px;margin-bottom:10px;}
.np-title{font-weight:700;font-size:12px;color:#3b82f6;}
.np-count{background:#3b82f6;color:white;border-radius:50%;width:18px;height:18px;
display:flex;align-items:center;justify-content:center;font-size:10px;font-weight:800;}
.np-empty{text-align:center;color:#c7d2e0;font-size:11px;padding-top:4px;}
.np-row{display:flex;align-items:flex-start;gap:10px;padding:10px 0;
border-bottom:1px solid #f0f4ff;cursor:pointer;}
.np-row:last-child{border-bottom:none;}
.np-day-box{flex-shrink:0;text-align:center;border-radius:10px;padding:5px 8px;min-width:46px;}
.np-day-num{font-weight:800;font-size:17px;line-height:1;}
.np-day-emo{font-size:9px;}
.np-body{flex:1;min-width:0;}
.np-hijri{font-size:9px;color:var(–dim);margin-bottom:2px;}
.np-text{font-size:14px;color:var(–text);line-height:1.7;overflow:hidden;
display:-webkit-box;-webkit-line-clamp:4;-webkit-box-orient:vertical;}
.np-arrow{flex-shrink:0;color:var(–dim);font-size:14px;}

/* ── TABS CONTENT ── */
.tab-page{padding:12px 10px;}
.tab-title{font-weight:800;font-size:15px;color:var(–text);margin-bottom:10px;}
.card{background:var(–white);border-radius:13px;padding:12px;
margin-bottom:10px;box-shadow:var(–shadow);}
.fi{background:#f8faff;border:1.5px solid var(–border);border-radius:10px;
padding:9px 12px;font-size:13px;color:var(–text);outline:none;width:100%;
-webkit-appearance:none;}
.fi:focus{border-color:#3b82f6;}
.fi-select{padding:9px 4px;}
.lbl{font-size:11px;color:var(–muted);font-weight:600;margin-bottom:5px;display:block;}
.grid3{display:grid;grid-template-columns:1fr 1fr 1fr;gap:7px;}
.grid2{display:grid;grid-template-columns:1fr 1fr;gap:8px;}
.hijri-hint{color:var(–dim);font-size:10px;margin-top:6px;text-align:center;}
.search-tabs{display:flex;gap:5px;margin-bottom:10px;}
.stab{flex:1;border-radius:10px;padding:9px 0;cursor:pointer;font-weight:700;
font-size:12px;font-family:inherit;transition:all .2s;}
.stab.on{background:#3b82f6;border:1.5px solid #3b82f6;color:white;}
.stab.off{background:white;border:1.5px solid var(–border);color:var(–muted);}
.search-result{background:var(–white);border-radius:10px;padding:9px;margin-bottom:5px;
box-shadow:var(–shadow);cursor:pointer;display:flex;align-items:center;justify-content:space-between;}
.sr-name{font-weight:700;font-size:12px;color:var(–text);}
.sr-sub{font-size:9px;color:var(–dim);}
.tag{display:inline-flex;align-items:center;gap:4px;border-radius:8px;
padding:3px 9px;font-size:11px;font-weight:700;}
.btn-primary{background:linear-gradient(135deg,#3b82f6,#6366f1);border:none;
border-radius:13px;color:white;padding:13px 0;width:100%;cursor:pointer;
font-weight:700;font-size:14px;font-family:inherit;box-shadow:0 4px 14px rgba(59,130,246,.3);}
.btn-neutral{background:#f1f5f9;border:none;border-radius:13px;color:var(–muted);
padding:13px 16px;cursor:pointer;font-family:inherit;font-size:13px;}
.btn-danger{background:#fee2e2;border:none;border-radius:13px;color:#ef4444;
padding:13px 16px;cursor:pointer;font-family:inherit;font-size:13px;}
.act-row{display:flex;gap:8px;margin-top:14px;}

/* Compare */
.filter-row{display:flex;gap:5px;overflow-x:auto;margin-bottom:6px;}
.filter-btn{border:none;border-radius:8px;padding:5px 12px;cursor:pointer;
font-weight:700;font-size:11px;font-family:inherit;flex-shrink:0;}
.shift-filter{border-radius:8px;padding:4px 8px;cursor:pointer;font-weight:700;
font-size:10px;font-family:inherit;flex-shrink:0;display:flex;align-items:center;gap:2px;}
.cmp-block{background:var(–white);border-radius:13px;padding:11px;margin-bottom:8px;
box-shadow:var(–shadow);}
.cmp-header{display:flex;align-items:center;gap:6px;margin-bottom:7px;}
.cmp-badge{border-radius:7px;padding:3px 9px;display:flex;align-items:center;gap:4px;}
.cmp-count{border-radius:50%;width:19px;height:19px;display:flex;align-items:center;
justify-content:center;font-weight:800;font-size:10px;color:white;}
.cmp-teams{display:flex;flex-wrap:wrap;gap:4px;}
.cmp-team{border-radius:8px;padding:4px 8px;}
.cmp-tname{font-size:11px;font-weight:700;}
.cmp-tsub{font-size:9px;color:var(–dim);}
.cover-box{background:linear-gradient(135deg,#eff6ff,#f5f3ff);border-radius:14px;
padding:13px;margin-top:6px;border:1px solid #bfdbfe;}
.cover-title{font-weight:800;font-size:13px;margin-bottom:3px;}
.cover-sub{color:var(–dim);font-size:10px;margin-bottom:10px;}
.cover-shifts{display:flex;gap:4px;flex-wrap:wrap;margin-bottom:9px;}
.cover-item{background:var(–white);border-radius:10px;padding:9px;margin-bottom:5px;
border:1.5px solid #6ee7b7;display:flex;align-items:center;justify-content:space-between;}
.busy-items{display:flex;flex-wrap:wrap;gap:3px;}
.busy-item{border-radius:7px;padding:2px 7px;display:flex;align-items:center;gap:2px;}
.no-avail{background:#fff7ed;border-radius:10px;padding:10px;text-align:center;
border:1px solid #fed7aa;}

/* Settings */
.sec-section{margin-bottom:14px;}
.sec-label-row{display:flex;align-items:center;justify-content:space-between;margin-bottom:7px;}
.sec-bar{width:4px;height:22px;border-radius:2px;flex-shrink:0;}
.sec-name-text{font-weight:800;font-size:14px;}
.grp-card{background:var(–white);border-radius:16px;padding:12px;margin-bottom:10px;
box-shadow:var(–shadow);}
.grp-header{display:flex;align-items:center;justify-content:space-between;
margin-bottom:9px;padding-bottom:9px;border-bottom:1px solid #f0f4ff;}
.grp-icon-box{width:30px;height:30px;border-radius:8px;display:flex;align-items:center;
justify-content:center;font-size:15px;}
.grp-name{font-weight:800;font-size:13px;}
.grp-pat{font-size:10px;color:var(–dim);}
.team-item{background:#f8faff;border-radius:11px;padding:10px;margin-bottom:6px;
display:flex;align-items:center;justify-content:space-between;}
.team-name{font-weight:700;font-size:13px;}
.team-meta{font-size:10px;color:var(–dim);margin-top:1px;}
.btn-row{display:flex;gap:3px;}
.sm-btn{border-radius:7px;padding:4px 8px;cursor:pointer;font-family:inherit;
font-size:10px;font-weight:700;border:none;}
.sm-add{background:#f0fdf4;border:1px solid #86efac;color:#16a34a;}
.sm-edit{background:#eff6ff;border:1px solid #bfdbfe;color:#3b82f6;}
.sm-del{background:#fee2e2;color:#ef4444;}
.save-card{background:var(–white);border-radius:14px;padding:13px;
box-shadow:var(–shadow);border:1px solid var(–border);}
.save-ok{display:flex;align-items:center;justify-content:center;gap:5px;
background:#ecfdf5;border:1px solid #6ee7b7;border-radius:10px;
padding:8px;margin-bottom:10px;}
.save-ok-text{font-weight:700;font-size:12px;color:#059669;}

/* ── PATTERN BUILDER ── */
.pat-pills{display:flex;gap:5px;flex-wrap:wrap;margin-bottom:8px;min-height:36px;}
.pat-pill{display:flex;align-items:center;gap:2px;border-radius:9px;
padding:4px 7px;border:1.5px solid;}
.pat-idx{font-size:9px;border-radius:4px;padding:0 4px;margin-right:1px;}
.pat-mv{background:none;border:none;cursor:pointer;font-size:11px;padding:0 1px;line-height:1;}
.pat-rm{border:none;border-radius:4px;background:#fee2e2;color:#ef4444;
width:15px;height:15px;cursor:pointer;font-size:9px;display:flex;
align-items:center;justify-content:center;line-height:1;}
.pat-add-btns{display:flex;gap:4px;flex-wrap:wrap;}
.pat-add{border-radius:8px;padding:4px 8px;cursor:pointer;font-weight:700;
font-size:10px;font-family:inherit;display:flex;align-items:center;gap:2px;}
.pat-hint{color:var(–dim);font-size:10px;margin-top:6px;}
.color-grid{display:flex;gap:5px;flex-wrap:wrap;}
.color-dot{width:26px;height:26px;border-radius:50%;cursor:pointer;border:3px solid transparent;}
.icon-grid{display:flex;gap:5px;flex-wrap:wrap;}
.icon-btn2{width:34px;height:34px;border-radius:9px;font-size:18px;cursor:pointer;
background:#f8faff;border:1.5px solid var(–border);}
.icon-btn2.active{background:#eff6ff;border-color:#3b82f6;}

/* ── BOTTOM NAV ── */
#bottom-nav{background:var(–white);border-top:1px solid var(–border);
display:flex;flex-shrink:0;box-shadow:0 -2px 8px rgba(59,130,246,.06);}
.nav-tab{flex:1;background:transparent;border:none;padding:9px 0 7px;cursor:pointer;
display:flex;flex-direction:column;align-items:center;gap:2px;transition:all .15s;}
.nav-tab .nt-ico{font-size:18px;filter:grayscale(1);opacity:.5;transition:all .15s;}
.nav-tab .nt-lbl{font-size:9px;font-weight:500;color:var(–dim);transition:all .15s;}
.nav-tab.active .nt-ico{font-size:20px;filter:none;opacity:1;}
.nav-tab.active .nt-lbl{font-weight:800;}

/* ── MODAL ── */
.modal-overlay{position:fixed;inset:0;z-index:500;background:rgba(0,0,0,.5);
backdrop-filter:blur(6px);-webkit-backdrop-filter:blur(6px);
display:flex;align-items:flex-end;justify-content:center;}
.modal-sheet{background:var(–white);border-radius:22px 22px 0 0;
width:100%;max-width:430px;max-height:92dvh;display:flex;flex-direction:column;
animation:slideUp .25s cubic-bezier(.32,1.1,.5,1);}
@keyframes slideUp{from{transform:translateY(60px);opacity:0;}to{transform:translateY(0);opacity:1;}}
.modal-handle-area{padding:14px 18px 10px;border-bottom:1px solid #f0f4ff;flex-shrink:0;}
.modal-handle{width:36px;height:4px;background:#e2e8f0;border-radius:2px;margin:0 auto 12px;}
.modal-title{font-weight:800;font-size:16px;}
.modal-body{overflow-y:auto;flex:1;padding:14px 18px 32px;}
.member-row{display:flex;gap:6px;margin-bottom:5px;}
.member-input{flex:1;}
.date-display{background:#f8faff;border:1.5px solid var(–border);border-radius:10px;
padding:9px 12px;display:flex;align-items:center;justify-content:space-between;
font-size:13px;font-weight:600;}

/* ── ANIMATIONS ── */
.fade-in{animation:fadeIn .15s ease;}
@keyframes fadeIn{from{opacity:0;transform:translateY(4px);}to{opacity:1;transform:none;}}
</style>

</head>
<body>
<div id="app">
  <div id="header">
    <!-- JS fills this -->
  </div>
  <div id="content">
    <!-- JS fills this -->
  </div>
  <nav id="bottom-nav">
    <!-- JS fills this -->
  </nav>
</div>

<!-- No external JS dependencies! Everything is inline below -->

<script>
'use strict';
/* ════════════════════════════════════════════
   CONSTANTS
════════════════════════════════════════════ */
const MA=["يناير","فبراير","مارس","أبريل","مايو","يونيو","يوليو","أغسطس","سبتمبر","أكتوبر","نوفمبر","ديسمبر"];
const DS=["أح","إث","ثل","أر","خم","جم","سب"];
const HM=["محرم","صفر","ربيع الأول","ربيع الآخر","جمادى الأولى","جمادى الآخرة","رجب","شعبان","رمضان","شوال","ذو القعدة","ذو الحجة"];
const SHIFTS={
  day:    {label:"نهار",    short:"نهار", e:"☀️",c:"#f97316",l:"#fff7ed",d:"#431407",b:"#fed7aa"},
  work1:  {label:"أول ليل",short:"أول",  e:"🌅",c:"#3b82f6",l:"#dbeafe",d:"#1e3a5f",b:"#93c5fd"},
  work2:  {label:"آخر ليل",short:"آخر",  e:"🌙",c:"#8b5cf6",l:"#ede9fe",d:"#2e1a5e",b:"#c4b5fd"},
  off:    {label:"راحة",   short:"راحة", e:"💤",c:"#f59e0b",l:"#fef3c7",d:"#3d2a00",b:"#fcd34d"},
  vacation:{label:"إجازة", short:"إجازة",e:"🌴",c:"#10b981",l:"#d1fae5",d:"#063a25",b:"#6ee7b7"},
};
const SK=Object.keys(SHIFTS);
const PAL=["#3b82f6","#ec4899","#14b8a6","#f97316","#8b5cf6","#eab308","#06b6d4","#10b981","#6366f1","#ef4444"];
const ICONS=["🏠","🔄","🌟","🏢","🎯","⚡","🔥","💡","🚀","🌊","🎪","🏋️"];

let _uid=300;
const uid=()=>`u${++_uid}`;
const getDays=(y,m)=>new Date(y,m+1,0).getDate();
const getFirst=(y,m)=>new Date(y,m,1).getDay();
const TODAY=new Date().toISOString().slice(0,10);

function fmtDate(iso){if(!iso)return"";const[y,m,d]=iso.split("-");return`${d}/${m}/${y}`;}

function toHijri(gY,gM,gD){
  const a=Math.floor((14-gM)/12),y=gY+4800-a,m=gM+12*a-3;
  let jdn=gD+Math.floor((153*m+2)/5)+365*y+Math.floor(y/4)-Math.floor(y/100)+Math.floor(y/400)-32045;
  jdn-=10; // Umm al-Qura calibration
  const L=jdn-1948440+10632,N=Math.floor((L-1)/10631),L2=L-10631*N+354;
  const J=Math.floor((10985-L2)/5316)*Math.floor((50*L2)/17719)+Math.floor(L2/5670)*Math.floor((43*L2)/15238);
  const L3=L2-Math.floor((30-J)/15)*Math.floor((17719*J)/50)-Math.floor(J/16)*Math.floor((15238*J)/43)+29;
  const hm=Math.floor((24*L3)/709),hd=L3-Math.floor((709*hm)/24),hy=30*N+J-30;
  return{y:hy,m:hm,d:hd};
}
function hToG(hY,hM,hD){
  const j=Math.floor((11*hY+3)/30)+354*hY+30*hM-Math.floor((hM-1)/2)+hD+1948440-385;
  const l=j+68569,n=Math.floor((4*l)/146097),l2=l-Math.floor((146097*n+3)/4),i=Math.floor((4000*(l2+1))/1461001);
  const l3=l2-Math.floor((1461*i)/4)+31,jj=Math.floor((80*l3)/2447);
  const d=l3-Math.floor((2447*jj)/80),mm=jj+2-12*Math.floor(jj/11),yy=100*(n-49)+i+Math.floor(jj/11);
  return{y:yy,m:mm,d};
}

function buildSched(year,pattern,startDate){
  if(!pattern||!pattern.length)return{};
  const pl=pattern.length;
  const diff=Math.floor((new Date(year,0,1)-new Date(startDate||new Date(year,0,1)))/86400000);
  let idx=((diff%pl)+pl)%pl;
  const sc={};
  for(let m=0;m<12;m++){const d=getDays(year,m);for(let i=1;i<=d;i++){sc[`${m}-${i}`]=pattern[idx%pl];idx++;}}
  return sc;
}

/* ════════════════════════════════════════════
   DATA
════════════════════════════════════════════ */
const INIT_SECTORS=[
  {id:"sec_day",label:"نهار",emoji:"☀️",color:"#f97316",
   groups:[
    {id:"sg_d_g",label:"الغرفة",icon:"🏠",color:"#f97316",pattern:["day","day","off","vacation"],
     teams:[{id:"td1",name:"المجموعة 1",color:"#f97316",members:["منصور","طارق"]},
            {id:"td2",name:"المجموعة 2",color:"#ec4899",members:["عبدالجبار","محمود","ياسر"]},
            {id:"td3",name:"المجموعة 3",color:"#14b8a6",members:["أحمد النايف","فارس"]},
            {id:"td4",name:"المجموعة 4",color:"#eab308",members:["عمر","فيصل"]}]},
    {id:"sg_d_dw",label:"الدوريات",icon:"🔄",color:"#8b5cf6",pattern:["day","day","off","vacation"],
     teams:[{id:"tdd1",name:"فريق أ",color:"#8b5cf6",members:["عضو 1","عضو 2"]},
            {id:"tdd2",name:"فريق ب",color:"#06b6d4",members:["عضو 3"]}]}]},
  {id:"sec_night",label:"نوبات",emoji:"🌙",color:"#3b82f6",
   groups:[
    {id:"sg_n_g",label:"الغرفة",icon:"🏠",color:"#3b82f6",pattern:["work1","work2","off","vacation"],
     teams:[{id:"tg1",name:"المجموعة 1",color:"#3b82f6",members:["منصور","طارق"]},
            {id:"tg2",name:"المجموعة 2",color:"#ec4899",members:["عبدالجبار","محمود","ياسر"]},
            {id:"tg3",name:"المجموعة 3",color:"#14b8a6",members:["أحمد النايف","فارس"]},
            {id:"tg4",name:"المجموعة 4",color:"#f97316",members:["عمر","فيصل"]}]},
    {id:"sg_n_dw",label:"الدوريات",icon:"🔄",color:"#8b5cf6",pattern:["work1","work1","work2","work2","off","vacation"],
     teams:[{id:"tn1",name:"محمد حسن",  color:"#8b5cf6",members:["محمد حسن"]},
            {id:"tn2",name:"هاني عماري",color:"#06b6d4",members:["هاني عماري"]},
            {id:"tn3",name:"حسن خطار", color:"#eab308",members:["حسن خطار"]}]}]}
];

function initDates(secs){const r={};secs.forEach(s=>s.groups.forEach(g=>g.teams.forEach(t=>{r[t.id]=TODAY;})));return r;}
function allTeams(secs){return secs.flatMap(s=>s.groups.flatMap(g=>g.teams.map(t=>({t,g,s}))));}
function findTeam(secs,tid){for(const s of secs)for(const g of s.groups){const t=g.teams.find(x=>x.id===tid);if(t)return{s,g,t};}return null;}

/* ════════════════════════════════════════════
   STATE
════════════════════════════════════════════ */
function loadSaved(){
  try{const r=localStorage.getItem("wrd_v2");return r?JSON.parse(r):null;}catch(e){return null;}
}
const sv=loadSaved();
const S={
  sectors: sv?.sectors||JSON.parse(JSON.stringify(INIT_SECTORS)),
  teamDates: sv?.teamDates||initDates(INIT_SECTORS),
  overrides: sv?.overrides||{},
  notes: sv?.notes||{},
  selSec: sv?.selSec||"sec_day",
  selGrp: sv?.selGrp||"sg_d_g",
  selTeam: sv?.selTeam||"td1",
  year: sv?.year||new Date().getFullYear(),
  month: sv?.month!==undefined?sv.month:new Date().getMonth(),
  tab: "cal",
  paint: false,
  paintShift: "vacation",
  sModeM: true,
  sMD:"",sMM:"",sMY:"",
  sHD:"",sHM:"",sHY:"",
  sResult: null,
  cDate: TODAY,cDay:new Date().getDate().toString(),cMon:(new Date().getMonth()+1).toString(),cYr:new Date().getFullYear().toString(),
  coverSh:"work1",coverRes:null,
  noteCtx: null,
  rangeMode: false,
  rangeFrom: sv?.rangeFrom||TODAY,
  rangeTo: sv?.rangeTo||TODAY,
  membersPopup: null,
  cmpFilter:"all",
  cmpShiftFilter:"all",
  savedMsg: false,
  modal: null, // {type, ...data}
  highlight: null,
  dragActive: false, dragShift: null,
};

let _saveTimer;
function save(){
  clearTimeout(_saveTimer);
  _saveTimer=setTimeout(()=>{
    try{
      localStorage.setItem("wrd_v2",JSON.stringify({
        sectors:S.sectors,teamDates:S.teamDates,overrides:S.overrides,notes:S.notes,
        selSec:S.selSec,selGrp:S.selGrp,selTeam:S.selTeam,
        year:S.year,month:S.month,rangeFrom:S.rangeFrom,rangeTo:S.rangeTo
      }));
    }catch(e){}
  },300);
}

/* ════════════════════════════════════════════
   SCHEDULE HELPERS
════════════════════════════════════════════ */
function getSched(tid,yr){
  const f=findTeam(S.sectors,tid);if(!f)return{};
  const base=buildSched(yr||S.year,f.g.pattern,S.teamDates[tid]);
  return Object.assign({},base,S.overrides[tid]||{});
}
function getStats(tid){
  const sc=getSched(tid),r={};SK.forEach(k=>r[k]=0);
  Object.values(sc).forEach(v=>{if(v&&r[v]!==undefined)r[v]++;});return r;
}
function getCmpData(){
  if(!S.cDate)return null;
  const[y,m,d]=S.cDate.split("-").map(Number),gM=m-1;
  if(!y||gM<0||gM>11||d<1||d>getDays(y,gM))return null;
  const h=toHijri(y,m,d);
  const rows=allTeams(S.sectors).map(({t,g,s})=>{
    const sc=Object.assign({},buildSched(y,g.pattern,S.teamDates[t.id]),S.overrides[t.id]||{});
    return{t,g,s,type:sc[`${gM}-${d}`]||null};
  });
  const byShift={};rows.forEach(r=>{const k=r.type||"none";(byShift[k]=byShift[k]||[]).push(r);});
  return{h,rows,byShift};
}
function findCover(){
  if(!S.cDate)return;
  const[y,m,d]=S.cDate.split("-").map(Number),gM=m-1;
  const av=[],busy=[];
  allTeams(S.sectors).forEach(({t,g,s})=>{
    const sc=Object.assign({},buildSched(y,g.pattern,S.teamDates[t.id]),S.overrides[t.id]||{});
    const tp=sc[`${gM}-${d}`]||null;
    (tp==="off"||tp==="vacation"||!tp?av:busy).push({t,g,s,type:tp});
  });
  S.coverRes={av,busy};
}

/* ════════════════════════════════════════════
   DOM HELPERS
════════════════════════════════════════════ */
function h(tag,attrs,children){
  const el=document.createElement(tag);
  if(attrs)Object.entries(attrs).forEach(([k,v])=>{
    if(k==="style"&&typeof v==="object")Object.assign(el.style,v);
    else if(k.startsWith("on"))el.addEventListener(k.slice(2).toLowerCase(),v);
    else if(k==="class")el.className=v;
    else if(k==="html")el.innerHTML=v;
    else el.setAttribute(k,v);
  });
  if(children){
    (Array.isArray(children)?children:[children]).forEach(c=>{
      if(c==null||c===false)return;
      el.appendChild(typeof c==="string"?document.createTextNode(c):c);
    });
  }
  return el;
}
function el(sel,attrs,kids){
  // shorthand: h with class selector
  const[tag,...cls]=sel.split(".");
  const a=Object.assign({},attrs);
  if(cls.length)a.class=(a.class?a.class+" ":"")+cls.join(" ");
  return h(tag||"div",a,kids);
}
function txt(str){return document.createTextNode(str);}
function clr(el){while(el.firstChild)el.removeChild(el.firstChild);}

/* ════════════════════════════════════════════
   RENDER
════════════════════════════════════════════ */
function render(){
  renderHeader();
  renderContent();
  renderNav();
}

function curSec(){return S.sectors.find(s=>s.id===S.selSec)||S.sectors[0];}
function curGrp(){const sec=curSec();return sec.groups.find(g=>g.id===S.selGrp)||sec.groups[0];}
function curTeam(){const g=curGrp();return g.teams.find(t=>t.id===S.selTeam)||g.teams[0];}
function accent(){return curTeam()?.color||curGrp()?.color||"#3b82f6";}

/* ── HEADER ── */
function renderHeader(){
  const hdr=document.getElementById("header");clr(hdr);
  const sec=curSec(),grp=curGrp(),team=curTeam(),acc=accent();
  const now=new Date();

  // Row 1: Logo + month nav (only on cal tab)
  const row1=h("div",{class:"hrow"});

  // Logo
  const logo=h("div",{style:{display:"flex",alignItems:"center",gap:"8px",flex:"1"}});
  const lb=h("div",{class:"logo-box",style:{background:`linear-gradient(135deg,${acc},${acc}cc)`,boxShadow:`0 3px 8px ${acc}44`}},["🗓️"]);
  const lt=h("div",{},[
    h("div",{class:"logo-title"},["تقويم الورديات"]),
    h("div",{class:"logo-sub"},[`${sec.emoji}${sec.label} · ${grp.icon}${grp.label} · `,h("span",{style:{color:acc,fontWeight:"700"}},[team?.name||"—"])])
  ]);
  logo.appendChild(lb);logo.appendChild(lt);row1.appendChild(logo);

  // Month nav (cal tab only)
  if(S.tab==="cal"){
    const nav=h("div",{class:"nav-btns"});

    // Today button
    const todayBtn=h("button",{class:"nav-btn today",title:"اليوم",onclick:()=>{
      S.year=now.getFullYear();S.month=now.getMonth();
      S.highlight={m:now.getMonth(),d:now.getDate()};
      save();render();
    }},["●"]);
    nav.appendChild(todayBtn);

    // Prev
    const prev=h("button",{class:"nav-btn",onclick:()=>{
      let m=S.month-1,y=S.year;if(m<0){m=11;y--;}
      S.month=m;S.year=y;S.highlight=null;save();renderContent();renderHeader();
    }},["‹"]);
    nav.appendChild(prev);

    // Month label (clickable for range mode)
    const ml=h("div",{class:"month-label"+(S.rangeMode?" range-on":""),onclick:()=>{
      S.rangeMode=!S.rangeMode;renderContent();renderHeader();
    }});
    ml.appendChild(h("div",{class:"ml",style:{color:S.rangeMode?"#3b82f6":""}},[MA[S.month]]));
    ml.appendChild(h("div",{class:"my",style:{color:S.rangeMode?"#3b82f6":""}},[S.rangeMode?"📅 نطاق":String(S.year)]));
    nav.appendChild(ml);

    // Next
    const next=h("button",{class:"nav-btn",onclick:()=>{
      let m=S.month+1,y=S.year;if(m>11){m=0;y++;}
      S.month=m;S.year=y;S.highlight=null;save();renderContent();renderHeader();
    }},["›"]);
    nav.appendChild(next);

    row1.appendChild(nav);
  }
  hdr.appendChild(row1);

  // Row 2: Sector tabs
  const row2=h("div",{class:"hrow"});
  const secRow=h("div",{class:"scroll-x",style:{flex:"1"}});
  S.sectors.forEach(s=>{
    const btn=h("button",{class:"sec-btn",
      style:{background:S.selSec===s.id?s.color:"#f1f5f9",color:S.selSec===s.id?"white":"#64748b",
             boxShadow:S.selSec===s.id?`0 3px 10px ${s.color}44`:"none"},
      onclick:()=>{
        S.selSec=s.id;S.selGrp=s.groups[0].id;
        S.selTeam=s.groups[0].teams[0]?.id||"";S.tab="cal";
        save();render();
      }},[`${s.emoji} ${s.label}`]);
    secRow.appendChild(btn);
  });
  row2.appendChild(secRow);
  hdr.appendChild(row2);

  // Row 3: Group tabs
  const row3=h("div",{class:"hrow"});
  const grpRow=h("div",{class:"scroll-x",style:{flex:"1"}});
  curSec().groups.forEach(g=>{
    const active=S.selGrp===g.id;
    const btn=h("button",{class:"grp-btn"+(active?" active":""),
      style:{background:active?`${g.color}18`:"transparent",borderColor:active?g.color:"",color:active?g.color:""},
      onclick:()=>{
        S.selGrp=g.id;S.selTeam=g.teams[0]?.id||"";S.tab="cal";
        save();render();
      }},[`${g.icon} ${g.label}`]);
    grpRow.appendChild(btn);
  });
  row3.appendChild(grpRow);
  hdr.appendChild(row3);

  // Row 4: Team tabs with members popup
  const row4=h("div",{class:"hrow"});
  const teamRow=h("div",{class:"scroll-x",style:{flex:"1",position:"relative",overflowX:"visible"}});
  curGrp().teams.forEach(t=>{
    const active=S.selTeam===t.id;
    const grpWrap=h("div",{class:"team-grp"});
    const tBtn=h("button",{class:"team-btn"+(active?" active":""),
      style:{background:active?`${t.color}14`:"transparent",borderColor:active?t.color:"",
             color:active?t.color:"",borderRadius:"8px 0 0 8px",borderLeft:"1.5px solid",
             borderLeftColor:active?t.color:"var(--border)"},
      onclick:()=>{S.selTeam=t.id;S.tab="cal";save();render();}},[t.name]);
    const mBtn=h("button",{class:"members-icon team-btn"+(active?" active":""),
      style:{background:active?`${t.color}14`:"transparent",borderColor:active?t.color:"",color:active?t.color:""},
      onclick:(e)=>{e.stopPropagation();S.membersPopup=S.membersPopup===t.id?null:t.id;renderHeader();}},["👥"]);

    // Popup
    const popup=h("div",{class:"popup"+(S.membersPopup===t.id?" open":"")});
    popup.appendChild(h("div",{class:"popup-title",style:{color:t.color}},[t.name]));
    t.members.forEach((m)=>{
      const mr=h("div",{class:"popup-member"});
      const av=h("div",{class:"popup-avatar",style:{background:`${t.color}22`,border:`1.5px solid ${t.color}66`,color:t.color}},[m[0]||"?"]);
      mr.appendChild(av);
      mr.appendChild(h("span",{style:{fontSize:"13px",fontWeight:"600"}},[m]));
      popup.appendChild(mr);
    });
    popup.appendChild(h("div",{class:"popup-count"},[`${t.members.length} أعضاء`]));

    grpWrap.appendChild(tBtn);grpWrap.appendChild(mBtn);grpWrap.appendChild(popup);
    teamRow.appendChild(grpWrap);
  });
  row4.appendChild(teamRow);
  hdr.appendChild(row4);
}

/* ── CONTENT ── */
function renderContent(){
  const content=document.getElementById("content");clr(content);
  const now=new Date();
  if(S.tab==="cal") renderCal(content,now);
  else if(S.tab==="compare") renderCompare(content);
  else if(S.tab==="search") renderSearch(content);
  else if(S.tab==="settings") renderSettings(content);
}

/* ── CALENDAR ── */
function renderCal(container,now){
  const team=curTeam(),grp=curGrp(),acc=accent();
  const sched=team?getSched(team.id):{};
  const stats=team?getStats(team.id):{};
  const wrap=h("div",{class:"fade-in"});

  // Range panel
  if(S.rangeMode){
    const rp=h("div",{class:"range-panel"});
    rp.appendChild(h("div",{class:"range-title"},["📅 عرض نطاق زمني"]));
    const rg=h("div",{class:"range-grid"});
    ["from","to"].forEach(which=>{
      const val=which==="from"?S.rangeFrom:S.rangeTo;
      const d=h("div",{});
      d.appendChild(h("div",{class:"range-label"},[which==="from"?"📅 من:":"📅 إلى:"]));
      const inp=h("input",{type:"date",class:"fi",value:val,
        style:{direction:"ltr",borderColor:"#93c5fd"},
        onchange:(e)=>{
          if(which==="from")S.rangeFrom=e.target.value;
          else S.rangeTo=e.target.value;
          save();renderContent();
        }});
      d.appendChild(inp);
      if(val){
        const[y,m,dv]=val.split("-").map(Number);
        const hh=toHijri(y,m,dv);
        d.appendChild(h("div",{class:"range-hijri"},[`🌙 ${hh.d} ${HM[hh.m-1]}`]));
      }
      rg.appendChild(d);
    });
    rp.appendChild(rg);
    if(S.rangeFrom&&S.rangeTo&&S.rangeFrom<=S.rangeTo){
      const[fy,fm,fd]=S.rangeFrom.split("-").map(Number);
      const[ty,tm,td]=S.rangeTo.split("-").map(Number);
      const counts={};SK.forEach(k=>counts[k]=0);
      let cur=new Date(fy,fm-1,fd);const end=new Date(ty,tm-1,td);
      while(cur<=end){
        const sc2=Object.assign({},buildSched(cur.getFullYear(),grp.pattern,S.teamDates[team?.id]),S.overrides[team?.id]||{});
        const v=sc2[`${cur.getMonth()}-${cur.getDate()}`];
        if(v&&counts[v]!==undefined)counts[v]++;
        cur.setDate(cur.getDate()+1);
      }
      const days=Math.round((new Date(ty,tm-1,td)-new Date(fy,fm-1,fd))/86400000)+1;
      rp.appendChild(h("div",{class:"range-days"},[`إجمالي ${days} يوم`]));
      const rs=h("div",{class:"range-stats"});
      SK.filter(k=>counts[k]>0).forEach(k=>{
        const sh=SHIFTS[k];
        const chip=h("div",{class:"rstat",style:{background:sh.l,borderColor:sh.b}});
        chip.appendChild(h("span",{style:{fontSize:"12px"}},[sh.e]));
        chip.appendChild(h("span",{style:{fontWeight:"800",fontSize:"13px",color:sh.c}},[String(counts[k])]));
        chip.appendChild(h("span",{style:{fontSize:"10px",color:sh.c+"99"}},[sh.label]));
        rs.appendChild(chip);
      });
      rp.appendChild(rs);
    }
    wrap.appendChild(rp);
  }

  // Paint bar
  const pb=h("div",{class:"paint-bar"});
  // Paint toggle
  pb.appendChild(h("button",{class:"paint-btn"+(S.paint?" on":""),onclick:()=>{S.paint=!S.paint;renderContent();renderHeader();}},[S.paint?"✏️ تعديل":"✏️ تعديل"]));
  // +/- day offset
  pb.appendChild(h("button",{class:"day-adj",style:{background:"#fee2e2",border:"1px solid #fca5a5",color:"#ef4444"},
    title:"تقديم يوم",onclick:()=>{
      if(!team)return;const td=S.teamDates[team.id];if(!td)return;
      const d=new Date(td);d.setDate(d.getDate()-1);
      S.teamDates[team.id]=d.toISOString().slice(0,10);save();renderContent();
    }},["−"]));
  pb.appendChild(h("button",{class:"day-adj",style:{background:"#d1fae5",border:"1px solid #6ee7b7",color:"#059669"},
    title:"تأخير يوم",onclick:()=>{
      if(!team)return;const td=S.teamDates[team.id];if(!td)return;
      const d=new Date(td);d.setDate(d.getDate()+1);
      S.teamDates[team.id]=d.toISOString().slice(0,10);save();renderContent();
    }},["＋"]));

  if(S.paint){
    SK.forEach(k=>{
      const sh=SHIFTS[k];
      const btn=h("button",{class:"shift-pick",
        style:{background:S.paintShift===k?sh.d:sh.l,borderColor:sh.b,color:S.paintShift===k?sh.c:sh.c+"bb"},
        onclick:()=>{S.paintShift=k;renderContent();}},[`${sh.e}${sh.short}`]);
      pb.appendChild(btn);
    });
  }else{
    pb.appendChild(h("span",{class:"hint"},["💡 اضغط يوماً لملاحظة"]));
    // Share
    pb.appendChild(h("button",{class:"share-btn",onclick:()=>{
      const dim=getDays(S.year,S.month);
      let txt2=`📅 تقويم الورديات\n${MA[S.month]} ${S.year}\n${curSec().label} · ${curGrp().label} · ${team?.name}\n`;
      const h1=toHijri(S.year,S.month+1,1),hE=toHijri(S.year,S.month+1,dim);
      txt2+=`🌙 ${h1.d} ${HM[h1.m-1]} – ${hE.d} ${HM[hE.m-1]} ${h1.y} هـ\n\n`;
      for(let dv=1;dv<=dim;dv++){const k=`${S.month}-${dv}`;const tid=sched[k];const sh=tid?SHIFTS[tid]:null;txt2+=`${dv} ${sh?sh.e:"-"} ${sh?sh.label:""}`+(S.notes[k]?` 📝 ${S.notes[k]}`:"")+"\n";}
      txt2+=`\nالإجمالي:\n`;
      SK.forEach(k=>{if(stats[k]>0)txt2+=`${SHIFTS[k].e} ${SHIFTS[k].label}: ${stats[k]} يوم\n`;});
      if(navigator.share)navigator.share({title:"تقويم الورديات",text:txt2});
      else if(navigator.clipboard)navigator.clipboard.writeText(txt2).then(()=>alert("✅ تم نسخ التقويم!"));
    }},["📤 مشاركة"]));
  }
  wrap.appendChild(pb);

  // Hijri + compact stats
  const hs=h("div",{class:"hs-bar"});
  const h1=toHijri(S.year,S.month+1,1),hE=toHijri(S.year,S.month+1,getDays(S.year,S.month));
  hs.appendChild(h("div",{class:"hs-hijri"},[`🌙 ${h1.d} ${HM[h1.m-1]}${h1.m!==hE.m?`–${hE.d} ${HM[hE.m-1]}`:""}  ${h1.y}هـ`]));
  const chips=h("div",{class:"hs-chips"});
  SK.filter(k=>(stats[k]||0)>0).forEach(k=>{
    const sh=SHIFTS[k];
    const c=h("div",{class:"stat-chip",style:{background:sh.l,borderColor:sh.b}});
    c.appendChild(h("span",{class:"se"},[sh.e]));
    c.appendChild(h("span",{class:"sn",style:{color:sh.c}},[String(stats[k])]));
    chips.appendChild(c);
  });
  hs.appendChild(chips);
  wrap.appendChild(hs);

  // Calendar grid
  const cal=h("div",{class:"cal-card"});
  const dhdiv=h("div",{class:"day-headers"});
  DS.forEach(d=>dhdiv.appendChild(h("div",{class:"dh"},[d])));
  cal.appendChild(dhdiv);
  const grid=h("div",{class:"days-grid",
    onmouseleave:()=>{S.dragActive=false;S.dragShift=null;}});

  for(let i=0;i<getFirst(S.year,S.month);i++)grid.appendChild(h("div",{}));
  for(let day=1;day<=getDays(S.year,S.month);day++){
    const key=`${S.month}-${day}`;
    const tid=sched[key],sh=tid?SHIFTS[tid]:null;
    const isToday=now.getFullYear()===S.year&&now.getMonth()===S.month&&now.getDate()===day;
    const isOv=!!(S.overrides[team?.id]&&S.overrides[team?.id][key]);
    const hasNote=!!S.notes[key];
    const isHL=S.highlight&&S.highlight.m===S.month&&S.highlight.d===day;
    const cell=h("div",{class:"day-cell"+(isToday?" today":"")+(isHL?" hl":"")+(isOv?" ov":"")});
    cell.style.background=sh?sh.l:"#f8faff";
    cell.style.borderColor=isToday?"#1a2035":isHL?"#f59e0b":sh?sh.b:"#e8edf5";
    if(isToday)cell.style.boxShadow=`0 2px 6px ${acc}33`;
    if(isHL)cell.style.boxShadow="0 0 0 3px #fef3c7";
    const numEl=h("span",{class:"dc-num"+(isToday?" bold":""),style:{color:sh?sh.c:"#c7d2e0"}},[String(day)]);
    cell.appendChild(numEl);
    if(sh)cell.appendChild(h("span",{class:"dc-emo"},[sh.e]));
    if(hasNote)cell.appendChild(h("div",{class:"note-dot"}));
    if(isOv&&sh)cell.appendChild(h("div",{class:"ov-dot",style:{background:sh.c}}));

    const doCell=(isTouch)=>{
      if(S.paint){
        const cur=sched[key];const nx=cur===S.paintShift?"off":S.paintShift;
        S.dragActive=true;S.dragShift=nx;
        if(!S.overrides[team?.id])S.overrides[team?.id]={};
        S.overrides[team.id][key]=nx;save();renderContent();renderHeader();
      }else if(!isTouch||!S.dragActive){
        S.noteCtx={month:S.month,day,key};openNoteModal();
      }
    };
    cell.addEventListener("mousedown",(e)=>{e.preventDefault();doCell(false);});
    cell.addEventListener("mouseenter",()=>{
      if(S.dragActive&&S.paint){
        if(!S.overrides[team?.id])S.overrides[team?.id]={};
        S.overrides[team.id][key]=S.dragShift;save();renderContent();
      }
    });
    cell.addEventListener("mouseup",()=>{S.dragActive=false;S.dragShift=null;});
    cell.addEventListener("touchstart",()=>{doCell(true);},{passive:true});
    cell.addEventListener("touchend",()=>{if(!S.paint)doCell(true);S.dragActive=false;},{passive:true});
    grid.appendChild(cell);
  }
  cal.appendChild(grid);
  wrap.appendChild(cal);

  // Legend
  const leg=h("div",{class:"legend"});
  SK.forEach(k=>{const sh=SHIFTS[k];
    const li=h("div",{class:"leg",style:{borderColor:sh.b,background:sh.l}});
    li.appendChild(h("span",{style:{fontSize:"10px"}},[sh.e]));
    li.appendChild(h("span",{class:"leg-txt",style:{color:sh.c}},[sh.label]));
    leg.appendChild(li);
  });
  const noteLeg=h("div",{class:"leg",style:{borderColor:"#fcd34d",background:"#fef3c7"}});
  noteLeg.appendChild(h("div",{style:{width:"3px",height:"3px",borderRadius:"50%",background:"#f59e0b"}}));
  noteLeg.appendChild(h("span",{class:"leg-txt",style:{color:"#f59e0b"}},["ملاحظة"]));
  leg.appendChild(noteLeg);
  wrap.appendChild(leg);

  // Notes panel
  const dim=getDays(S.year,S.month);
  const mNotes=[];
  for(let dv=1;dv<=dim;dv++){
    const key=`${S.month}-${dv}`;
    if(S.notes[key]){
      const tid=sched[key],sh=tid?SHIFTS[tid]:null;
      const hh=toHijri(S.year,S.month+1,dv);
      mNotes.push({day:dv,key,text:S.notes[key],sh,hh});
    }
  }
  const np=h("div",{class:"notes-panel",style:{borderColor:mNotes.length?"#3b82f6":"#e2ecff"}});
  const nph=h("div",{class:"np-header"});
  nph.appendChild(h("span",{style:{fontSize:"14px"}},["📋"]));
  nph.appendChild(h("span",{class:"np-title"},[`ملاحظات ${MA[S.month]}`]));
  if(mNotes.length)nph.appendChild(h("div",{class:"np-count"},[String(mNotes.length)]));
  np.appendChild(nph);
  if(mNotes.length===0){
    np.appendChild(h("div",{class:"np-empty"},["لا توجد ملاحظات في هذا الشهر"]));
  }else{
    mNotes.forEach(({day:dv,key,text,sh,hh})=>{
      const row=h("div",{class:"np-row",onclick:()=>{S.noteCtx={month:S.month,day:dv,key};openNoteModal();}});
      const dayBox=h("div",{class:"np-day-box",style:{background:sh?sh.l:"#f8faff",border:`1px solid ${sh?sh.b:"#e2e8f0"}`}});
      dayBox.appendChild(h("div",{class:"np-day-num",style:{color:sh?sh.c:"#64748b"}},[String(dv)]));
      dayBox.appendChild(h("div",{class:"np-day-emo"},[sh?sh.e:""]));
      row.appendChild(dayBox);
      const body=h("div",{class:"np-body"});
      body.appendChild(h("div",{class:"np-hijri"},[`${hh.d} ${HM[hh.m-1]} ${hh.y} هـ`]));
      body.appendChild(h("div",{class:"np-text"},[text]));
      row.appendChild(body);
      row.appendChild(h("div",{class:"np-arrow"},["←"]));
      np.appendChild(row);
    });
  }
  wrap.appendChild(np);
  container.appendChild(wrap);
}

/* ── COMPARE ── */
function renderCompare(container){
  const wrap=h("div",{class:"tab-page fade-in"});
  wrap.appendChild(h("div",{class:"tab-title"},["⚖️ مقارنة الورديات"]));

  // Filters
  const fc=h("div",{class:"card"});
  fc.appendChild(h("span",{class:"lbl"},["🔎 تصفية حسب القطاع والمجموعة:"]));
  const fr=h("div",{class:"filter-row"});
  [["all","الكل","#64748b","#f1f5f9"]].forEach(([id,lbl,c,bg])=>{
    const act=S.cmpFilter===id;
    fr.appendChild(h("button",{class:"filter-btn",style:{background:act?"#3b82f6":bg,color:act?"white":c},
      onclick:()=>{S.cmpFilter=id;renderContent();}},[lbl]));
  });
  S.sectors.forEach(s=>{
    const act=S.cmpFilter===s.id;
    fr.appendChild(h("button",{class:"filter-btn",style:{background:act?s.color:"#f1f5f9",color:act?"white":"#64748b",boxShadow:act?`0 3px 8px ${s.color}44`:"none"},
      onclick:()=>{S.cmpFilter=s.id;renderContent();}},[`${s.emoji} ${s.label}`]));
  });
  S.sectors.flatMap(s=>s.groups).forEach(g=>{
    const act=S.cmpFilter===g.id;
    fr.appendChild(h("button",{class:"filter-btn",style:{background:act?`${g.color}22`:"transparent",border:`1.5px solid ${act?g.color:"#e2e8f0"}`,color:act?g.color:"#94a3b8"},
      onclick:()=>{S.cmpFilter=g.id;renderContent();}},[`${g.icon} ${g.label}`]));
  });
  fc.appendChild(fr);

  fc.appendChild(h("span",{class:"lbl",style:{marginTop:"8px"}},["تصفية الورديات:"]));
  const sf=h("div",{class:"filter-row"});
  const allSF=S.cmpShiftFilter==="all";
  sf.appendChild(h("button",{class:"shift-filter",style:{background:allSF?"#1e293b":"#f1f5f9",border:"none",color:allSF?"white":"#64748b"},
    onclick:()=>{S.cmpShiftFilter="all";renderContent();}},"كل الورديات"));
  SK.forEach(k=>{const sh=SHIFTS[k];const act=S.cmpShiftFilter===k;
    sf.appendChild(h("button",{class:"shift-filter",style:{background:act?sh.d:sh.l,borderColor:sh.b,border:`1.5px solid ${sh.b}`,color:act?sh.c:sh.c+"bb"},
      onclick:()=>{S.cmpShiftFilter=S.cmpShiftFilter===k?"all":k;renderContent();}},[`${sh.e}${sh.short}`]));
  });
  fc.appendChild(sf);
  wrap.appendChild(fc);

  // Date picker
  const dc=h("div",{class:"card"});
  dc.appendChild(h("span",{class:"lbl"},["📅 اختر التاريخ:"]));
  const dg=h("div",{class:"grid3"});
  [{lbl:"اليوم",st:"cDay",mn:1,mx:31},{lbl:"الشهر",st:"cMon",isSelect:true},{lbl:"السنة م",st:"cYr",mn:2020,mx:2100}].forEach(cfg=>{
    const d=h("div",{});
    d.appendChild(h("span",{class:"lbl"},[cfg.lbl]));
    let inp;
    if(cfg.isSelect){
      inp=h("select",{class:"fi fi-select",onchange:(e)=>{
        S.cMon=e.target.value;
        const y2=parseInt(S.cYr),m2=parseInt(S.cMon)-1,d2=parseInt(S.cDay);
        if(y2&&m2>=0&&d2)S.cDate=`${y2}-${String(m2+1).padStart(2,"0")}-${String(d2).padStart(2,"0")}`;
        S.coverRes=null;renderContent();
      }});
      inp.appendChild(h("option",{value:""},"--"));
      MA.forEach((name,i)=>inp.appendChild(h("option",{value:String(i+1),selected:S.cMon===String(i+1)?true:undefined},[name])));
    }else{
      inp=h("input",{type:"number",class:"fi",value:S[cfg.st],min:String(cfg.mn),max:String(cfg.mx),
        placeholder:cfg.lbl,onchange:(e)=>{
          S[cfg.st]=e.target.value;
          const y2=parseInt(S.cYr),m2=parseInt(S.cMon)-1,d2=parseInt(S.cDay);
          if(y2&&m2>=0&&d2)S.cDate=`${y2}-${String(m2+1).padStart(2,"0")}-${String(d2).padStart(2,"0")}`;
          S.coverRes=null;renderContent();
        }});
    }
    d.appendChild(inp);
    dg.appendChild(d);
  });
  dc.appendChild(dg);
  if(S.cDate){
    const[y,m,d]=S.cDate.split("-").map(Number);
    if(y&&m&&d){const hh=toHijri(y,m,d);dc.appendChild(h("div",{class:"hijri-hint"},[`🌙 ${hh.d} ${HM[hh.m-1]} ${hh.y} هـ`]));}
  }
  wrap.appendChild(dc);

  // Compare results
  const cmpData=getCmpData();
  if(cmpData){
    const res=h("div",{class:"fade-in"});
    SK.filter(sid=>S.cmpShiftFilter==="all"||S.cmpShiftFilter===sid).forEach(sid=>{
      const sh=SHIFTS[sid];
      const members=(cmpData.byShift[sid]||[]).filter(({t,g,s})=>
        S.cmpFilter==="all"||S.cmpFilter===s.id||S.cmpFilter===g.id);
      if(!members.length)return;
      const blk=h("div",{class:"cmp-block",style:{border:`1px solid ${sh.b}`}});
      const bh=h("div",{class:"cmp-header"});
      const badge=h("div",{class:"cmp-badge",style:{background:sh.l,border:`1px solid ${sh.b}`}});
      badge.appendChild(h("span",{style:{fontSize:"14px"}},[sh.e]));
      badge.appendChild(h("span",{style:{fontWeight:"800",fontSize:"12px",color:sh.c}},[sh.label]));
      bh.appendChild(badge);
      bh.appendChild(h("div",{class:"cmp-count",style:{background:sh.c}},[String(members.length)]));
      blk.appendChild(bh);
      const teams=h("div",{class:"cmp-teams"});
      members.forEach(({t,g,s})=>{
        const tc=h("div",{class:"cmp-team",style:{background:`${t.color}12`,border:`1px solid ${t.color}40`}});
        tc.appendChild(h("div",{class:"cmp-tname",style:{color:t.color}},[t.name]));
        tc.appendChild(h("div",{class:"cmp-tsub"},[`${s.emoji}${g.label}`]));
        teams.appendChild(tc);
      });
      blk.appendChild(teams);
      res.appendChild(blk);
    });
    if((cmpData.byShift["none"]||[]).filter(({t,g,s})=>S.cmpFilter==="all"||S.cmpFilter===s.id||S.cmpFilter===g.id).length>0&&S.cmpShiftFilter==="all"){
      const none=h("div",{class:"card"});
      none.appendChild(h("div",{style:{color:"#94a3b8",fontSize:"10px",fontWeight:"600",marginBottom:"5px"}},["غير محدد"]));
      const nf=h("div",{style:{display:"flex",flexWrap:"wrap",gap:"4px"}});
      (cmpData.byShift["none"]||[]).filter(({t,g,s})=>S.cmpFilter==="all"||S.cmpFilter===s.id||S.cmpFilter===g.id).forEach(({t})=>{
        nf.appendChild(h("span",{style:{background:"#f1f5f9",borderRadius:"6px",padding:"3px 7px",fontSize:"10px",color:"#64748b"}},[t.name]));
      });
      none.appendChild(nf);res.appendChild(none);
    }
    wrap.appendChild(res);
  }

  // Coverage
  const cv=h("div",{class:"cover-box"});
  cv.appendChild(h("div",{class:"cover-title"},["🔄 البحث عن تغطية"]));
  cv.appendChild(h("div",{class:"cover-sub"},["من هو على راحة أو إجازة ويمكنه التغطية؟"]));
  const cvs=h("div",{class:"cover-shifts"});
  SK.filter(k=>k!=="off"&&k!=="vacation").forEach(k=>{
    const sh=SHIFTS[k];const act=S.coverSh===k;
    cvs.appendChild(h("button",{class:"shift-filter",style:{background:act?sh.d:sh.l,border:`1.5px solid ${sh.b}`,color:act?sh.c:sh.c+"bb"},
      onclick:()=>{S.coverSh=k;renderContent();}},[`${sh.e}${sh.label}`]));
  });
  cv.appendChild(cvs);
  cv.appendChild(h("button",{class:"btn-primary",style:{marginBottom:S.coverRes?"10px":"0"},onclick:()=>{findCover();renderContent();}},["🔍 ابحث عن متاح"]));
  if(S.coverRes){
    const cr=h("div",{class:"fade-in"});
    if(S.coverRes.av.length>0){
      const avh=h("div",{style:{display:"flex",alignItems:"center",gap:"5px",marginBottom:"6px"}});
      avh.appendChild(h("div",{style:{width:"6px",height:"6px",borderRadius:"50%",background:"#10b981"}}));
      avh.appendChild(h("span",{style:{fontWeight:"700",fontSize:"11px",color:"#059669"}},[`متاح للتغطية (${S.coverRes.av.length})`]));
      cr.appendChild(avh);
      S.coverRes.av.forEach(({t,g,s,type})=>{
        const sh=type?SHIFTS[type]:null;
        const ci=h("div",{class:"cover-item"});
        const ld=h("div",{});
        ld.appendChild(h("div",{style:{fontWeight:"700",fontSize:"12px"}},[t.name]));
        ld.appendChild(h("div",{style:{fontSize:"9px",color:"#94a3b8"}},[`${t.members.join("، ")} · ${s.emoji}${g.label}`]));
        ci.appendChild(ld);
        if(sh){const tag=h("div",{class:"tag",style:{background:sh.l,border:`1px solid ${sh.b}`,color:sh.c}});tag.appendChild(h("span",[],sh.e));tag.appendChild(h("span",[],[sh.label]));ci.appendChild(tag);}
        cr.appendChild(ci);
      });
    }else{
      const na=h("div",{class:"no-avail"});
      na.appendChild(h("div",{style:{fontSize:"13px",marginBottom:"2px"}},["😔"]));
      na.appendChild(h("div",{style:{fontWeight:"700",fontSize:"11px",color:"#c2410c"}},["لا يوجد أحد متاح في هذا اليوم"]));
      cr.appendChild(na);
    }
    if(S.coverRes.busy.length>0){
      cr.appendChild(h("div",{style:{color:"#94a3b8",fontSize:"10px",fontWeight:"600",marginBottom:"4px"}},["مشغولون:"]));
      const bi=h("div",{class:"busy-items"});
      S.coverRes.busy.forEach(({t,type})=>{const sh=type?SHIFTS[type]:null;
        const bitem=h("div",{class:"busy-item",style:{background:sh?.l||"#f8faff",border:`1px solid ${sh?.b||"#e2e8f0"}`}});
        bitem.appendChild(h("span",{style:{fontSize:"9px"}},[sh?.e||"—"]));
        bitem.appendChild(h("span",{style:{fontSize:"9px",fontWeight:"600",color:sh?.c||"#64748b"}},[t.name]));
        bi.appendChild(bitem);
      });
      cr.appendChild(bi);
    }
    cv.appendChild(cr);
  }
  wrap.appendChild(cv);
  container.appendChild(wrap);
}

/* ── SEARCH ── */
function renderSearch(container){
  const wrap=h("div",{class:"tab-page fade-in"});
  wrap.appendChild(h("div",{class:"tab-title"},["🔍 البحث بالتاريخ"]));

  // Mode tabs
  const st=h("div",{class:"search-tabs"});
  st.appendChild(h("button",{class:"stab"+(S.sModeM?" on":" off"),onclick:()=>{S.sModeM=true;S.sResult=null;renderContent();}},"📅 ميلادي"));
  st.appendChild(h("button",{class:"stab"+(!S.sModeM?" on":" off"),onclick:()=>{S.sModeM=false;S.sResult=null;renderContent();}},"🌙 هجري"));
  wrap.appendChild(st);

  // Input card
  const ic=h("div",{class:"card"});
  if(S.sModeM){
    ic.appendChild(h("span",{class:"lbl"},["التاريخ الميلادي:"]));
    const g=h("div",{class:"grid3"});
    [{lbl:"اليوم",k:"sMD",mn:1,mx:31},{lbl:"الشهر",k:"sMM",isSelect:true},{lbl:"السنة م",k:"sMY",mn:2020,mx:2100}].forEach(cfg=>{
      const d=h("div",{});
      d.appendChild(h("span",{class:"lbl"},[cfg.lbl]));
      if(cfg.isSelect){
        const sel=h("select",{class:"fi fi-select",onchange:(e)=>{S[cfg.k]=e.target.value;renderContent();}});
        sel.appendChild(h("option",{value:""},"--"));
        MA.forEach((n,i)=>sel.appendChild(h("option",{value:String(i+1),...(S[cfg.k]===String(i+1)?{selected:""}:{})},n)));
        d.appendChild(sel);
      }else{
        d.appendChild(h("input",{type:"number",class:"fi",value:S[cfg.k],placeholder:cfg.lbl,min:String(cfg.mn),max:String(cfg.mx),onchange:(e)=>{S[cfg.k]=e.target.value;renderContent();}}));
      }
      g.appendChild(d);
    });
    ic.appendChild(g);
    if(S.sMD&&S.sMM&&S.sMY){
      const y2=parseInt(S.sMY),m2=parseInt(S.sMM)-1,d2=parseInt(S.sMD);
      if(y2>0&&m2>=0&&d2>0){const hh=toHijri(y2,m2+1,d2);ic.appendChild(h("div",{class:"hijri-hint"},[`🌙 ${hh.d} ${HM[hh.m-1]} ${hh.y} هـ`]));}
    }
  }else{
    ic.appendChild(h("span",{class:"lbl"},["التاريخ الهجري:"]));
    const g=h("div",{class:"grid3"});
    [{lbl:"اليوم",k:"sHD",mn:1,mx:30},{lbl:"الشهر",k:"sHM",isSelect:true,ar:true},{lbl:"السنة هـ",k:"sHY",mn:1400,mx:1500}].forEach(cfg=>{
      const d=h("div",{});
      d.appendChild(h("span",{class:"lbl"},[cfg.lbl]));
      if(cfg.isSelect){
        const sel=h("select",{class:"fi fi-select",onchange:(e)=>{S[cfg.k]=e.target.value;renderContent();}});
        sel.appendChild(h("option",{value:""},"--"));
        HM.forEach((n,i)=>sel.appendChild(h("option",{value:String(i+1),...(S[cfg.k]===String(i+1)?{selected:""}:{})},n)));
        d.appendChild(sel);
      }else{
        d.appendChild(h("input",{type:"number",class:"fi",value:S[cfg.k],placeholder:cfg.lbl,min:String(cfg.mn),max:String(cfg.mx),onchange:(e)=>{S[cfg.k]=e.target.value;renderContent();}}));
      }
      g.appendChild(d);
    });
    ic.appendChild(g);
  }
  wrap.appendChild(ic);

  wrap.appendChild(h("button",{class:"btn-primary",style:{marginBottom:"10px"},onclick:()=>{doSearch();renderContent();}},["بحث ←"]));

  if(S.sResult==="err")wrap.appendChild(h("div",{style:{textAlign:"center",color:"#ef4444",fontSize:"12px",padding:"8px"}},["⚠️ التاريخ غير صحيح"]));
  if(S.sResult&&S.sResult!=="err"){
    const rv=h("div",{class:"fade-in"});
    const rh=h("div",{class:"card"});
    rh.appendChild(h("div",{style:{fontWeight:"800",fontSize:"14px"}},[`${S.sResult.gD} ${MA[S.sResult.gM]} ${S.sResult.gY}`]));
    rh.appendChild(h("div",{style:{fontSize:"10px",color:"#94a3b8",marginTop:"2px"}},[`🌙 ${S.sResult.h.d} ${HM[S.sResult.h.m-1]} ${S.sResult.h.y} هـ`]));
    rv.appendChild(rh);
    S.sResult.rows.forEach(({t,g,s,type})=>{
      const sh=type?SHIFTS[type]:null;
      const sr=h("div",{class:"search-result",onclick:()=>{
        jumpToSearch(S.sResult.gY,S.sResult.gM,S.sResult.gD,t.id);
      }});
      const sl=h("div",{});
      sl.appendChild(h("div",{class:"sr-name"},[t.name]));
      sl.appendChild(h("div",{class:"sr-sub"},[`${s.emoji}${s.label} · ${g.icon}${g.label} · ${t.members.join("، ")}`]));
      sr.appendChild(sl);
      if(sh){const tag=h("div",{class:"tag",style:{background:sh.l,border:`1px solid ${sh.b}`,color:sh.c}});tag.appendChild(h("span",[],[sh.e]));tag.appendChild(h("span",[],[sh.label]));sr.appendChild(tag);}
      else sr.appendChild(h("span",{style:{color:"#94a3b8",fontSize:"10px"}},["—"]));
      rv.appendChild(sr);
    });
    wrap.appendChild(rv);
  }
  container.appendChild(wrap);
}

function doSearch(){
  let gY,gM,gD;
  if(S.sModeM){
    const d2=parseInt(S.sMD),m2=parseInt(S.sMM),y2=parseInt(S.sMY);
    if(!d2||!m2||!y2){S.sResult=null;return;}
    gY=y2;gM=m2-1;gD=d2;
  }else{
    const d=parseInt(S.sHD),hm=parseInt(S.sHM),hy=parseInt(S.sHY);
    if(!d||!hm||!hy){S.sResult=null;return;}
    const g=hToG(hy,hm,d);gY=g.y;gM=g.m-1;gD=g.d;
  }
  if(isNaN(gY)||gM<0||gM>11||gD<1||gD>getDays(gY,gM)){S.sResult="err";return;}
  const hh=toHijri(gY,gM+1,gD);
  const rows=allTeams(S.sectors).map(({t,g,s})=>{
    const sc=Object.assign({},buildSched(gY,g.pattern,S.teamDates[t.id]),S.overrides[t.id]||{});
    return{t,g,s,type:sc[`${gM}-${gD}`]||null};
  });
  S.sResult={gY,gM,gD,h:hh,rows};
}

function jumpToSearch(gY,gM,gD,tid){
  const f=findTeam(S.sectors,tid);
  if(f){S.selSec=f.s.id;S.selGrp=f.g.id;S.selTeam=tid;}
  S.year=gY;S.month=gM;S.highlight={m:gM,d:gD};S.tab="cal";
  save();render();
}

/* ── SETTINGS ── */
function renderSettings(container){
  const wrap=h("div",{class:"tab-page fade-in"});
  const sh=h("div",{style:{display:"flex",alignItems:"center",justifyContent:"space-between",marginBottom:"12px"}});
  sh.appendChild(h("div",{class:"tab-title",style:{marginBottom:"0"}},["⚙️ الإعدادات"]));
  sh.appendChild(h("button",{class:"sm-btn sm-add",onclick:()=>openModal("add-sec")},["+ قطاع"]));
  wrap.appendChild(sh);

  S.sectors.forEach(sec=>{
    const ss=h("div",{class:"sec-section"});
    const slr=h("div",{class:"sec-label-row"});
    const sll=h("div",{style:{display:"flex",alignItems:"center",gap:"6px"}});
    sll.appendChild(h("div",{class:"sec-bar",style:{background:sec.color}}));
    sll.appendChild(h("span",{class:"sec-name-text",style:{color:sec.color}},[`${sec.emoji} ${sec.label}`]));
    slr.appendChild(sll);
    const sbr=h("div",{style:{display:"flex",gap:"4px"}});
    sbr.appendChild(h("button",{class:"sm-btn sm-add",onclick:()=>openModal("add-grp",{secId:sec.id})},["+ مجموعة"]));
    sbr.appendChild(h("button",{class:"sm-btn sm-edit",onclick:()=>openModal("edit-sec",{secId:sec.id})},["✏️"]));
    sbr.appendChild(h("button",{class:"sm-btn sm-del",onclick:()=>{
      if(S.sectors.length<=1){alert("يجب أن يبقى قطاع واحد");return;}
      S.sectors=S.sectors.filter(s=>s.id!==sec.id);
      if(S.selSec===sec.id){S.selSec=S.sectors[0].id;S.selGrp=S.sectors[0].groups[0]?.id||"";S.selTeam=S.sectors[0].groups[0]?.teams[0]?.id||"";}
      save();render();
    }},["🗑️"]));
    slr.appendChild(sbr);ss.appendChild(slr);

    sec.groups.forEach(grp=>{
      const gc=h("div",{class:"grp-card"});
      const gh=h("div",{class:"grp-header"});
      const gl=h("div",{style:{display:"flex",alignItems:"center",gap:"7px"}});
      const gib=h("div",{class:"grp-icon-box",style:{background:`${grp.color}18`,border:`2px solid ${grp.color}44`}},[grp.icon]);
      gl.appendChild(gib);
      const glt=h("div",{});
      glt.appendChild(h("div",{class:"grp-name"},[grp.label]));
      glt.appendChild(h("div",{class:"grp-pat"},[grp.pattern.map(p=>SHIFTS[p]?.e||"").join("")+" ↺"]));
      gl.appendChild(glt);gh.appendChild(gl);
      const gbr=h("div",{class:"btn-row"});
      gbr.appendChild(h("button",{class:"sm-btn sm-add",onclick:()=>openModal("add-team",{secId:sec.id,grpId:grp.id})},["+ فرقة"]));
      gbr.appendChild(h("button",{class:"sm-btn sm-edit",onclick:()=>openModal("edit-grp",{secId:sec.id,grpId:grp.id})},["✏️"]));
      gbr.appendChild(h("button",{class:"sm-btn sm-del",onclick:()=>{
        if(sec.groups.length<=1){alert("يجب أن تبقى مجموعة واحدة");return;}
        const si=S.sectors.findIndex(x=>x.id===sec.id);
        S.sectors[si].groups=S.sectors[si].groups.filter(g=>g.id!==grp.id);
        if(S.selGrp===grp.id){S.selGrp=S.sectors[si].groups[0].id;S.selTeam=S.sectors[si].groups[0].teams[0]?.id||"";}
        save();render();
      }},["🗑️"]));
      gh.appendChild(gbr);gc.appendChild(gh);

      grp.teams.forEach(team=>{
        const ti=h("div",{class:"team-item",style:{border:`1px solid ${team.color}20`}});
        const tl=h("div",{});
        tl.appendChild(h("div",{class:"team-name",style:{color:team.color}},[team.name]));
        tl.appendChild(h("div",{class:"team-meta"},[`${team.members.join(" · ")} · ${fmtDate(S.teamDates[team.id])}`]));
        ti.appendChild(tl);
        const tbr=h("div",{class:"btn-row"});
        tbr.appendChild(h("button",{class:"sm-btn sm-edit",onclick:()=>openModal("edit-team",{secId:sec.id,grpId:grp.id,teamId:team.id})},["✏️"]));
        tbr.appendChild(h("button",{class:"sm-btn sm-del",onclick:()=>{
          if(grp.teams.length<=1){alert("يجب أن تبقى فرقة واحدة");return;}
          const si=S.sectors.findIndex(x=>x.id===sec.id);
          const gi=S.sectors[si].groups.findIndex(x=>x.id===grp.id);
          S.sectors[si].groups[gi].teams=S.sectors[si].groups[gi].teams.filter(t=>t.id!==team.id);
          if(S.selTeam===team.id)S.selTeam=S.sectors[si].groups[gi].teams[0].id;
          save();render();
        }},["🗑️"]));
        ti.appendChild(tbr);gc.appendChild(ti);
      });
      ss.appendChild(gc);
    });
    wrap.appendChild(ss);
  });

  // Save
  const sv2=h("div",{class:"save-card"});
  if(S.savedMsg){
    const ok=h("div",{class:"save-ok"});
    ok.appendChild(h("span",{},"✅"));
    ok.appendChild(h("span",{class:"save-ok-text"},"تم حفظ جميع التغييرات"));
    sv2.appendChild(ok);
  }
  sv2.appendChild(h("button",{class:"btn-primary",onclick:()=>{
    save();S.savedMsg=true;render();setTimeout(()=>{S.savedMsg=false;renderContent();},2500);
  }},["💾 حفظ التغييرات"]));
  wrap.appendChild(sv2);
  container.appendChild(wrap);
}

/* ── BOTTOM NAV ── */
function renderNav(){
  const nav=document.getElementById("bottom-nav");clr(nav);
  const tabs=[["cal","🗓️","التقويم"],["compare","⚖️","المقارنة"],["search","🔍","البحث"],["settings","⚙️","الإعدادات"]];
  const acc=accent();
  tabs.forEach(([id,ico,lbl])=>{
    const btn=h("button",{class:"nav-tab"+(S.tab===id?" active":""),onclick:()=>{S.tab=id;render();}});
    btn.appendChild(h("span",{class:"nt-ico"},[ico]));
    const lblEl=h("span",{class:"nt-lbl"},[lbl]);
    if(S.tab===id)lblEl.style.color=acc;
    btn.appendChild(lblEl);
    nav.appendChild(btn);
  });
}

/* ════════════════════════════════════════════
   MODALS
════════════════════════════════════════════ */
function openNoteModal(){
  const ctx=S.noteCtx;if(!ctx)return;
  const team=curTeam(),sched=team?getSched(team.id):{};
  const tid=sched[ctx.key],sh=tid?SHIFTS[tid]:null;
  const hh=toHijri(S.year,ctx.month+1,ctx.day);
  const existing=S.notes[ctx.key]||"";

  let noteText=existing;
  const overlay=h("div",{class:"modal-overlay",onclick:(e)=>{if(e.target===overlay)overlay.remove();}});
  const sheet=h("div",{class:"modal-sheet"});
  const hdrArea=h("div",{class:"modal-handle-area"});
  hdrArea.appendChild(h("div",{class:"modal-handle"}));
  hdrArea.appendChild(h("div",{class:"modal-title"},[`${ctx.day} ${MA[ctx.month]} ${S.year}`]));
  sheet.appendChild(hdrArea);
  const body=h("div",{class:"modal-body"});
  body.appendChild(h("div",{style:{fontSize:"11px",color:"#94a3b8",marginBottom:"8px"}},[`🌙 ${hh.d} ${HM[hh.m-1]} ${hh.y} هـ`]));
  if(sh){
    const tag=h("div",{class:"tag",style:{background:sh.l,border:`1px solid ${sh.b}`,color:sh.c,marginBottom:"12px"}});
    tag.appendChild(h("span",[],[sh.e]));
    tag.appendChild(h("span",[],[`${sh.label} · ${team?.name||""}`]));
    body.appendChild(tag);
  }
  const ta=h("textarea",{class:"fi",rows:"5",style:{resize:"none",lineHeight:"1.7",fontSize:"14px",padding:"11px"},placeholder:"اكتب ملاحظتك..."});
  ta.value=noteText;
  ta.addEventListener("input",(e)=>{noteText=e.target.value;});
  body.appendChild(ta);
  const ar=h("div",{class:"act-row"});
  ar.appendChild(h("button",{class:"btn-primary",onclick:()=>{
    const t=noteText.trim();
    if(t)S.notes[ctx.key]=t;else delete S.notes[ctx.key];
    save();overlay.remove();renderContent();
  }},["حفظ ✓"]));
  if(existing)ar.appendChild(h("button",{class:"btn-danger",onclick:()=>{delete S.notes[ctx.key];save();overlay.remove();renderContent();}},["🗑️"]));
  ar.appendChild(h("button",{class:"btn-neutral",onclick:()=>overlay.remove()},["✕"]));
  body.appendChild(ar);
  sheet.appendChild(body);overlay.appendChild(sheet);document.body.appendChild(overlay);
  setTimeout(()=>ta.focus(),300);
}

/* Settings modals */
function openModal(type,data={}){
  const overlay=h("div",{class:"modal-overlay",onclick:(e)=>{if(e.target===overlay)overlay.remove();}});
  const sheet=h("div",{class:"modal-sheet"});
  const hdrArea=h("div",{class:"modal-handle-area"});
  hdrArea.appendChild(h("div",{class:"modal-handle"}));

  let title="",bodyFn=null;

  if(type==="add-sec"||type==="edit-sec"){
    const editing=type==="edit-sec";
    const sec=editing?S.sectors.find(s=>s.id===data.secId):null;
    let name=sec?.label||"",emoji=sec?.emoji||"🌟",color=sec?.color||PAL[0];
    title=editing?"✏️ تعديل القطاع":"➕ إضافة قطاع";
    hdrArea.appendChild(h("div",{class:"modal-title"},[title]));sheet.appendChild(hdrArea);
    const body=h("div",{class:"modal-body"});
    body.appendChild(h("span",{class:"lbl"},["الاسم:"]));
    const ni=h("input",{class:"fi",style:{marginBottom:"12px"},placeholder:"مثال: نهار",value:name,onchange:(e)=>{name=e.target.value;}});
    body.appendChild(ni);
    body.appendChild(h("span",{class:"lbl"},["الأيقونة:"]));
    const ig=h("div",{class:"icon-grid",style:{marginBottom:"12px"}});
    ICONS.forEach(ic=>{
      const ib=h("button",{class:"icon-btn2"+(emoji===ic?" active":""),onclick:()=>{
        emoji=ic;ig.querySelectorAll(".icon-btn2").forEach(b=>{b.classList.remove("active");if(b.textContent===ic)b.classList.add("active");});
      }},[ic]);
      ig.appendChild(ib);
    });
    body.appendChild(ig);
    body.appendChild(h("span",{class:"lbl"},["اللون:"]));
    const cg=h("div",{class:"color-grid",style:{marginBottom:"14px"}});
    PAL.forEach(c=>{
      const cd=h("button",{class:"color-dot",style:{background:c,borderColor:color===c?"#1a2035":"transparent",boxShadow:color===c?`0 0 0 2px white,0 0 0 4px ${c}`:"none"},
        onclick:()=>{color=c;cg.querySelectorAll(".color-dot").forEach(b=>{b.style.borderColor=b.style.background===c?"#1a2035":"transparent";b.style.boxShadow=b.style.background===c?`0 0 0 2px white,0 0 0 4px ${c}`:"none";});}});
      cg.appendChild(cd);
    });
    body.appendChild(cg);
    const ar=h("div",{class:"act-row"});
    ar.appendChild(h("button",{class:"btn-primary",onclick:()=>{
      if(!name.trim())return;
      if(editing){const s=S.sectors.find(x=>x.id===data.secId);if(s){s.label=name.trim();s.emoji=emoji;s.color=color;}}
      else{const id=uid();S.sectors.push({id,label:name.trim(),emoji,color,groups:[]});}
      save();overlay.remove();render();
    }},["حفظ ✓"]));
    ar.appendChild(h("button",{class:"btn-neutral",onclick:()=>overlay.remove()},["إلغاء"]));
    body.appendChild(ar);sheet.appendChild(body);
  }

  else if(type==="add-grp"||type==="edit-grp"){
    const editing=type==="edit-grp";
    const sec=S.sectors.find(s=>s.id===data.secId);
    const grp=editing?sec?.groups.find(g=>g.id===data.grpId):null;
    let name=grp?.label||"",icon=grp?.icon||"🏠",color=grp?.color||PAL[0],pat=[...(grp?.pattern||["work1","work2","off","vacation"])];
    title=editing?"✏️ تعديل المجموعة":"➕ إضافة مجموعة";
    hdrArea.appendChild(h("div",{class:"modal-title"},[title]));sheet.appendChild(hdrArea);
    const body=h("div",{class:"modal-body"});
    body.appendChild(h("span",{class:"lbl"},["الاسم:"]));
    body.appendChild(h("input",{class:"fi",style:{marginBottom:"12px"},placeholder:"مثال: الغرفة",value:name,onchange:(e)=>{name=e.target.value;}}));
    body.appendChild(h("span",{class:"lbl"},["الأيقونة:"]));
    const ig=h("div",{class:"icon-grid",style:{marginBottom:"12px"}});
    ICONS.forEach(ic=>{const ib=h("button",{class:"icon-btn2"+(icon===ic?" active":""),onclick:()=>{icon=ic;ig.querySelectorAll(".icon-btn2").forEach(b=>{b.classList.remove("active");if(b.textContent===ic)b.classList.add("active");});}});ig.appendChild(ib);});
    body.appendChild(ig);
    body.appendChild(h("span",{class:"lbl"},["اللون:"]));
    const cg=h("div",{class:"color-grid",style:{marginBottom:"12px"}});
    PAL.forEach(c=>{const cd=h("button",{class:"color-dot",style:{background:c,borderColor:color===c?"#1a2035":"transparent",boxShadow:color===c?`0 0 0 2px white,0 0 0 4px ${c}`:"none"},onclick:()=>{color=c;cg.querySelectorAll(".color-dot").forEach(b=>{b.style.borderColor=b.style.background===c?"#1a2035":"transparent";b.style.boxShadow=b.style.background===c?`0 0 0 2px white,0 0 0 4px ${c}`:"none";});}});cg.appendChild(cd);});
    body.appendChild(cg);
    // Pattern builder
    body.appendChild(h("span",{class:"lbl"},[`نمط الوردية `,h("span",{style:{color:"#3b82f6"}},["(أول وردية = بداية الدورة)"])]));
    const pb=h("div",{class:"pat-pills"});
    const patAdd=h("div",{class:"pat-add-btns"});
    const renderPat=()=>{
      // Remove old pills
      while(pb.firstChild)pb.removeChild(pb.firstChild);
      if(pat.length===0){pb.appendChild(h("div",{style:{color:"#94a3b8",fontSize:"11px",padding:"8px 0"}},["أضف وردية أدناه ↓"]));}
      pat.forEach((sid,pi)=>{
        const sh=SHIFTS[sid];
        const pill=h("div",{class:"pat-pill",style:{background:sh.l,borderColor:sh.b}});
        pill.appendChild(h("span",{style:{fontSize:"12px"}},[sh.e]));
        pill.appendChild(h("span",{style:{fontSize:"10px",fontWeight:"700",color:sh.c}},[sh.short]));
        pill.appendChild(h("span",{class:"pat-idx",style:{color:sh.c+"70",background:sh.c+"15"}},["يوم "+(pi+1)]));
        const lBtn=h("button",{class:"pat-mv",style:{color:pi>0?"#64748b":"#e2e8f0",cursor:pi>0?"pointer":"default"},onclick:()=>{if(pi>0){[pat[pi-1],pat[pi]]=[pat[pi],pat[pi-1]];renderPat();}}},["›"]);
        const rBtn=h("button",{class:"pat-mv",style:{color:pi<pat.length-1?"#64748b":"#e2e8f0",cursor:pi<pat.length-1?"pointer":"default"},onclick:()=>{if(pi<pat.length-1){[pat[pi+1],pat[pi]]=[pat[pi],pat[pi+1]];renderPat();}}},["‹"]);
        pill.appendChild(lBtn);pill.appendChild(rBtn);
        pill.appendChild(h("button",{class:"pat-rm",onclick:()=>{pat.splice(pi,1);renderPat();}},["✕"]));
        pb.appendChild(pill);
      });
    };
    while(patAdd.firstChild)patAdd.removeChild(patAdd.firstChild);
    SK.forEach(k=>{const sh=SHIFTS[k];patAdd.appendChild(h("button",{class:"pat-add",style:{background:sh.l,borderColor:sh.b,border:`1.5px solid ${sh.b}`,color:sh.c},onclick:()=>{pat.push(k);renderPat();}},[`+${sh.e}${sh.short}`]));});
    renderPat();
    body.appendChild(pb);
    body.appendChild(patAdd);
    body.appendChild(h("div",{class:"pat-hint",id:"pat-hint"}));
    const ar=h("div",{class:"act-row"});
    ar.appendChild(h("button",{class:"btn-primary",onclick:()=>{
      if(!name.trim())return;
      if(editing){const si=S.sectors.findIndex(x=>x.id===data.secId);const gi=S.sectors[si].groups.findIndex(x=>x.id===data.grpId);if(gi>-1){S.sectors[si].groups[gi].label=name.trim();S.sectors[si].groups[gi].icon=icon;S.sectors[si].groups[gi].color=color;S.sectors[si].groups[gi].pattern=[...pat];}}
      else{const id=uid(),tid=uid();const si=S.sectors.findIndex(x=>x.id===data.secId);if(si>-1){S.sectors[si].groups.push({id,label:name.trim(),icon,color,pattern:[...pat],teams:[{id:tid,name:"المجموعة 1",color,members:["عضو جديد"]}]});S.teamDates[tid]=TODAY;}}
      save();overlay.remove();render();
    }},["حفظ ✓"]));
    ar.appendChild(h("button",{class:"btn-neutral",onclick:()=>overlay.remove()},["إلغاء"]));
    body.appendChild(ar);sheet.appendChild(body);
  }

  else if(type==="add-team"||type==="edit-team"){
    const editing=type==="edit-team";
    const f=editing?findTeam(S.sectors,data.teamId):null;
    let name=f?.t.name||"",color=f?.t.color||PAL[0],members=[...(f?.t.members||[""])],tmDate=S.teamDates[data.teamId]||TODAY;
    title=editing?"✏️ تعديل المجموعة الفرعية":"➕ إضافة مجموعة فرعية";
    hdrArea.appendChild(h("div",{class:"modal-title"},[title]));sheet.appendChild(hdrArea);
    const body=h("div",{class:"modal-body"});
    body.appendChild(h("span",{class:"lbl"},[`اسم المجموعة `,h("span",{style:{color:"#64748b"}},["(يظهر في التقويم)"])]));
    body.appendChild(h("input",{class:"fi",style:{marginBottom:"12px"},placeholder:"مثال: المجموعة 1",value:name,onchange:(e)=>{name=e.target.value;}}));
    body.appendChild(h("span",{class:"lbl"},["اللون:"]));
    const cg=h("div",{class:"color-grid",style:{marginBottom:"12px"}});
    PAL.forEach(c=>{const cd=h("button",{class:"color-dot",style:{background:c,borderColor:color===c?"#1a2035":"transparent",boxShadow:color===c?`0 0 0 2px white,0 0 0 4px ${c}`:"none"},onclick:()=>{color=c;cg.querySelectorAll(".color-dot").forEach(b=>{b.style.borderColor=b.style.background===c?"#1a2035":"transparent";b.style.boxShadow=b.style.background===c?`0 0 0 2px white,0 0 0 4px ${c}`:"none";});}});cg.appendChild(cd);});
    body.appendChild(cg);
    body.appendChild(h("span",{class:"lbl"},["الأعضاء:"]));
    const mw=h("div",{style:{marginBottom:"12px"}});
    const renderMembers=()=>{
      while(mw.firstChild)mw.removeChild(mw.firstChild);
      members.forEach((m,mi)=>{
        const mr=h("div",{class:"member-row"});
        const inp=h("input",{class:"fi member-input",value:m,placeholder:`عضو ${mi+1}`,onchange:(e)=>{members[mi]=e.target.value;}});
        mr.appendChild(inp);
        mr.appendChild(h("button",{class:"btn-danger",style:{width:"34px",height:"36px",borderRadius:"8px",flexShrink:"0",display:"flex",alignItems:"center",justifyContent:"center",fontSize:"13px"},onclick:()=>{members.splice(mi,1);if(!members.length)members.push("");renderMembers();}},["✕"]));
        mw.appendChild(mr);
      });
      mw.appendChild(h("button",{class:"fi",style:{background:"transparent",border:"1.5px dashed var(--border)",color:"var(--dim)",cursor:"pointer",fontSize:"11px",textAlign:"center"},onclick:()=>{members.push("");renderMembers();}},["+ إضافة عضو"]));
    };
    renderMembers();
    body.appendChild(mw);
    body.appendChild(h("span",{class:"lbl"},[`📅 تاريخ بداية الدورة `,h("span",{style:{color:"#10b981",fontWeight:"700"}},["(إجازة)"])]));
    const dateWrap=h("div",{style:{position:"relative",marginBottom:"14px"}});
    const dateInp=h("input",{type:"date",style:{position:"absolute",inset:"0",opacity:"0",zIndex:"2",cursor:"pointer",width:"100%"},value:tmDate,onchange:(e)=>{tmDate=e.target.value;dispDate.textContent=fmtDate(tmDate);}});
    const dispDate=h("div",{class:"date-display"},[fmtDate(tmDate),h("span",{style:{fontSize:"15px"}},["📅"])]);
    dateWrap.appendChild(dateInp);dateWrap.appendChild(dispDate);
    body.appendChild(dateWrap);
    const ar=h("div",{class:"act-row"});
    ar.appendChild(h("button",{class:"btn-primary",onclick:()=>{
      const valid=members.filter(m=>m.trim());
      if(!name.trim()||!valid.length)return;
      if(editing){const f2=findTeam(S.sectors,data.teamId);if(f2){f2.t.name=name.trim();f2.t.color=color;f2.t.members=valid;S.teamDates[data.teamId]=tmDate;}}
      else{const id=uid();const si=S.sectors.findIndex(x=>x.id===data.secId);const gi=S.sectors[si].groups.findIndex(x=>x.id===data.grpId);S.sectors[si].groups[gi].teams.push({id,name:name.trim(),color,members:valid});S.teamDates[id]=tmDate;}
      save();overlay.remove();render();
    }},["حفظ ✓"]));
    ar.appendChild(h("button",{class:"btn-neutral",onclick:()=>overlay.remove()},["إلغاء"]));
    body.appendChild(ar);sheet.appendChild(body);
  }

  overlay.appendChild(sheet);document.body.appendChild(overlay);
}

/* ════════════════════════════════════════════
   SWIPE TO CHANGE MONTH
════════════════════════════════════════════ */
let swipeX=null;
document.getElementById("content").addEventListener("touchstart",(e)=>{swipeX=e.touches[0].clientX;},{passive:true});
document.getElementById("content").addEventListener("touchend",(e)=>{
  if(swipeX===null)return;
  const dx=e.changedTouches[0].clientX-swipeX;swipeX=null;
  if(S.tab==="cal"&&Math.abs(dx)>55){
    const d=dx>0?-1:1;
    let m=S.month+d,y=S.year;if(m>11){m=0;y++;}if(m<0){m=11;y--;}
    S.month=m;S.year=y;S.highlight=null;save();renderContent();renderHeader();
  }
},{passive:true});

/* Close members popup on outside click */
document.addEventListener("touchstart",()=>{if(S.membersPopup){S.membersPopup=null;renderHeader();}},{passive:true});
document.addEventListener("mousedown",()=>{if(S.membersPopup){S.membersPopup=null;renderHeader();}});
document.addEventListener("mouseup",()=>{S.dragActive=false;S.dragShift=null;});

/* ════════════════════════════════════════════
   SERVICE WORKER
════════════════════════════════════════════ */
if('serviceWorker' in navigator){
  window.addEventListener('load',()=>navigator.serviceWorker.register('sw.js').catch(()=>{}));
}

/* iOS install banner */
(function(){
  const isIOS=/iphone|ipad|ipod/i.test(navigator.userAgent);
  const isSA=navigator.standalone||window.matchMedia('(display-mode:standalone)').matches;
  if(isIOS&&!isSA&&!localStorage.getItem('ib_w3')){
    setTimeout(()=>{
      const b=document.createElement('div');
      b.style.cssText='position:fixed;bottom:calc(14px + env(safe-area-inset-bottom));left:50%;transform:translateX(-50%);background:white;border:1px solid #bfdbfe;border-radius:16px;padding:12px 16px;z-index:9999;direction:rtl;display:flex;align-items:center;gap:12px;box-shadow:0 8px 32px rgba(59,130,246,.15);max-width:calc(100% - 28px);font-family:Cairo,-apple-system,sans-serif;';
      b.innerHTML='<div style="font-size:24px">🗓️</div><div style="flex:1"><div style="font-weight:700;font-size:13px;color:#1a2035">أضف للشاشة الرئيسية</div><div style="font-size:11px;color:#64748b;margin-top:2px">اضغط مشاركة ← إضافة إلى الشاشة الرئيسية</div></div><button onclick="this.parentNode.remove();localStorage.setItem(\'ib_w3\',\'1\')" style="background:transparent;border:none;color:#94a3b8;cursor:pointer;font-size:20px;padding:4px">✕</button>';
      document.body.appendChild(b);
    },2500);
  }
})();

/* ════════════════════════════════════════════
   INIT
════════════════════════════════════════════ */
render();
</script>

</body>
</html>