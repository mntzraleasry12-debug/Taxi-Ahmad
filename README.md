<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, user-scalable=no">
<title>تاكسي أحمد - النسخة المدمجة 🚕</title>
<link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
<style>
/* ================= 1. التنسيقات الأساسية (CSS) ================= */
:root{--p:#FFC107;--d:#0F1419;--g:#6B7280;--bg:#F5F5F7;--w:#FFF;--grn:#10B981;--red:#EF4444;--blu:#3B82F6}
*{box-sizing:border-box;margin:0;padding:0;-webkit-tap-highlight-color:transparent;font-family:system-ui,-apple-system,sans-serif}
body{height:100vh;overflow:hidden;background:#000;display:flex;justify-content:center}
#app{width:100%;max-width:500px;height:100%;background:var(--bg);position:relative;overflow:hidden}
/* شاشة التحميل */
#boot{position:absolute;inset:0;background:linear-gradient(135deg,var(--p),#FF9800);display:flex;flex-direction:column;align-items:center;justify-content:center;z-index:9999;color:#fff;transition:.3s}
#boot.hide{opacity:0;pointer-events:none}
.sp{width:40px;height:40px;border:4px solid #fff3;border-top-color:#fff;border-radius:50%;animation:spin 1s linear infinite;margin-top:20px}
@keyframes spin{to{transform:rotate(360deg)}}
/* واجهة الدخول */
.auth{position:absolute;inset:0;background:linear-gradient(135deg,var(--p),#FF9800);display:flex;align-items:center;justify-content:center;padding:20px;z-index:100;overflow-y:auto}
.card{background:#fff;border-radius:24px;padding:30px 20px;width:100%;max-width:380px;box-shadow:0 20px 40px rgba(0,0,0,.2)}
.roles{display:flex;gap:10px;margin:20px 0}
.role{flex:1;padding:15px;text-align:center;border:2px solid #eee;border-radius:16px;cursor:pointer;font-weight:800}
.role.active{border-color:var(--p);background:#FFF8E1}
.tabs{display:flex;background:var(--bg);border-radius:12px;padding:4px;margin-bottom:20px}
.tab{flex:1;padding:10px;border:none;background:0;font-weight:700;color:var(--g);border-radius:8px;cursor:pointer}
.tab.active{background:#fff;color:var(--d);box-shadow:0 2px 5px #0001}
.fg{margin-bottom:15px}
.fg label{display:block;font-size:12px;font-weight:800;color:var(--g);margin-bottom:5px}
.fg input,.fg select{width:100%;padding:14px;border:2px solid #eee;background:var(--bg);border-radius:12px;font-size:15px;font-weight:700;outline:none}
.fg input:focus{border-color:var(--p);background:#fff}
.btn{width:100%;padding:16px;border:none;border-radius:16px;font-size:16px;font-weight:800;cursor:pointer;display:flex;justify-content:center;gap:8px;transition:.2s}
.btn:active{transform:scale(.97)}
.btn-p{background:var(--p);color:var(--d)}
.btn-r{background:0;color:var(--red);border:2px solid var(--red)}
.btn-b{background:var(--blu);color:#fff}
.btn-g{background:var(--grn);color:#fff}
/* الخريطة والواجهة */
#map{position:absolute;inset:0;z-index:1;background:#E5E7EB}
.top{position:absolute;top:0;left:0;right:0;padding:15px;z-index:10;display:flex;justify-content:space-between;pointer-events:none}
.chip{background:#fff;padding:6px 15px 6px 6px;border-radius:99px;display:flex;align-items:center;gap:10px;box-shadow:0 4px 15px #0001;pointer-events:auto;cursor:pointer;border:none}
.av{width:40px;height:40px;border-radius:50%;background:var(--p);display:flex;align-items:center;justify-content:center;font-weight:800}
.chip.driver .av{background:var(--blu);color:#fff}
/* القائمة السفلية */
.sheet{position:absolute;bottom:0;left:0;right:0;background:#fff;border-radius:24px 24px 0 0;padding:10px 20px 20px;z-index:20;box-shadow:0 -10px 30px #0001;max-height:85%;overflow-y:auto;transition:.3s;animation:up .3s}
@keyframes up{from{transform:translateY(100%)}}
.close{position:absolute;top:20px;left:20px;width:32px;height:32px;border-radius:50%;background:var(--bg);border:none;font-weight:800;color:var(--g);cursor:pointer}
.open-btn{position:absolute;bottom:20px;left:50%;transform:translateX(-50%);z-index:15;width:85%;padding:16px;border-radius:99px;font-weight:800;font-size:16px;box-shadow:0 10px 20px #FFC10766;display:none;background:var(--p);border:none}
.handle{width:40px;height:5px;background:#eee;border-radius:9px;margin:0 auto 15px}
.title{font-size:20px;font-weight:800;margin-bottom:15px}
.row{display:flex;align-items:center;gap:12px;background:var(--bg);padding:12px 15px;border-radius:16px;margin-bottom:10px;border:2px solid transparent}
.dot{width:12px;height:12px;border-radius:50%}
.dot.g{background:var(--grn);box-shadow:0 0 0 4px #10B98133}
.dot.r{background:var(--red);box-shadow:0 0 0 4px #EF444433}
.loc-info{flex:1;cursor:pointer}
.loc-info .l{font-size:11px;color:var(--g);font-weight:700}
.loc-info .v{font-size:14px;font-weight:800}
.map-btn{width:36px;height:36px;border-radius:10px;border:1px solid #eee;background:#fff;cursor:pointer}
.cars{display:grid;grid-template-columns:repeat(4,1fr);gap:8px;margin:15px 0}
.car{background:var(--bg);padding:10px 5px;border-radius:12px;text-align:center;cursor:pointer;border:2px solid transparent}
.car.active{border-color:var(--p);background:#FFF8E1}
.car .i{font-size:24px}.car .n{font-size:12px;font-weight:800}
.summary{background:linear-gradient(135deg,#FFF8E1,#FFF3C4);padding:15px;border-radius:16px;margin:15px 0;display:flex;justify-content:space-between}
.summary .lbl{font-size:12px;color:#92400E;font-weight:700}
.summary .val{font-size:20px;font-weight:800}
/* حالات الرحلة */
.center{text-align:center;padding:20px 0}
.radar{width:80px;height:80px;border-radius:50%;background:#FFF8E1;margin:0 auto 15px;display:flex;align-items:center;justify-content:center;font-size:36px;animation:pulse 1.5s infinite}
@keyframes pulse{0%,100%{transform:scale(1)}50%{transform:scale(1.1)}}
.driver-card{display:flex;align-items:center;gap:12px;background:var(--bg);padding:15px;border-radius:16px;margin-bottom:15px}
.driver-card .dav{width:50px;height:50px;border-radius:50%;background:var(--p);display:flex;align-items:center;justify-content:center;font-size:24px}
.progress{height:8px;background:#eee;border-radius:9px;overflow:hidden;margin:15px 0}
.p-bar{height:100%;background:var(--p);width:0;transition:.2s}
.stars{display:flex;justify-content:center;gap:8px;font-size:30px;margin:15px 0;cursor:pointer}
.stars span{opacity:.3;filter:grayscale(1)}
.stars span.on{opacity:1;filter:none}
/* السائق */
.switch{position:relative;width:50px;height:28px}
.switch input{opacity:0;width:0;height:0}
.slider{position:absolute;inset:0;background:#ccc;border-radius:99px;transition:.3s}
.slider:before{content:"";position:absolute;width:20px;height:20px;background:#fff;border-radius:50%;top:4px;left:4px;transition:.3s}
input:checked+.slider{background:var(--grn)}
input:checked+.slider:before{transform:translateX(22px)}
/* الإشعارات */
.toast{position:fixed;bottom:30px;left:50%;transform:translate(-50%,100px);background:var(--d);color:#fff;padding:12px 20px;border-radius:12px;font-size:14px;font-weight:700;z-index:9999;opacity:0;transition:.3s;pointer-events:none}
.toast.show{opacity:1;transform:translate(-50%,0)}
.toast.ok{background:var(--grn)}.toast.err{background:var(--red)}
/* الخريطة الكاملة */
#map-picker{position:fixed;inset:0;z-index:2000;background:#fff;display:none;flex-direction:column}
#pick-map{flex:1}
.pick-head{position:absolute;top:0;left:0;right:0;padding:15px;background:#fffc;z-index:10;display:flex;align-items:center;gap:10px;box-shadow:0 4px 10px #0001}
.pin{position:absolute;top:50%;left:50%;transform:translate(-50%,-100%);font-size:40px;z-index:10;animation:up .5s infinite alternate}
.pick-foot{position:absolute;bottom:0;left:0;right:0;padding:20px;background:#fff;z-index:10;border-radius:24px 24px 0 0;text-align:center;box-shadow:0 -5px 20px #0001}
</style>
</head>
<body>
<div id="app">
  <div id="boot"><div style="font-size:60px;margin-bottom:15px">🚕</div><div>جاري التحميل...</div><div class="sp"></div></div>
</div>
<div class="toast" id="toast"></div>
<div id="map-picker">
  <div class="pick-head"><button class="close" onclick="closePick()">✕</button><h3>حدد الموقع</h3></div>
  <div id="pick-map"></div><div class="pin">📍</div>
  <div class="pick-foot"><h3 id="pick-name" style="margin-bottom:15px">جاري التحديد...</h3><button class="btn btn-p" onclick="savePick()">تأكيد الموقع</button></div>
</div>
<script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>
<script>
/* ================= 1. البيانات وقاعدة البيانات ================= */
const GOVS=[{n:'بغداد',a:['الكرادة','المنصور','الأعظمية','زيونة','الجادرية','الحارثية']},{n:'البصرة',a:['العشار','الجنينة','الزبير','شط العرب']},{n:'أربيل',a:['مركز أربيل','عنكاوا','سوران']},{n:'النجف',a:['النجف القديمة','الكوفة','الحيرة']},{n:'كربلاء',a:['مركز كربلاء','الحر','الحسينية']}]; // مختصرة لتخفيف الكود
const CARS={e:{n:'اقتصادي',i:'🚗',m:1},c:{n:'مريح',i:'🚙',m:1.3},v:{n:'VIP',i:'🚘',m:1.8}};
const DB={
  k:'taxi_v14', l(){try{return JSON.parse(localStorage.getItem(this.k))||{u:[],s:null,r:null}}catch{return{u:[],s:null,r:null}}},
  s(d){localStorage.setItem(this.k,JSON.stringify(d))},
  reg(n,p,pw,g,ro){let d=this.l();if(d.u.find(x=>x.p===p))throw 'الرقم مسجل';if(!/^07\d{9}$/.test(p))throw 'رقم غير صحيح';let u={id:Date.now(),n,p,pw,g,ro,rides:[]};d.u.push(u);this.s(d);return u},
  log(p,pw){let d=this.l(),u=d.u.find(x=>x.p===p);if(!u)throw 'لا يوجد حساب';if(u.pw!==pw)throw 'كلمة المرور خاطئة';return u},
  sess(u,rem){let d=this.l();d.s={id:u.id,exp:rem?Date.now()+31536000000:Date.now()+86400000};this.s(d)},
  getSess(){let d=this.l();if(!d.s||d.s.exp<Date.now())return null;return d.u.find(x=>x.id===d.s.id)||null},
  out(){let d=this.l();d.s=null;this.s(d)},
  req(r){let d=this.l();d.r={...r,st:'pending'};this.s(d)},
  getReq(){return this.l().r}, setReq(st){let d=this.l();if(d.r)d.r.st=st;this.s(d)},
  clearReq(){let d=this.l();d.r=null;this.s(d)},
  addHist(id,r){let d=this.l(),u=d.u.find(x=>x.id===id);if(u){u.rides=u.rides||[];u.rides.unshift(r);this.s(d)}}
};

/* ================= 2. الحالة والأدوات ================= */
const S={u:null,ro:'passenger',st:'book',pg:'بغداد',pa:'الكرادة',plat:33.315,plng:44.366,dg:'بغداد',da:'المنصور',dlat:33.32,dlng:44.37,car:'e',dist:0,price:0,drv:null,prog:0,arr:false,on:false};
let map,m2,pin1,pin2,carM,route,tgt,step,coords,intv,dtm;

function toast(m,t=''){let e=document.getElementById('toast');e.className='toast show '+t;e.textContent=m;setTimeout(()=>e.classList.remove('show'),2500)}
function fmt(n){return Number(n).toLocaleString()}
function init(n){let p=n.split(' ');return((p[0]?.[0]||'')+(p[1]?.[0]||'')).toUpperCase()}
function getC(g,a){let gi=GOVS.findIndex(x=>x.n===g),ai=GOVS[gi]?.a.indexOf(a)||0;return{lat:33.315+Math.cos(ai*2)*0.05,lng:44.366+Math.sin(ai*2)*0.05}}
function haversine(l1,o1,l2,o2){let R=6371,dL=(l2-l1)*Math.PI/180,dO=(o2-o1)*Math.PI/180,a=Math.sin(dL/2)**2+Math.cos(l1*Math.PI/180)*Math.cos(l2*Math.PI/180)*Math.sin(dO/2)**2;return R*2*Math.atan2(Math.sqrt(a),Math.sqrt(1-a))}
function calcP(k,t){return Math.round((2000+k*800)*CARS[t].m/250)*250}

/* ================= 3. المصادقة ================= */
function renderAuth(){
  document.getElementById('boot').classList.add('hide');
  document.getElementById('app').innerHTML=`
  <div class="auth"><div class="card">
    <h1 style="text-align:center;margin-bottom:20px">تاكسي أحمد 🚕</h1>
    <div class="roles" id="roles">
      <div class="role active" data-r="passenger">🙋‍♂️ راكب</div>
      <div class="role" data-r="driver">🚕 سائق</div>
    </div>
    <div class="tabs"><button class="tab active" data-t="login">دخول</button><button class="tab" data-t="reg">حساب جديد</button></div>
    <form id="f">
      <div class="fg" id="fg-n" style="display:none"><label>الاسم</label><input type="text" id="i-n" placeholder="أحمد كريم"></div>
      <div class="fg"><label>رقم الهاتف</label><input type="tel" id="i-p" placeholder="07XXXXXXXXX" maxlength="11"></div>
      <div class="fg"><label>الرمز السري</label><input type="password" id="i-pw" placeholder="••••"></div>
      <div class="fg" id="fg-g" style="display:none"><label>المحافظة</label><select id="i-g">${GOVS.map(g=>`<option>${g.n}</option>`)}</select></div>
      <button type="submit" class="btn btn-p" id="sub">دخول</button>
    </form>
  </div></div>`;

  let curR='passenger',curT='login';
  document.querySelectorAll('.role').forEach(e=>e.onclick=()=>{document.querySelectorAll('.role').forEach(x=>x.classList.remove('active'));e.classList.add('active');curR=e.dataset.r});
  document.querySelectorAll('.tab').forEach(e=>e.onclick=()=>{document.querySelectorAll('.tab').forEach(x=>x.classList.remove('active'));e.classList.add('active');curT=e.dataset.t;document.getElementById('fg-n').style.display=curT==='reg'?'block':'none';document.getElementById('fg-g').style.display=curT==='reg'?'block':'none';document.getElementById('sub').textContent=curT==='reg'?'إنشاء':'دخول'});
  
  document.getElementById('f').onsubmit=(e)=>{
    e.preventDefault();
    try{
      let p=document.getElementById('i-p').value.trim(),pw=document.getElementById('i-pw').value,u;
      if(curT==='reg') u=DB.reg(document.getElementById('i-n').value,p,pw,document.getElementById('i-g').value,curR);
      else{u=DB.log(p,pw);if(u.ro!==curR)throw 'نوع الحساب غير مطابق'}
      DB.sess(u,true);S.u=u;S.ro=u.ro;renderApp();
    }catch(err){toast(err,'err')}
  };
}

/* ================= 4. الواجهة الرئيسية ================= */
function renderApp(){
  document.getElementById('app').innerHTML=`
  <div id="map"></div>
  <div class="top"><button class="chip ${S.ro==='driver'?'driver':''}" onclick="profile()">
    <div class="av">${init(S.u.n)}</div><div style="text-align:right"><div style="font-weight:800;font-size:14px">${S.u.n.split(' ')[0]}</div><div style="font-size:11px;color:var(--g)">📍 ${S.u.g} (${S.ro==='driver'?'سائق':'راكب'})</div></div>
  </button></div>
  <button class="open-btn btn btn-p" id="open-btn" onclick="toggle(true)">عرض التفاصيل</button>
  <div class="sheet" id="sheet"></div>`;

  setTimeout(()=>{
    if(!window.L){document.getElementById('map').innerHTML='<div style="padding:20px;text-align:center;color:red">الخريطة غير متاحة، تأكد من النت</div>';renderSheet();return}
    map=L.map('map',{zoomControl:false,attributionControl:false}).setView([S.plat,S.plng],12);
    L.tileLayer('https://server.arcgisonline.com/ArcGIS/rest/services/World_Street_Map/MapServer/tile/{z}/{y}/{x}',{maxZoom:19}).addTo(map);
    setTimeout(()=>{map.invalidateSize();if(S.ro==='passenger')upPins()},300);
    renderSheet();
  },200);
  document.getElementById('boot').classList.add('hide');
}

function upPins(){
  if(!map)return;
  if(pin1)map.removeLayer(pin1);if(pin2)map.removeLayer(pin2);
  pin1=L.marker([S.plat,S.plng],{icon:L.divIcon({html:'<div style="width:20px;height:20px;border-radius:50%;background:var(--grn);border:3px solid #fff"></div>',className:'',iconSize:[20,20],iconAnchor:[10,10]})}).addTo(map);
  pin2=L.marker([S.dlat,S.dlng],{icon:L.divIcon({html:'<div style="width:20px;height:20px;border-radius:50%;background:var(--red);border:3px solid #fff"></div>',className:'',iconSize:[20,20],iconAnchor:[10,10]})}).addTo(map);
  if(!route){let b=L.featureGroup([pin1,pin2]).getBounds();if(b.isValid())map.fitBounds(b,{padding:[80,80],maxZoom:13})}
}

function toggle(o){document.getElementById('sheet').style.transform=o?'translateY(0)':'translateY(100%)';document.getElementById('open-btn').style.display=o?'none':'block'}

function renderSheet(){
  if(S.ro==='passenger'){
    if(S.st==='book')renderBook(); else if(S.st==='search')renderSearch(); else if(S.st==='found')renderFound(); else if(S.st==='ride')renderRide(); else if(S.st==='done')renderDone();
  }else{ S.st==='book'?renderDriver():renderDride(); }
}

/* ================= 5. شاشات الراكب ================= */
function renderBook(){
  toggle(true);
  document.getElementById('sheet').innerHTML=`
  <button class="close" onclick="toggle(false)">✕</button><div class="handle"></div><div class="title">وين نوصلك؟</div>
  <div class="row"><div class="dot g"></div><div class="loc-info" onclick="openList('p')"><div class="l">من</div><div class="v">${S.pa}، ${S.pg}</div></div><button class="map-btn" onclick="openPick('p')">📍</button></div>
  <div class="row"><div class="dot r"></div><div class="loc-info" onclick="openList('d')"><div class="l">إلى</div><div class="v">${S.da}، ${S.dg}</div></div><button class="map-btn" onclick="openPick('d')">📍</button></div>
  <div class="cars">${Object.keys(CARS).map(k=>`<div class="car ${S.car===k?'active':''}" onclick="setCar('${k}')"><div class="i">${CARS[k].i}</div><div class="n">${CARS[k].n}</div></div>`).join('')}</div>
  <div class="summary"><div><div class="lbl">المسافة</div><div class="val" style="font-size:16px">${S.dist.toFixed(1)} كم</div></div><div style="text-align:left"><div class="lbl">السعر</div><div class="val">${fmt(S.price)}<span style="font-size:12px;color:var(--g)"> د.ع</span></div></div></div>
  <button class="btn btn-p" onclick="reqTaxi()">🚕 اطلب التكسي</button>`;
  if(S.dist===0)drawRoute();
}

function setCar(k){S.car=k;S.price=calcP(S.dist,k);renderBook()}
function renderSearch(){toggle(true);document.getElementById('sheet').innerHTML=`<div class="handle"></div><div class="center"><div class="radar">🚕</div><h3>جاري البحث عن سائق...</h3></div>`}

function renderFound(){
  toggle(true);
  document.getElementById('sheet').innerHTML=`
  <div class="handle"></div>
  <div style="background:var(--d);color:#fff;padding:15px;border-radius:16px;display:flex;justify-content:space-between;margin-bottom:15px">
    <div><div style="font-size:12px;opacity:.8">${S.arr?'السائق وصل':'وقت الوصول'}</div><div style="font-size:20px;font-weight:800;color:var(--p)">${S.arr?'🎉':'5 دقائق'}</div></div><div style="font-size:30px">🚕</div>
  </div>
  <div class="driver-card"><div class="dav">${S.drv.i}</div><div style="flex:1"><div style="font-weight:800">${S.drv.n}</div><div style="font-size:12px;color:var(--g)">${S.drv.c} • ${S.drv.pl}</div></div><button class="map-btn" style="background:var(--grn);color:#fff;border:none" onclick="location.href='tel:${S.drv.ph}'">📞</button></div>
  <button class="btn ${S.arr?'btn-p':'btn-r'}" onclick="${S.arr?'startRide()':'cancelRide()'}">${S.arr?'🚀 بدء الرحلة':'إلغاء'}</button>`;
}

function renderRide(){
  toggle(true);let r=Math.max(1,Math.round((1-S.prog/100)*S.dist*2));
  document.getElementById('sheet').innerHTML=`
  <div class="handle"></div><div class="title" style="font-size:16px">🚕 في الطريق إلى ${S.da}</div><p style="font-size:13px;color:var(--g);margin-bottom:10px">الوصول بعد ${r} دقيقة</p>
  <div class="progress"><div class="p-bar" id="bar" style="width:${S.prog}%"></div></div>
  <div class="summary"><div><div class="lbl">الكلفة</div><div class="val">${fmt(S.price)}<span style="font-size:12px;color:var(--g)"> د.ع</span></div></div></div>`;
}

function renderDone(){
  toggle(true);
  document.getElementById('sheet').innerHTML=`
  <div class="handle"></div><div class="center"><div style="font-size:50px">🎉</div><h3>وصلت بالسلامة!</h3></div>
  <div class="summary"><div><div class="lbl">الإجمالي</div><div class="val">${fmt(S.price)}<span style="font-size:12px;color:var(--g)"> د.ع</span></div></div></div>
  <div style="text-align:center;font-size:13px;font-weight:800;margin-bottom:5px">قيّم السائق</div>
  <div class="stars" id="stars">${[1,2,3,4,5].map(i=>`<span onclick="rate(${i})">⭐</span>`).join('')}</div>
  <button class="btn btn-p" onclick="endRide()">تم ✅</button>`;
  rate(5);
}
function rate(v){document.querySelectorAll('#stars span').forEach((s,i)=>s.classList.toggle('on',i<v))}

/* ================= 6. منطق الرحلة ================= */
function reqTaxi(){
  if(S.pa===S.da&&S.pg===S.dg)return toast('اختر مناطق مختلفة','err');
  S.st='search';renderSearch();
  DB.req({pn:S.u.n,pa:S.pa,pg:S.pg,da:S.da,dg:S.dg,price:S.price,dist:S.dist});
  setTimeout(()=>{let r=DB.getReq();if(r&&r.st==='pending'){DB.setReq('acc');foundSim()}},5000);
}

function foundSim(){
  if(S.st!=='search')return;
  S.drv={n:'أحمد كريم',ph:'077...',c:'تويوتا',pl:'٢٤٧٨١',i:'👨'};S.st='found';S.arr=false;renderFound();
  if(map){
    let sl=S.plat+.02,so=S.plng+.02;
    if(carM)map.removeLayer(carM);carM=L.marker([sl,so],{icon:L.divIcon({html:'🚕',className:'',iconSize:[40,40],iconAnchor:[20,20]})}).addTo(map);
    let t0=Date.now();let tm=setInterval(()=>{let t=Math.min(1,(Date.now()-t0)/4000);carM.setLatLng([sl+(S.plat-sl)*t,so+(S.plng-so)*t]);if(t>=1){clearInterval(tm);S.arr=true;if(S.st==='found')renderFound();toast('السائق وصل 🎉','ok')}},50);
  }
}

function startRide(){
  S.st='ride';S.prog=0;renderRide();
  let t0=Date.now(),dur=Math.max(8000,S.dist*1000);
  intv=setInterval(()=>{
    let t=Math.min(1,(Date.now()-t0)/dur);S.prog=Math.round(t*100);
    let b=document.getElementById('bar');if(b)b.style.width=S.prog+'%';
    if(route&&carM){let c=route.getLatLngs();if(c.length>1){let idx=t*(c.length-1),i=Math.floor(idx),f=idx-i;if(i<c.length-1)carM.setLatLng([c[i].lat+(c[i+1].lat-c[i].lat)*f,c[i].lng+(c[i+1].lng-c[i].lng)*f])}}
    if(t>=1){clearInterval(intv);S.st='done';DB.clearReq();renderDone()}
  },100);
}

function cancelRide(){DB.clearReq();reset();upPins();toast('تم الإلغاء')}
function endRide(){DB.addHist(S.u.id,{f:S.pa,t:S.da,p:S.price,d:S.drv?.n||'سائق',r:5,dt:new Date().toLocaleDateString()});reset();upPins();toast('شكراً لك 🌟','ok')}
function reset(){S.st='book';S.drv=null;S.prog=0;S.arr=false;if(intv)clearInterval(intv);if(route){map.removeLayer(route);route=null}if(carM){map.removeLayer(carM);carM=null}}

/* ================= 7. اختيار المواقع ================= */
function openList(t){
  tgt=t;step='g';
  document.getElementById('loc-list-title').textContent=t==='p'?'اختر المحافظة':'اختر الوجهة';
  document.getElementById('loc-list-modal').classList.add('show');renderList();
}
function renderList(){
  let c=document.getElementById('loc-list-container'),cg=tgt==='p'?S.pg:S.dg;
  if(step==='g')c.innerHTML=GOVS.map(g=>`<li class="loc-list-item" onclick="selG('${g.n}')"><span>${g.n}</span><span style="color:#aaa">◀</span></li>`).join('');
  else{let g=GOVS.find(x=>x.n===cg);c.innerHTML=`<li class="loc-list-item" style="background:#f0f0f0" onclick="step='g';renderList()"><span>🔙 رجوع</span></li>`+g.a.map(a=>`<li class="loc-list-item" onclick="selA('${a}')"><span>${a}</span></li>`).join('')}
}
function selG(g){if(tgt==='p')S.pg=g;else S.dg=g;step='a';renderList()}
function selA(a){
  let g=tgt==='p'?S.pg:S.dg,c=getC(g,a);
  if(tgt==='p'){S.pa=a;S.plat=c.lat;S.plng=c.lng}else{S.da=a;S.dlat=c.lat;S.dlng=c.lng}
  document.getElementById('loc-list-modal').classList.remove('show');upPins();drawRoute();renderBook();
}
function openPick(t){
  tgt=t;document.getElementById('map-picker').style.display='flex';
  setTimeout(()=>{
    if(!m2){m2=L.map('pick-map',{zoomControl:false}).setView([33.315,44.366],14);L.tileLayer('https://server.arcgisonline.com/ArcGIS/rest/services/World_Street_Map/MapServer/tile/{z}/{y}/{x}',{maxZoom:19}).addTo(m2);m2.on('moveend',upC)}
    m2.invalidateSize();m2.setView([t==='p'?S.plat:S.dlat,t==='p'?S.plng:S.dlng],15);upC();
  },300);
}
function upC(){
  if(!m2)return;let c=m2.getCenter(),mn=Infinity,gn='بغداد',an='الكرادة';
  GOVS.forEach(g=>g.a.forEach(a=>{let gc=getC(g.n,a),d=haversine(c.lat,c.lng,gc.lat,gc.lng);if(d<mn){mn=d;gn=g.n;an=a}}));
  S.tmp={lat:c.lat,lng:c.lng,n:an,g:gn};document.getElementById('pick-name').textContent=an+'، '+gn;
}
function savePick(){
  if(!S.tmp)return;let p=S.tmp;
  if(tgt==='p'){S.pa=p.n;S.pg=p.g;S.plat=p.lat;S.plng=p.lng}else{S.da=p.n;S.dg=p.g;S.dlat=p.lat;S.dlng=p.lng}
  closePick();upPins();drawRoute();renderBook();
}
function closePick(){document.getElementById('map-picker').style.display='none'}

async function drawRoute(){
  try{
    let u=`https://router.project-osrm.org/route/v1/driving/${S.plng},${S.plat};${S.dlng},${S.dlat}?overview=full&geometries=geojson`;
    let r=await fetch(u),d=await r.json();
    if(d.routes&&d.routes[0]){
      S.dist=d.routes[0].distance/1000;S.price=calcP(S.dist,S.car);
      if(map){if(route)map.removeLayer(route);route=L.polyline(d.routes[0].geometry.coordinates.map(c=>[c[1],c[0]]),{color:'#FFC107',weight:5}).addTo(map);map.fitBounds(route.getBounds(),{padding:[60,60]})}
    }
  }catch(e){S.dist=haversine(S.plat,S.plng,S.dlat,S.dlng)*1.3;S.price=calcP(S.dist,S.car);if(map){if(route)map.removeLayer(route);route=L.polyline([[S.plat,S.plng],[S.dlat,S.dlng]],{color:'#FFC107',weight:5,dashArray:'10,10'}).addTo(map)}}
  if(S.st==='book')renderBook();
}

/* ================= 8. واجهة السائق ================= */
function renderDriver(){
  toggle(true);
  document.getElementById('sheet').innerHTML=`
  <div class="handle"></div><div class="title">أهلاً كابتن ${S.u.n.split(' ')[0]}</div>
  <div style="display:flex;justify-content:space-between;align-items:center;background:var(--bg);padding:15px;border-radius:16px;margin-bottom:15px">
    <div><div style="font-weight:800">حالة الاتصال</div><div style="font-size:12px;color:var(--g)">${S.on?'متصل':'غير متصل'}</div></div>
    <label class="switch"><input type="checkbox" ${S.on?'checked':''} onchange="toggleOn(this.checked)"><span class="slider"></span></label>
  </div>
  ${S.on?`<div class="center"><div class="radar">📡</div><h3>جاري البحث عن طلبات...</h3></div>`:`<div class="center" style="opacity:.5"><div style="font-size:40px">💤</div><h3>قم بتفعيل الاتصال</h3></div>`}`;
}
function toggleOn(o){S.on=o;renderDriver();if(o){dtm=setTimeout(()=>{if(!S.on)return;let r=DB.getReq();if(!r||r.st!=='pending')r={pn:'راكب',pa:'الكرادة',pg:'بغداد',da:'المنصور',dg:'بغداد',price:8500,dist:7};showReq(r)},5000)}else clearTimeout(dtm)}
function showReq(r){
  toggle(true);
  document.getElementById('sheet').innerHTML=`
  <div class="handle"></div><div class="title">🚨 طلب جديد!</div>
  <div style="background:#fff;border:2px solid var(--blu);padding:15px;border-radius:16px;margin-bottom:15px">
    <div style="display:flex;justify-content:space-between;font-weight:800;margin-bottom:10px"><span>🙋‍♂️ ${r.pn}</span><span style="color:var(--grn)">${fmt(r.price)} د.ع</span></div>
    <div style="font-size:13px;font-weight:700;margin-bottom:5px">📍 ${r.pa} ➔ ${r.da}</div><div style="font-size:12px;color:var(--g)">المسافة: ${r.dist} كم</div>
  </div>
  <button class="btn btn-b" onclick="accReq()" style="margin-bottom:10px">قبول</button>
  <button class="btn btn-r" onclick="rejReq()">تجاهل</button>`;
}
function accReq(){S.st='dride';DB.setReq('acc');renderDride();toast('تم القبول','ok')}
function rejReq(){DB.clearReq();S.st='book';renderDriver();toast('تم التجاهل')}
function renderDride(){
  toggle(true);
  document.getElementById('sheet').innerHTML=`
  <div class="handle"></div><div style="background:var(--blu);color:#fff;padding:15px;border-radius:16px;margin-bottom:15px"><div style="font-size:12px">أنت في رحلة</div><div style="font-size:18px;font-weight:800">جاري التوصيل</div></div>
  <button class="btn btn-p" onclick="endDRide()">🏁 إنهاء الرحلة</button>`;
}
function endDRide(){DB.clearReq();S.st='book';renderDriver();toast('تم إنهاء الرحلة 💸','ok')}

/* ================= 9. الحساب والمزامنة ================= */
function profile(){
  let r=S.u.rides||[];
  document.getElementById('profile-modal-content').innerHTML=`
  <div class="handle"></div><div style="text-align:center;margin-bottom:15px"><div class="av" style="width:70px;height:70px;font-size:28px;margin:0 auto 10px">${init(S.u.n)}</div><div style="font-weight:800;font-size:18px">${S.u.n}</div><div style="color:var(--g)">${S.u.p}</div></div>
  <div style="display:grid;grid-template-columns:1fr 1fr;gap:10px;margin-bottom:15px"><div style="background:var(--bg);padding:15px;border-radius:12px;text-align:center"><div style="font-weight:800">${r.length}</div><div style="font-size:12px;color:var(--g)">رحلة</div></div><div style="background:var(--bg);padding:15px;border-radius:12px;text-align:center"><div style="font-weight:800">${fmt(r.reduce((s,x)=>s+x.p,0))}</div><div style="font-size:12px;color:var(--g)">مصروف</div></div></div>
  <button class="btn btn-r" onclick="logout()">خروج</button><button class="btn btn-ghost" style="margin-top:10px" onclick="document.getElementById('profile-modal').classList.remove('show')">إغلاق</button>`;
  document.getElementById('profile-modal').classList.add('show');
}
function logout(){if(!confirm('تريد الخروج؟'))return;DB.out();S.u=null;if(map){map.remove();map=null}renderAuth()}

window.addEventListener('storage',e=>{
  if(e.key===DB.k&&S.u){
    let r=DB.getReq();
    if(S.ro==='driver'&&S.on&&r&&r.st==='pending'&&S.st==='book'){clearTimeout(dtm);showReq(r)}
    if(S.ro==='passenger'&&S.st==='search'&&r&&r.st==='acc'){foundSim()}
  }
});

/* ================= 10. التشغيل ================= */
setTimeout(()=>{
  try{
    let u=DB.getSess();
    if(!window.L){document.getElementById('boot').innerHTML='<div style="text-align:center;color:red;font-weight:bold">تعذر تحميل الخريطة، تأكد من النت</div>';return}
    if(u){S.u=u;S.ro=u.ro;renderApp()}else renderAuth();
  }catch(e){document.getElementById('boot').innerHTML='<div style="text-align:center"><div style="color:red;font-weight:bold">حدث خطأ</div><button onclick="location.reload()" style="margin-top:10px;padding:8px 16px;border:none;border-radius:8px;background:#fff">إعادة المحاولة</button></div>'}
},1000);
</script>
</body>
</html>
