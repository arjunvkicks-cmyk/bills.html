<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
  <title>PUMA Bill Manager</title>
  <style>
    *{box-sizing:border-box;margin:0;padding:0}
    body{font-family:'Segoe UI',sans-serif;background:#f0f4f8;min-height:100vh;display:flex;justify-content:center;padding:20px 0 60px}
    .card{background:#fff;border-radius:12px;padding:22px 18px;box-shadow:0 4px 20px rgba(0,0,0,0.1);width:390px;text-align:center;height:fit-content}
    h2{color:#c62828;margin-bottom:3px;font-size:1.25rem;letter-spacing:1px}
    .sub{font-size:0.78rem;color:#888;margin-bottom:14px}
    h3{color:#555;margin-bottom:10px;font-size:0.95rem}

    /* TABS */
    .tabs{display:flex;gap:3px;margin-bottom:14px;background:#f0f4f8;border-radius:8px;padding:4px}
    .tab{flex:1;padding:7px 2px;border:none;border-radius:6px;font-size:0.7rem;font-weight:700;cursor:pointer;background:transparent;color:#666;transition:all 0.2s}
    .tab.active{background:#c62828;color:#fff}
    .tc{display:none}.tc.active{display:block}

    /* INPUTS */
    input,select,textarea{width:100%;padding:9px 12px;border:1px solid #ddd;border-radius:8px;margin-bottom:9px;font-size:0.9rem;font-family:inherit;background:#fff;color:#222}
    input:focus,select:focus,textarea:focus{outline:none;border-color:#c62828}
    textarea{resize:vertical;min-height:60px}
    .lbl{font-size:0.75rem;color:#555;display:block;text-align:left;margin-bottom:3px;font-weight:700}
    .req{color:#c62828}
    .amt{position:relative}.amt input{padding-left:24px!important;font-weight:700}
    .rs{position:absolute;left:10px;top:50%;transform:translateY(-60%);color:#888;font-weight:700;pointer-events:none}

    /* BILL PILLS */
    .pills{display:flex;flex-wrap:wrap;gap:6px;margin-bottom:12px;justify-content:center}
    .pill{padding:7px 11px;border-radius:18px;font-size:0.74rem;font-weight:700;cursor:pointer;border:2px solid #eee;background:#f8f8f8;color:#555;transition:all 0.18s;user-select:none}
    .pill:hover{border-color:#c62828;color:#c62828}
    .pill.on{color:#fff!important;border-color:transparent!important;transform:scale(1.04)}
    .pr{background:#1565c0}.ph{background:#00838f}.pe{background:#f57f17}
    .pc{background:#558b2f}.pm{background:#6a1b9a}.pn{background:#c62828}
    .pt{background:#4a148c}.px{background:#37474f}

    /* SECTION CARDS */
    .sc{background:#f8f9ff;border:1px solid #e3e8f0;border-radius:10px;padding:12px 12px 4px;margin-bottom:10px;text-align:left}
    .sc-t{font-size:0.68rem;font-weight:700;color:#999;letter-spacing:1.5px;text-transform:uppercase;margin-bottom:8px}
    .g2{display:grid;grid-template-columns:1fr 1fr;gap:0 10px}

    /* BUTTONS */
    button{width:100%;padding:10px;border:none;border-radius:8px;font-size:0.92rem;cursor:pointer;margin-bottom:8px;font-weight:700;font-family:inherit;transition:opacity 0.2s}
    button:hover{opacity:0.87}
    button:disabled{opacity:0.4;cursor:not-allowed}
    #loginBtn{background:#c62828;color:#fff}
    #submitBtn{background:#c62828;color:#fff;font-size:1rem;padding:13px}
    #clearBtn{background:#f0f4f8;color:#666;font-size:0.83rem}
    #logoutBtn{background:#9e9e9e;color:#fff;font-weight:400}
    .b-grn{background:#2e7d32;color:#fff;font-size:0.83rem;padding:8px}
    .b-blu{background:#1565c0;color:#fff;font-size:0.8rem;padding:7px;margin-bottom:10px}

    /* IMAGE */
    .iz{border:2px dashed #ddd;border-radius:10px;padding:14px;text-align:center;cursor:pointer;margin-bottom:9px;position:relative;transition:all 0.2s}
    .iz:hover,.iz.on{border-color:#c62828;background:#fff5f5}
    .iz input[type=file]{position:absolute;inset:0;opacity:0;cursor:pointer;width:100%;height:100%}
    #imgPrev{width:100%;max-height:150px;object-fit:cover;border-radius:8px;display:none;margin-bottom:5px;border:2px solid #43a047}
    #chgImg{background:#fff3e0;color:#e65100;font-size:0.78rem;padding:6px;display:none;margin-bottom:9px}

    /* ALERTS */
    #statusMsg{font-size:0.82rem;color:#555;min-height:16px;margin-top:4px}
    .info-box{background:#e8f5e9;border-radius:8px;padding:7px 10px;font-size:0.78rem;color:#2e7d32;margin-bottom:8px;display:none;text-align:left}
    .err-box{background:#ffebee;border-radius:8px;padding:7px 10px;font-size:0.78rem;color:#c62828;margin-bottom:8px;display:none;text-align:left}
    .due-w{background:#ffebee;border-left:3px solid #c62828;border-radius:0 8px 8px 0;padding:5px 10px;font-size:0.75rem;color:#c62828;margin-bottom:9px;font-weight:700;display:none}
    .divider{border:none;border-top:1px solid #eee;margin:8px 0 12px}

    /* RECORDS */
    .rt{width:100%;border-collapse:collapse;font-size:0.72rem;margin-top:6px}
    .rt th{background:#c62828;color:#fff;padding:6px 4px;text-align:left}
    .rt td{padding:5px 4px;border-bottom:1px solid #eee;vertical-align:top}
    .rt tr:nth-child(even) td{background:#fafafa}
    .bg{padding:2px 6px;border-radius:10px;font-size:0.63rem;font-weight:700;white-space:nowrap;display:inline-block}
    .bgr{background:#e3f2fd;color:#1565c0}.bgh{background:#e0f7fa;color:#00838f}
    .bge{background:#fff8e1;color:#f57f17}.bgc{background:#f1f8e9;color:#558b2f}
    .bgm{background:#f3e5f5;color:#6a1b9a}.bgn{background:#ffebee;color:#c62828}
    .bgt{background:#ede7f6;color:#4a148c}.bgx{background:#eceff1;color:#37474f}
    .bpd{background:#e8f5e9;color:#2e7d32}.bpu{background:#ffebee;color:#c62828}.bpp{background:#fff3e0;color:#e65100}
    .vl{color:#c62828;font-size:0.68rem;text-decoration:none;font-weight:700}

    /* SUMMARY */
    .sg{display:grid;grid-template-columns:1fr 1fr;gap:8px;margin-bottom:12px}
    .sk{border-radius:8px;padding:10px 6px;text-align:center}
    .sk .n{font-size:1.25rem;font-weight:700}
    .sk .l{font-size:0.6rem;font-weight:700;margin-top:2px}

    /* POPUP */
    #pop{display:none;position:fixed;inset:0;background:rgba(0,0,0,0.5);z-index:999;justify-content:center;align-items:center}
    #pop.on{display:flex}
    #pbox{background:#fff;border-radius:16px;padding:24px 20px;text-align:center;width:285px;box-shadow:0 8px 32px rgba(0,0,0,0.2);animation:pi 0.28s ease}
    @keyframes pi{from{transform:scale(0.7);opacity:0}to{transform:scale(1);opacity:1}}
    #pico{font-size:2.3rem;margin-bottom:7px}
    #pttl{font-size:1.05rem;font-weight:700;color:#c62828;margin-bottom:5px}
    #pmsg{font-size:0.82rem;color:#444;margin-bottom:14px;line-height:1.5;white-space:pre-line}
    #pbtn{background:#c62828;color:#fff;border:none;border-radius:8px;padding:8px 26px;font-size:0.92rem;cursor:pointer;font-weight:700;width:auto}

    .s-ttl{font-size:0.83rem;font-weight:700;color:#c62828;text-align:left;margin-bottom:7px}
    .empty{color:#aaa;font-size:0.83rem;padding:20px;text-align:center}
    .spin{color:#888;font-size:0.83rem;padding:18px}
  </style>
</head>
<body>

<div id="pop">
  <div id="pbox">
    <div id="pico">✅</div>
    <div id="pttl">Done!</div>
    <div id="pmsg"></div>
    <button id="pbtn" onclick="closePop()">OK</button>
  </div>
</div>

<div class="card">

  <!-- LOGIN -->
  <div id="loginDiv">
    <h2>🏪 PUMA Bill Manager</h2>
    <p class="sub">Store Bill Submission System</p>
    <input type="text"     id="loginId"   placeholder="Store ID (e.g. PUMA001)" autocapitalize="characters">
    <input type="password" id="loginPass" placeholder="Password">
    <button id="loginBtn" onclick="login()">Login →</button>
    <div id="statusMsg"></div>
  </div>

  <!-- APP -->
  <div id="appDiv" style="display:none">
    <h2>🏪 PUMA Bill Manager</h2>
    <h3 id="wMsg"></h3>

    <div class="tabs">
      <button class="tab active" onclick="sw('submit')">📤 Submit</button>
      <button class="tab"        onclick="sw('records')">📋 Records</button>
      <button class="tab"        onclick="sw('summary')">📊 Summary</button>
    </div>

    <!-- TAB SUBMIT -->
    <div id="tab-submit" class="tc active">
      <div id="infoBox" class="info-box"></div>
      <div id="errBox"  class="err-box"></div>

      <div class="sc">
        <div class="sc-t">🧾 Bill Type चुनो <span class="req">*</span></div>
        <div class="pills">
          <div class="pill" id="p-r" onclick="pick('r')">🏠 Rent</div>
          <div class="pill" id="p-h" onclick="pick('h')">❄️ HVAC</div>
          <div class="pill" id="p-e" onclick="pick('e')">⚡ Electricity</div>
          <div class="pill" id="p-c" onclick="pick('c')">🏢 CAM</div>
          <div class="pill" id="p-m" onclick="pick('m')">📣 Marketing</div>
          <div class="pill" id="p-n" onclick="pick('n')">📞 Landline & Net</div>
          <div class="pill" id="p-t" onclick="pick('t')">🧾 Tax Invoice</div>
          <div class="pill" id="p-x" onclick="pick('x')">📄 Other</div>
        </div>
      </div>

      <div id="prompt" style="padding:14px 0;color:#bbb;font-size:0.83rem">☝️ Upar se bill type select karo</div>

      <div id="formArea" style="display:none">

        <!-- Image -->
        <div class="sc">
          <div class="sc-t">📷 Bill Photo <span class="req">*</span></div>
          <div class="iz" id="iz">
            <input type="file" id="fileIn" accept="image/*" capture="environment" onchange="handleFile(event)">
            <img id="imgPrev">
            <div id="imgPH">
              <div style="font-size:1.8rem;margin-bottom:4px">📷</div>
              <div style="font-size:0.76rem;color:#888"><strong style="color:#333;display:block">Tap — Camera ya Gallery</strong>Max 5MB</div>
            </div>
          </div>
          <button id="chgImg" onclick="clearImg()">🔄 Change Image</button>
        </div>

        <!-- Basic -->
        <div class="sc">
          <div class="sc-t">📋 Basic Details</div>
          <label class="lbl">Store</label>
          <input id="sName" readonly style="background:#f5f5f5;color:#999">

          <div class="g2">
            <div><label class="lbl">Bill Date <span class="req">*</span></label>
              <input type="date" id="bDate" onchange="onDC()"></div>
            <div><label class="lbl">Month <span class="req">*</span></label>
              <select id="bMonth">
                <option value="">Month</option>
                <option>January</option><option>February</option><option>March</option>
                <option>April</option><option>May</option><option>June</option>
                <option>July</option><option>August</option><option>September</option>
                <option>October</option><option>November</option><option>December</option>
              </select></div>
          </div>

          <div class="g2">
            <div><label class="lbl">Year <span class="req">*</span></label>
              <select id="bYear"></select></div>
            <div><label class="lbl">Amount ₹ <span class="req">*</span></label>
              <div class="amt"><span class="rs">₹</span>
                <input type="number" id="bAmt" placeholder="0.00" inputmode="decimal" step="0.01"></div></div>
          </div>

          <label class="lbl">Invoice / Bill No.</label>
          <input id="invNo" placeholder="e.g. INV-2026-0034">
          <label class="lbl">Vendor / Supplier Name</label>
          <input id="vendr" placeholder="e.g. BSES Rajdhani">
        </div>

        <!-- Due & Payment -->
        <div class="sc">
          <div class="sc-t">📅 Due Date & Payment</div>
          <div class="g2">
            <div><label class="lbl">Due Date</label>
              <input type="date" id="dDate" onchange="chkDue()"></div>
            <div><label class="lbl">Status <span class="req">*</span></label>
              <select id="pStat" onchange="onPS()">
                <option value="">— Select —</option>
                <option value="Unpaid">❌ Unpaid</option>
                <option value="Paid">✅ Paid</option>
                <option value="Partial">⚠️ Partial</option>
              </select></div>
          </div>
          <div class="due-w" id="dueW">⚠️ Due date nikal gayi!</div>
          <div id="pFlds" style="display:none">
            <div class="g2">
              <div><label class="lbl">Paid ₹</label>
                <div class="amt"><span class="rs">₹</span>
                  <input type="number" id="aPaid" placeholder="0.00" inputmode="decimal" step="0.01"></div></div>
              <div><label class="lbl">Pay Date</label>
                <input type="date" id="pDate"></div>
            </div>
            <label class="lbl">Payment Mode</label>
            <select id="pMode">
              <option value="">— Select —</option>
              <option>Cash</option><option>Cheque</option>
              <option>NEFT / RTGS</option><option>UPI</option>
              <option>Credit Card</option><option>Debit Card</option>
            </select>
            <label class="lbl">Txn / Cheque Ref No.</label>
            <input id="txnR" placeholder="UTR / Cheque number">
          </div>
        </div>

        <!-- Dynamic Bill-Type Fields -->
        <div id="dynArea"></div>

        <!-- Remarks -->
        <div class="sc">
          <div class="sc-t">💬 Remarks</div>
          <textarea id="rmks" placeholder="Extra notes..."></textarea>
        </div>

        <button id="submitBtn" onclick="doSubmit()">📤 Submit Bill</button>
        <button id="clearBtn"  onclick="clearForm()">Clear Form</button>
        <hr class="divider">
        <button id="logoutBtn" onclick="logout()">Logout</button>
      </div>
    </div>

    <!-- TAB RECORDS -->
    <div id="tab-records" class="tc">
      <div class="s-ttl">📋 My Bill Records</div>
      <button class="b-grn" onclick="expCSV()">⬇️ Export CSV</button>
      <button class="b-blu" onclick="loadRecs(true)">🔄 Refresh</button>
      <div id="recWrap"><div class="spin">Loading…</div></div>
    </div>

    <!-- TAB SUMMARY -->
    <div id="tab-summary" class="tc">
      <div class="s-ttl">📊 Summary</div>
      <button class="b-blu" onclick="buildSum()">🔄 Refresh</button>
      <div class="sg" id="sg"></div>
      <div class="s-ttl" style="margin-top:6px">💸 Unpaid / Partial</div>
      <div id="unpWrap"></div>
      <div class="s-ttl" style="margin-top:10px">📅 By Bill Type</div>
      <div id="typWrap"></div>
    </div>
  </div>
</div>

<script>
// ════════════════════════
// ⚙️  APNA URL YAHAN PASTE KARO (Step 4 ke baad milega)
// ════════════════════════
const SCRIPT_URL = "https://script.google.com/macros/s/AKfycby4P649xFZnk2akWNa-0xA0pJwTXiE_vj61vBhz3FMfkgB4WckIqmulRUVFqQEbEP9X/exec";

// Bill types config — Tax Invoice + Other added
const BT = {
  r:{ label:"Rent",             icon:"🏠", cls:"pr", bdg:"bgr",
      fields:[
        {id:"landlord",  lbl:"Landlord Name",          type:"text",   ph:"Full name"},
        {id:"rentPeriod",lbl:"Rental Period",           type:"text",   ph:"e.g. March 2026"},
        {id:"agreeNo",   lbl:"Agreement No. (Optional)",type:"text",   ph:"Ref number"},
        {id:"escPct",    lbl:"Escalation % (if any)",   type:"number", ph:"e.g. 5"}
      ]},
  h:{ label:"HVAC",             icon:"❄️", cls:"ph", bdg:"bgh",
      fields:[
        {id:"hvacCo",   lbl:"Contractor / Company",    type:"text",   ph:"Contractor name"},
        {id:"svcType",  lbl:"Service Type",             type:"select", opts:["","AMC","Repair","Installation","Maintenance","Other"]},
        {id:"unitCnt",  lbl:"Units Serviced",           type:"number", ph:"e.g. 4"},
        {id:"svcDate",  lbl:"Service Date",             type:"date"},
        {id:"nxtSvc",   lbl:"Next Service Due",         type:"date"}
      ]},
  e:{ label:"Electricity",      icon:"⚡", cls:"pe", bdg:"bge",
      fields:[
        {id:"consNo",   lbl:"Consumer No.",             type:"text",   ph:"Consumer number"},
        {id:"board",    lbl:"Board / Utility Name",     type:"text",   ph:"e.g. BSES, Tata Power"},
        {id:"units",    lbl:"Units Consumed (kWh)",     type:"number", ph:"e.g. 1200"},
        {id:"rdFrom",   lbl:"Meter Reading — From",     type:"number", ph:"Previous"},
        {id:"rdTo",     lbl:"Meter Reading — To",       type:"number", ph:"Current"},
        {id:"tariff",   lbl:"Tariff Rate ₹/unit",       type:"number", ph:"e.g. 8.5"}
      ]},
  c:{ label:"CAM",              icon:"🏢", cls:"pc", bdg:"bgc",
      fields:[
        {id:"mallNm",   lbl:"Mall / Property Name",     type:"text",   ph:"e.g. DLF Promenade"},
        {id:"camType",  lbl:"CAM Type",                 type:"select", opts:["","Monthly","Quarterly","Annual","Special Assessment","Other"]},
        {id:"camPrd",   lbl:"CAM Period",               type:"text",   ph:"e.g. Q1 2026"},
        {id:"baseCAM",  lbl:"Base CAM Amount ₹",        type:"number", ph:"Base amount"},
        {id:"gstAmt",   lbl:"GST / Tax Amount ₹",       type:"number", ph:"Tax charged"}
      ]},
  m:{ label:"Marketing",        icon:"📣", cls:"pm", bdg:"bgm",
      fields:[
        {id:"campaign", lbl:"Campaign Name",            type:"text",   ph:"e.g. Diwali Campaign"},
        {id:"mktgType", lbl:"Marketing Type",           type:"select", opts:["","Mall Branding","Digital Ads","Flyers","Events","Social Media","Other"]},
        {id:"agency",   lbl:"Agency / Vendor",          type:"text",   ph:"Agency name"},
        {id:"campPrd",  lbl:"Campaign Period",          type:"text",   ph:"e.g. 01–15 Mar 2026"},
        {id:"budget",   lbl:"Approved Budget ₹",        type:"number", ph:"Sanctioned amount"}
      ]},
  n:{ label:"Landline & Internet", icon:"📞", cls:"pn", bdg:"bgn",
      fields:[
        {id:"provider", lbl:"Service Provider",         type:"text",   ph:"e.g. Airtel, Jio, BSNL"},
        {id:"acctNo",   lbl:"Account / Connection No.", type:"text",   ph:"Account number"},
        {id:"svcKind",  lbl:"Service Type",             type:"select", opts:["","Landline Only","Internet Only","Landline + Internet","Leased Line","Other"]},
        {id:"plan",     lbl:"Plan / Package Name",      type:"text",   ph:"e.g. 100 Mbps Unlimited"},
        {id:"billPrd",  lbl:"Billing Period",           type:"text",   ph:"e.g. March 2026"}
      ]},
  t:{ label:"Tax Invoice",      icon:"🧾", cls:"pt", bdg:"bgt",
      fields:[
        {id:"gstin",    lbl:"Supplier GSTIN",           type:"text",   ph:"e.g. 07AAAAA0000A1Z5"},
        {id:"taxInvNo", lbl:"Tax Invoice No.",          type:"text",   ph:"e.g. TXINV-2026-001"},
        {id:"taxInvDt", lbl:"Invoice Date",             type:"date"},
        {id:"hsnCode",  lbl:"HSN / SAC Code",           type:"text",   ph:"e.g. 9972"},
        {id:"taxType",  lbl:"Tax Type",                 type:"select", opts:["","IGST","CGST + SGST","GST 5%","GST 12%","GST 18%","GST 28%","Other"]},
        {id:"taxAmt",   lbl:"Tax Amount ₹",             type:"number", ph:"Total tax charged"},
        {id:"baseAmt",  lbl:"Base Amount (before tax) ₹",type:"number",ph:"Amount before tax"}
      ]},
  x:{ label:"Other",            icon:"📄", cls:"px", bdg:"bgx",
      fields:[
        {id:"expType",  lbl:"Expense Type / Category",  type:"text",   ph:"e.g. Stationery, Cleaning"},
        {id:"purpose",  lbl:"Purpose / Description",    type:"text",   ph:"Brief description"}
      ]}
};

const ALLCLS = ["pr","ph","pe","pc","pm","pn","pt","px"];
let store=null, recs=[], blob=null, sel=null;

// API
function callScript(action,data,cb){
  fetch(SCRIPT_URL,{method:"POST",body:JSON.stringify(Object.assign({action},data)),headers:{"Content-Type":"text/plain"}})
  .then(r=>r.json()).then(cb)
  .catch(err=>{dis(false);setSt("❌ Network error: "+err.message);});
}

// Helpers
function setSt(m){document.getElementById("statusMsg").textContent=m;}
function showI(m){const b=document.getElementById("infoBox");b.textContent=m;b.style.display=m?"block":"none";}
function showE(m){const b=document.getElementById("errBox"); b.textContent=m;b.style.display=m?"block":"none";}
function dis(d){["loginBtn","submitBtn","clearBtn"].forEach(id=>{const e=document.getElementById(id);if(e)e.disabled=d;});}
function td(d){return d.getFullYear()+"-"+String(d.getMonth()+1).padStart(2,"0")+"-"+String(d.getDate()).padStart(2,"0");}
function showPop(ic,ti,msg){document.getElementById("pico").textContent=ic;document.getElementById("pttl").textContent=ti;document.getElementById("pmsg").textContent=msg;document.getElementById("pop").classList.add("on");}
function closePop(){document.getElementById("pop").classList.remove("on");}

// Login
function login(){
  const id=document.getElementById("loginId").value.trim().toUpperCase();
  const pw=document.getElementById("loginPass").value.trim();
  if(!id||!pw){setSt("⚠️ Store ID aur Password bharo.");return;}
  setSt("Logging in…");dis(true);
  callScript("login",{storeId:id,password:pw},function(res){
    dis(false);
    if(res.success){
      store=res.store;
      document.getElementById("loginDiv").style.display="none";
      document.getElementById("appDiv").style.display="block";
      document.getElementById("wMsg").textContent="Welcome, "+store.name+" 👋";
      document.getElementById("sName").value=store.name;
      setSt("");initYrs();setDts();loadRecs(false);
    } else {setSt("❌ "+(res.message||"Invalid credentials."));}
  });
}
document.getElementById("loginPass").addEventListener("keydown",e=>{if(e.key==="Enter")login();});

function initYrs(){
  const el=document.getElementById("bYear"),y=new Date().getFullYear();
  el.innerHTML="";
  for(let i=y;i>=y-3;i--)el.innerHTML+=`<option value="${i}">${i}</option>`;
}
function setDts(){
  const now=new Date();
  document.getElementById("bDate").value=td(now);
  document.getElementById("pDate").value=td(now);
  const mn=["January","February","March","April","May","June","July","August","September","October","November","December"];
  document.getElementById("bMonth").value=mn[now.getMonth()];
  document.getElementById("bYear").value=String(now.getFullYear());
}

// Tabs
function sw(t){
  ["submit","records","summary"].forEach((n,i)=>{
    document.querySelectorAll(".tab")[i].classList.toggle("active",n===t);
    document.getElementById("tab-"+n).classList.toggle("active",n===t);
  });
  if(t==="records")loadRecs(false);
  if(t==="summary")buildSum();
}

// Bill type pick
function pick(k){
  sel=k;
  Object.keys(BT).forEach(b=>{
    const p=document.getElementById("p-"+b);
    p.classList.remove("on",...ALLCLS);
  });
  document.getElementById("p-"+k).classList.add("on",BT[k].cls);
  document.getElementById("formArea").style.display="block";
  document.getElementById("prompt").style.display="none";
  buildDyn(k);
  showI("✅ "+BT[k].icon+" "+BT[k].label+" selected!");showE("");
}

function buildDyn(k){
  const w=document.getElementById("dynArea");
  const cfg=BT[k];
  if(!cfg.fields||!cfg.fields.length){w.innerHTML="";return;}
  let h=`<div class="sc"><div class="sc-t">${cfg.icon} ${cfg.label} — Extra Details</div>`;
  cfg.fields.forEach(f=>{
    h+=`<div><label class="lbl">${f.lbl}</label>`;
    if(f.type==="select"){
      h+=`<select id="d_${f.id}">`;
      (f.opts||[]).forEach(o=>h+=`<option value="${o}">${o||"— Select —"}</option>`);
      h+=`</select>`;
    } else {
      h+=`<input type="${f.type}" id="d_${f.id}" placeholder="${f.ph||""}" ${f.type==="number"?"inputmode='decimal' step='0.01'":""}>`;
    }
    h+=`</div>`;
  });
  w.innerHTML=h+"</div>";
}

// Date helpers
function onDC(){
  const v=document.getElementById("bDate").value;
  if(!v)return;
  const d=new Date(v+"T00:00:00");
  const mn=["January","February","March","April","May","June","July","August","September","October","November","December"];
  document.getElementById("bMonth").value=mn[d.getMonth()];
  document.getElementById("bYear").value=String(d.getFullYear());
  const due=new Date(d);due.setDate(due.getDate()+15);
  document.getElementById("dDate").value=td(due);
  chkDue();
}
function chkDue(){
  const v=document.getElementById("dDate").value;
  document.getElementById("dueW").style.display=(v&&new Date(v+"T00:00:00")<new Date())?"block":"none";
}
function onPS(){
  const v=document.getElementById("pStat").value;
  document.getElementById("pFlds").style.display=(v==="Paid"||v==="Partial")?"block":"none";
}

// File
function handleFile(e){
  const file=e.target.files[0];
  if(!file)return;
  if(file.size>5*1024*1024){showE("❌ Image too large. Max 5MB.");return;}
  showE("");
  const reader=new FileReader();
  reader.onload=function(ev){
    const img=new Image();
    img.onload=function(){
      const canvas=document.createElement("canvas");
      let w=img.width,h=img.height;
      if(w>1200){h=Math.round(h*1200/w);w=1200;}
      canvas.width=w;canvas.height=h;
      canvas.getContext("2d").drawImage(img,0,0,w,h);
      canvas.toBlob(function(b){
        blob=b;
        document.getElementById("imgPrev").src=canvas.toDataURL("image/jpeg",0.8);
        document.getElementById("imgPrev").style.display="block";
        document.getElementById("imgPH").style.display="none";
        document.getElementById("chgImg").style.display="block";
        document.getElementById("iz").classList.add("on");
        showI("✅ Photo ready!");
      },"image/jpeg",0.8);
    };
    img.src=ev.target.result;
  };
  reader.readAsDataURL(file);
}
function clearImg(){
  blob=null;
  document.getElementById("fileIn").value="";
  document.getElementById("imgPrev").style.display="none";
  document.getElementById("imgPrev").src="";
  document.getElementById("imgPH").style.display="block";
  document.getElementById("chgImg").style.display="none";
  document.getElementById("iz").classList.remove("on");
}

// Submit
function doSubmit(){
  if(!sel)             {showE("❌ Bill type select karo.");return;}
  if(!blob)            {showE("❌ Photo attach karo.");return;}
  if(!document.getElementById("bDate").value){showE("❌ Bill date bharo.");return;}
  if(!document.getElementById("bAmt").value) {showE("❌ Amount bharo.");return;}
  if(!document.getElementById("pStat").value){showE("❌ Payment status select karo.");return;}
  showE("");

  const cfg=BT[sel];
  const spec={};
  (cfg.fields||[]).forEach(f=>{const el=document.getElementById("d_"+f.id);if(el)spec[f.id]=el.value;});

  dis(true);setSt("⏳ Uploading…");
  const reader=new FileReader();
  reader.onload=function(e){
    callScript("uploadBill",{
      storeId:store.id,storeName:store.name,
      billType:cfg.label,
      billDate:document.getElementById("bDate").value,
      billMonth:document.getElementById("bMonth").value,
      billYear:document.getElementById("bYear").value,
      amount:parseFloat(document.getElementById("bAmt").value)||0,
      invoiceNo:document.getElementById("invNo").value,
      vendorName:document.getElementById("vendr").value,
      dueDate:document.getElementById("dDate").value,
      paymentStatus:document.getElementById("pStat").value,
      amountPaid:parseFloat(document.getElementById("aPaid").value)||0,
      paymentDate:document.getElementById("pDate").value,
      paymentMode:document.getElementById("pMode").value,
      txnRef:document.getElementById("txnR").value,
      remarks:document.getElementById("rmks").value,
      specificFields:JSON.stringify(spec),
      imageBase64:e.target.result.split(",")[1],
      imageType:"image/jpeg"
    },function(res){
      dis(false);setSt("");
      if(res.success){
        recs=[];
        showPop("✅","Bill Submitted!",cfg.label+"\n₹"+parseFloat(document.getElementById("bAmt").value).toLocaleString("en-IN")+"\n"+document.getElementById("pStat").value);
        clearForm();
      } else {showE("❌ "+(res.message||"Submit fail."));}
    });
  };
  reader.readAsDataURL(blob);
}

function clearForm(){
  clearImg();sel=null;
  Object.keys(BT).forEach(k=>{const p=document.getElementById("p-"+k);p.classList.remove("on",...ALLCLS);});
  document.getElementById("formArea").style.display="none";
  document.getElementById("prompt").style.display="block";
  document.getElementById("dynArea").innerHTML="";
  document.getElementById("pFlds").style.display="none";
  document.getElementById("dueW").style.display="none";
  ["bAmt","invNo","vendr","dDate","aPaid","txnR","rmks"].forEach(id=>{const el=document.getElementById(id);if(el)el.value="";});
  document.getElementById("pStat").value="";document.getElementById("pMode").value="";
  setDts();showI("");showE("");
}

// Records
function loadRecs(force){
  if(recs.length>0&&!force){renderRecs();return;}
  document.getElementById("recWrap").innerHTML="<div class='spin'>Loading…</div>";
  callScript("getBillRecords",{storeId:store.id},function(res){
    recs=(res.records||[]).sort((a,b)=>new Date(b.billDate)-new Date(a.billDate));
    renderRecs();
  });
}
function renderRecs(){
  const w=document.getElementById("recWrap");
  if(!recs.length){w.innerHTML="<div class='empty'>Koi bill submit nahi hua abhi tak.</div>";return;}
  let h=`<table class="rt"><thead><tr><th>Date</th><th>Type</th><th>₹</th><th>Status</th><th>🖼️</th></tr></thead><tbody>`;
  recs.forEach(r=>{
    const k=Object.keys(BT).find(k=>BT[k].label===r.billType)||"x";
    const bc=BT[k]?.bdg||"bgx";
    const sc="bp"+((r.paymentStatus||"u")[0].toLowerCase());
    h+=`<tr>
      <td>${r.billDate}<br><span style="color:#bbb;font-size:0.62rem">${r.billMonth||""} ${r.billYear||""}</span></td>
      <td><span class="bg ${bc}">${r.billType}</span></td>
      <td style="font-weight:700">₹${Number(r.amount).toLocaleString("en-IN")}</td>
      <td><span class="bg ${sc}">${r.paymentStatus||"—"}</span></td>
      <td>${r.imageUrl?`<a href="${r.imageUrl}" target="_blank" class="vl">View</a>`:"—"}</td>
    </tr>`;
  });
  w.innerHTML=h+"</tbody></table>";
}

// Summary
function buildSum(){
  if(!recs.length){loadRecs(true);return;}
  const now=new Date();const m=now.getMonth(),y=now.getFullYear();
  const mn=["January","February","March","April","May","June","July","August","September","October","November","December"];
  const thisM=recs.filter(r=>r.billMonth===mn[m]&&String(r.billYear)===String(y));
  const tot=recs.reduce((s,r)=>s+(parseFloat(r.amount)||0),0);
  const unp=recs.filter(r=>r.paymentStatus==="Unpaid");
  const par=recs.filter(r=>r.paymentStatus==="Partial");

  document.getElementById("sg").innerHTML=`
    <div class="sk" style="background:#e3f2fd;color:#0d47a1"><div class="n">${recs.length}</div><div class="l">Total Bills</div></div>
    <div class="sk" style="background:#e8f5e9;color:#1b5e20"><div class="n">${thisM.length}</div><div class="l">This Month</div></div>
    <div class="sk" style="background:#fff3e0;color:#e65100"><div class="n">₹${fmtN(tot)}</div><div class="l">Total Amount</div></div>
    <div class="sk" style="background:#ffebee;color:#c62828"><div class="n">${unp.length+par.length}</div><div class="l">Unpaid/Partial</div></div>`;

  const uw=document.getElementById("unpWrap");
  const ua=[...unp,...par];
  if(!ua.length){uw.innerHTML="<div class='empty' style='padding:8px'>✅ Sab bills paid!</div>";}
  else{
    let h=`<table class="rt"><thead><tr><th>Type</th><th>Date</th><th>₹</th><th>Status</th></tr></thead><tbody>`;
    ua.forEach(r=>{
      const sc="bp"+((r.paymentStatus||"u")[0].toLowerCase());
      h+=`<tr><td>${r.billType}</td><td>${r.billDate}</td>
          <td style="font-weight:700;color:#c62828">₹${Number(r.amount).toLocaleString("en-IN")}</td>
          <td><span class="bg ${sc}">${r.paymentStatus}</span></td></tr>`;
    });
    uw.innerHTML=h+"</tbody></table>";
  }

  const tm={};
  recs.forEach(r=>{if(!tm[r.billType])tm[r.billType]={c:0,a:0};tm[r.billType].c++;tm[r.billType].a+=parseFloat(r.amount)||0;});
  let h=`<table class="rt"><thead><tr><th>Bill Type</th><th>Count</th><th>Total ₹</th></tr></thead><tbody>`;
  Object.entries(tm).sort((a,b)=>b[1].a-a[1].a).forEach(([t,v])=>{
    h+=`<tr><td>${t}</td><td>${v.c}</td><td style="font-weight:700">₹${v.a.toLocaleString("en-IN")}</td></tr>`;
  });
  document.getElementById("typWrap").innerHTML=h+"</tbody></table>";
}

function fmtN(n){if(n>=100000)return(n/100000).toFixed(1)+"L";if(n>=1000)return(n/1000).toFixed(1)+"K";return n.toFixed(0);}

// Export
function expCSV(){
  callScript("exportBills",{storeId:store.id},function(res){
    if(res.csv){
      const b=new Blob([res.csv],{type:"text/csv;charset=utf-8;"});
      const u=URL.createObjectURL(b);
      const a=document.createElement("a");
      a.href=u;a.download=store.name.replace(/\s/g,"_")+"_Bills_"+td(new Date())+".csv";a.click();
      URL.revokeObjectURL(u);
    } else {showPop("⚠️","No Data","Koi record nahi.");}
  });
}

// Logout
function logout(){
  store=null;recs=[];blob=null;sel=null;
  document.getElementById("appDiv").style.display="none";
  document.getElementById("loginDiv").style.display="block";
  document.getElementById("loginId").value="";
  document.getElementById("loginPass").value="";
  setSt("");showI("");showE("");
}
</script>
</body>
</html>
