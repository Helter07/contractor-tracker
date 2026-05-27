[index.html](https://github.com/user-attachments/files/28304882/index.html)
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="theme-color" content="#1F3864">
<title>Contractor Tracker - Modrobe Impex</title>

<script src="https://www.gstatic.com/firebasejs/9.23.0/firebase-app-compat.js"></script>
<script src="https://www.gstatic.com/firebasejs/9.23.0/firebase-auth-compat.js"></script>
<script src="https://www.gstatic.com/firebasejs/9.23.0/firebase-firestore-compat.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.18.5/xlsx.full.min.js"></script>

<style>
*{box-sizing:border-box;margin:0;padding:0;-webkit-tap-highlight-color:transparent}
body{font-family:-apple-system,BlinkMacSystemFont,"Segoe UI",Arial,sans-serif;background:#F5F5F5;color:#1A1A1A;font-size:14px}

/* PAGE SYSTEM */
#page-loading,#page-login,#page-app{position:fixed;inset:0;z-index:10}
#page-loading{background:#1F3864;display:flex;flex-direction:column;align-items:center;justify-content:center;color:#fff;z-index:30}
#page-loading .spin{font-size:48px;animation:spin 1.5s linear infinite;margin-bottom:16px}
@keyframes spin{from{transform:rotate(0deg)}to{transform:rotate(360deg)}}
#page-login{background:linear-gradient(160deg,#1F3864,#2D5090);display:none;align-items:center;justify-content:center;padding:24px;z-index:20;overflow-y:auto}
#page-app{display:none;background:#F5F5F5;overflow-y:auto;-webkit-overflow-scrolling:touch;z-index:10;padding-bottom:20px}

/* LOGIN */
.login-card{background:#fff;border-radius:20px;padding:36px 28px;width:100%;max-width:360px;text-align:center;box-shadow:0 20px 60px rgba(0,0,0,.3)}
.login-logo{font-size:52px;margin-bottom:12px}
.login-title{font-size:22px;font-weight:700;color:#1F3864;margin-bottom:6px}
.login-sub{font-size:13px;color:#888;margin-bottom:28px;line-height:1.6}
.google-btn{width:100%;padding:14px;background:#fff;border:2px solid #DDD;border-radius:12px;display:flex;align-items:center;justify-content:center;gap:12px;font-size:15px;font-weight:700;cursor:pointer;font-family:inherit;color:#1A1A1A}
.google-btn:hover{border-color:#4285F4}
.login-msg{font-size:12px;color:#888;margin-top:12px;min-height:20px}

/* HEADER */
.header{background:#1F3864;color:#fff;padding:0 20px;height:54px;display:flex;align-items:center;justify-content:space-between;position:sticky;top:0;z-index:50;box-shadow:0 2px 8px rgba(0,0,0,.3)}
.header-left h1{font-size:16px;font-weight:700}
.header-left p{font-size:10px;color:#B0C4DE;margin-top:1px}
.header-right{display:flex;align-items:center;gap:8px}
.hbtn{background:rgba(255,255,255,.15);border:none;color:#fff;border-radius:8px;padding:7px 12px;font-size:13px;cursor:pointer;font-family:inherit;white-space:nowrap}
.avatar-btn{width:32px;height:32px;border-radius:50%;background:rgba(255,255,255,.2);border:2px solid rgba(255,255,255,.4);cursor:pointer;display:flex;align-items:center;justify-content:center;font-size:13px;font-weight:700;color:#fff;overflow:hidden;flex-shrink:0}
.avatar-btn img{width:100%;height:100%;object-fit:cover;border-radius:50%}
.sync-dot{width:8px;height:8px;border-radius:50%;background:#4ADE80;display:inline-block;margin-left:4px;vertical-align:middle}
.sync-dot.busy{background:#FCD34D;animation:pulse .8s infinite}
@keyframes pulse{0%,100%{opacity:1}50%{opacity:.3}}

/* USER MENU */
.umenu{position:fixed;top:60px;right:12px;background:#fff;border-radius:12px;box-shadow:0 8px 32px rgba(0,0,0,.18);padding:8px;z-index:200;display:none;min-width:200px}
.umenu.open{display:block}
.umh{padding:10px 12px;border-bottom:1px solid #F0F0F0;margin-bottom:4px}
.umn{font-size:14px;font-weight:700}
.ume{font-size:11px;color:#888;margin-top:2px}
.umi{padding:10px 12px;border-radius:8px;cursor:pointer;font-size:14px;display:flex;align-items:center;gap:8px;color:#555}
.umi:hover{background:#F5F5F5}
.umi.danger{color:#DC2626}

/* CONTAINER */
.container{max-width:1100px;margin:0 auto;padding:16px}

/* STATS */
.stats{display:grid;grid-template-columns:repeat(4,1fr);gap:12px;margin-bottom:20px}
.stat-card{background:#fff;border-radius:10px;padding:14px 16px;border:1px solid #E0E0E0}
.stat-label{font-size:11px;color:#888;margin-bottom:6px}
.stat-val{font-size:22px;font-weight:600}
.stat-val.warn{color:#D97706}.stat-val.red{color:#DC2626}

/* TABS */
.tabs{display:flex;border-bottom:2px solid #E0E0E0;margin-bottom:16px}
.tab-btn{background:none;border:none;padding:10px 20px;font-size:13px;cursor:pointer;color:#888;border-bottom:2px solid transparent;margin-bottom:-2px;font-family:inherit}
.tab-btn.active{color:#1F3864;font-weight:600;border-bottom:2px solid #1F3864}

/* FILTER */
.filter-bar{display:flex;gap:6px;margin-bottom:14px;flex-wrap:wrap}
.filter-btn{background:#fff;border:1px solid #D0D0D0;border-radius:20px;padding:4px 14px;font-size:12px;cursor:pointer;font-family:inherit;color:#555}
.filter-btn.active{background:#1F3864;color:#fff;border-color:#1F3864;font-weight:600}

/* BUTTONS */
.btn{border:1px solid #C0C0C0;border-radius:7px;padding:7px 14px;font-size:13px;background:#fff;cursor:pointer;font-family:inherit;display:inline-flex;align-items:center;gap:5px;white-space:nowrap}
.btn:hover{background:#F0F0F0}
.btn.primary{background:#1F3864;color:#fff;border-color:#1F3864;font-weight:600}
.btn.primary:hover{background:#16305A}
.btn.success{color:#065F46;border-color:#6EE7B7}
.btn.danger{color:#DC2626;border-color:#FCA5A5}
.btn.print-btn{color:#6D28D9;border-color:#C4B5FD;background:#F5F3FF}
.btn.sm{padding:5px 11px;font-size:12px}

/* LOT CARD */
.lot-card{background:#fff;border:1px solid #E0E0E0;border-radius:12px;padding:16px 18px;margin-bottom:10px}
.lot-card-header{display:flex;align-items:flex-start;justify-content:space-between;margin-bottom:12px}
.lot-id-badge{background:#DBEAFE;color:#1E40AF;font-size:12px;font-weight:600;padding:3px 12px;border-radius:20px;white-space:nowrap}
.lot-name{font-size:15px;font-weight:600}
.lot-item{font-size:12px;color:#888;margin-top:1px}
.status-badge{font-size:11px;font-weight:600;padding:3px 12px;border-radius:20px;flex-shrink:0}
.status-PENDING{background:#FEF3C7;color:#92400E}
.status-RECEIVED{background:#DBEAFE;color:#1E40AF}
.status-SETTLED{background:#D1FAE5;color:#065F46}
.lot-nums{display:grid;grid-template-columns:repeat(5,1fr);gap:6px;margin-bottom:10px}
.num-box{background:#F7F7F7;border-radius:8px;padding:8px 10px}
.num-box .nl{font-size:10px;color:#888;margin-bottom:3px}
.num-box .nv{font-size:13px;font-weight:600}
.num-box .nv.warn{color:#D97706}.num-box .nv.red{color:#DC2626}
.lot-meta{display:flex;gap:16px;font-size:12px;color:#888;margin-bottom:10px;flex-wrap:wrap;align-items:center}
.lot-meta .ok{color:#059669;font-weight:600}
.lot-meta .miss{color:#DC2626;font-weight:600}
.lot-meta .note{font-style:italic;color:#AAA}
.lot-actions{display:flex;gap:6px;flex-wrap:wrap;align-items:center}

/* PAYMENTS TABLE */
.table-wrap{border:1px solid #E0E0E0;border-radius:10px;overflow:hidden}
table{width:100%;border-collapse:collapse;font-size:13px;table-layout:fixed}
thead tr{background:#F5F5F5}
th{padding:10px 11px;text-align:left;font-weight:600;color:#666;font-size:11px;border-bottom:1px solid #E0E0E0}
td{padding:9px 11px;border-bottom:1px solid #F0F0F0;overflow:hidden;text-overflow:ellipsis;white-space:nowrap}
tr:last-child td{border-bottom:none}
tr.alt{background:#FAFAFA}
.mode-pill{background:#F0F0F0;border:1px solid #DDD;padding:2px 8px;border-radius:12px;font-size:11px}
.total-row td{font-weight:600;background:#F0F4FF}
.pay-header{display:flex;justify-content:space-between;align-items:center;margin-bottom:12px;flex-wrap:wrap;gap:8px}
.lot-filter-tag{background:#DBEAFE;color:#1E40AF;font-size:12px;padding:3px 10px;border-radius:20px;display:inline-flex;align-items:center;gap:6px}
.lot-filter-tag .remove{cursor:pointer;font-weight:700;font-size:14px}

/* EMPTY */
.empty{text-align:center;padding:3rem;color:#AAA;border:1px solid #E0E0E0;border-radius:12px;background:#fff}
.empty-icon{font-size:40px;margin-bottom:8px}

/* OVERLAYS & MODALS */
.overlay{display:none;position:fixed;inset:0;background:rgba(0,0,0,.45);z-index:200;align-items:flex-end;justify-content:center}
.overlay.show{display:flex}
.modal{background:#fff;border-radius:20px 20px 0 0;width:100%;max-width:520px;max-height:92vh;overflow-y:auto;animation:sup .25s ease;padding-bottom:20px}
@keyframes sup{from{transform:translateY(100%)}to{transform:translateY(0)}}
.mhandle{width:40px;height:4px;background:#DDD;border-radius:2px;margin:12px auto 6px}
.modal-header{display:flex;justify-content:space-between;align-items:flex-start;padding:8px 20px 14px;border-bottom:1px solid #F0F0F0}
.modal-subtitle{font-size:12px;color:#888;margin-top:2px}
.modal-body{padding:14px 20px}
.modal-foot{padding:10px 20px 0;display:flex;gap:8px}
.close-btn{background:#F0F0F0;border:none;width:30px;height:30px;border-radius:50%;cursor:pointer;font-size:16px;display:flex;align-items:center;justify-content:center;color:#666;flex-shrink:0}
.form-group{margin-bottom:13px}
.form-row{display:grid;grid-template-columns:1fr 1fr;gap:12px;margin-bottom:13px}
label{font-size:12px;color:#666;display:block;margin-bottom:4px;font-weight:500}
input,select{width:100%;padding:11px 12px;font-size:14px;border:1.5px solid #DDD;border-radius:9px;font-family:inherit;background:#fff;color:#1A1A1A;outline:none;-webkit-appearance:none}
input:focus,select:focus{border-color:#1F3864;box-shadow:0 0 0 3px #DBEAFE}
.info-box{padding:10px 14px;border-radius:8px;font-size:13px;margin-top:2px;font-weight:600}
.info-box.blue{background:#EFF6FF;color:#1D4ED8}
.info-box.yellow{background:#FFFBEB;color:#92400E}
.info-box.green{background:#ECFDF5;color:#065F46}
.info-box.red{background:#FEF2F2;color:#B91C1C}
.confirm-body{font-size:13px;color:#555;line-height:1.6}

@media(min-width:600px){
  .overlay{align-items:center;padding:20px}
  .modal{border-radius:16px;max-height:88vh}
  .mhandle{display:none}
}
@media(max-width:700px){
  .stats{grid-template-columns:1fr 1fr}
  .lot-nums{grid-template-columns:repeat(3,1fr)}
  .form-row{grid-template-columns:1fr}
  .container{padding:12px}
  .header{padding:0 12px}
  .header-left h1{font-size:14px}
}
</style>
</head>
<body>

<!-- LOADING -->
<div id="page-loading">
  <div class="spin">🧵</div>
  <div style="font-size:20px;font-weight:700">Contractor Tracker</div>
  <p style="font-size:13px;color:#B0C4DE;margin-top:6px">Loading...</p>
</div>

<!-- LOGIN -->
<div id="page-login">
  <div class="login-card">
    <div class="login-logo">🧵</div>
    <div class="login-title">Contractor Tracker</div>
    <div class="login-sub">Modrobe Impex<br>Job Work · Lot Management</div>
    <button class="google-btn" onclick="doLogin()">
      <svg width="20" height="20" viewBox="0 0 24 24"><path d="M22.56 12.25c0-.78-.07-1.53-.2-2.25H12v4.26h5.92c-.26 1.37-1.04 2.53-2.21 3.31v2.77h3.57c2.08-1.92 3.28-4.74 3.28-8.09z" fill="#4285F4"/><path d="M12 23c2.97 0 5.46-.98 7.28-2.66l-3.57-2.77c-.98.66-2.23 1.06-3.71 1.06-2.86 0-5.29-1.93-6.16-4.53H2.18v2.84C3.99 20.53 7.7 23 12 23z" fill="#34A853"/><path d="M5.84 14.09c-.22-.66-.35-1.36-.35-2.09s.13-1.43.35-2.09V7.07H2.18C1.43 8.55 1 10.22 1 12s.43 3.45 1.18 4.93l3.66-2.84z" fill="#FBBC05"/><path d="M12 5.38c1.62 0 3.06.56 4.21 1.64l3.15-3.15C17.45 2.09 14.97 1 12 1 7.7 1 3.99 3.47 2.18 7.07l3.66 2.84c.87-2.6 3.3-4.53 6.16-4.53z" fill="#EA4335"/></svg>
      Continue with Google
    </button>
    <div class="login-msg" id="login-msg"></div>
  </div>
</div>

<!-- APP -->
<div id="page-app">

  <div class="header">
    <div class="header-left">
      <h1>🧵 Contractor Tracker</h1>
      <p>Modrobe Impex &mdash; Job Work <span class="sync-dot" id="sync-dot"></span></p>
    </div>
    <div class="header-right">
      <button class="hbtn" onclick="openModal('addLot')">+ New Lot</button>
      <button class="hbtn" onclick="openImportModal()" style="background:rgba(255,255,255,.1)">📥 Import</button>
      <div class="avatar-btn" id="avbtn" onclick="toggleMenu()"></div>
    </div>
  </div>

  <div class="container">
    <div class="stats">
      <div class="stat-card"><div class="stat-label">📦 Total Lots</div><div class="stat-val" id="st-total">0</div></div>
      <div class="stat-card"><div class="stat-label">⏳ Pending</div><div class="stat-val warn" id="st-pending">0</div></div>
      <div class="stat-card"><div class="stat-label">💵 Advance Paid</div><div class="stat-val" id="st-adv">Rs.0</div></div>
      <div class="stat-card"><div class="stat-label">💰 Balance Due</div><div class="stat-val red" id="st-bal">Rs.0</div></div>
    </div>
    <div class="tabs">
      <button class="tab-btn active" id="tab-lots-btn" onclick="switchTab('lots')">Lots (<span id="tab-lots-count">0</span>)</button>
      <button class="tab-btn" id="tab-pays-btn" onclick="switchTab('pays')">Payments (<span id="tab-pays-count">0</span>)</button>
    </div>
    <div id="panel-lots">
      <div class="filter-bar">
        <button class="filter-btn active" onclick="setFilter('ALL',this)">All</button>
        <button class="filter-btn" onclick="setFilter('PENDING',this)">Pending</button>
        <button class="filter-btn" onclick="setFilter('RECEIVED',this)">Received</button>
        <button class="filter-btn" onclick="setFilter('SETTLED',this)">Settled</button>
      </div>
      <div id="lots-list"></div>
    </div>
    <div id="panel-pays" style="display:none">
      <div class="pay-header">
        <div style="display:flex;align-items:center;gap:8px">
          <span id="pay-lot-tag" style="display:none" class="lot-filter-tag"></span>
          <span id="pay-meta" style="font-size:12px;color:#888"></span>
        </div>
        <button class="btn sm" onclick="openModal('addPayment')">+ Add Payment</button>
      </div>
      <div id="pays-list"></div>
    </div>
  </div>
</div>

<!-- USER MENU -->
<div class="umenu" id="umenu">
  <div class="umh"><div class="umn" id="um-name"></div><div class="ume" id="um-email"></div></div>
  <div class="umi" onclick="openImportModal()">📥 Import from Excel</div>
  <div class="umi danger" onclick="doLogout()">🚪 Sign Out</div>
</div>

<!-- ADD LOT MODAL -->
<div class="overlay" id="modal-addLot">
<div class="modal"><div class="mhandle"></div>
  <div class="modal-header"><h2>New Cutting Lot</h2><button class="close-btn" onclick="closeModal('addLot')">✕</button></div>
  <div class="modal-body">
    <div class="form-row">
      <div><label>Contractor Name *</label><input id="f-contractor" placeholder="e.g. Ramesh Tailor" oninput="previewLot()"></div>
      <div><label>Item Description *</label><input id="f-item" placeholder="e.g. Lower, Kurta" oninput="previewLot()"></div>
    </div>
    <div class="form-row">
      <div><label>Total Pieces *</label><input id="f-pieces" type="number" placeholder="500" oninput="previewLot()"></div>
      <div><label>Rate per Piece (Rs.) *</label><input id="f-rate" type="number" placeholder="23" oninput="previewLot()"></div>
    </div>
    <div class="form-group"><label>Date Issued *</label><input id="f-date" type="date"></div>
    <div class="form-group"><label>Remarks (optional)</label><input id="f-remarks" placeholder="Any notes"></div>
    <div class="info-box blue" id="lot-preview" style="display:none;margin-bottom:8px"></div>
  </div>
  <div class="modal-foot">
    <button class="btn" style="flex:1;justify-content:center" onclick="closeModal('addLot')">Cancel</button>
    <button class="btn primary" style="flex:1;justify-content:center" onclick="addLot()">Create Lot</button>
  </div>
</div></div>

<!-- ADD PAYMENT MODAL -->
<div class="overlay" id="modal-addPayment">
<div class="modal"><div class="mhandle"></div>
  <div class="modal-header"><h2>Record Payment</h2><button class="close-btn" onclick="closeModal('addPayment')">✕</button></div>
  <div class="modal-body">
    <div class="form-group"><label>Lot No. *</label><select id="p-lotid" onchange="previewPay()"><option value="">Select lot...</option></select></div>
    <div class="form-row">
      <div><label>Payment Date *</label><input id="p-date" type="date"></div>
      <div><label>Amount (Rs.) *</label><input id="p-amount" type="number" placeholder="2000" oninput="previewPay()" inputmode="numeric"></div>
    </div>
    <div class="form-group"><label>Mode</label><select id="p-mode"><option>Cash</option><option>UPI</option><option>Bank Transfer</option><option>Cheque</option><option>Other</option></select></div>
    <div class="form-group"><label>Remarks</label><input id="p-remarks" placeholder="e.g. Running advance"></div>
    <div class="info-box yellow" id="pay-preview" style="display:none;margin-bottom:8px"></div>
  </div>
  <div class="modal-foot">
    <button class="btn" style="flex:1;justify-content:center" onclick="closeModal('addPayment')">Cancel</button>
    <button class="btn primary" style="flex:1;justify-content:center" onclick="addPayment()">Save Payment</button>
  </div>
</div></div>

<!-- RECEIVE LOT MODAL -->
<div class="overlay" id="modal-receive">
<div class="modal"><div class="mhandle"></div>
  <div class="modal-header">
    <div><h2>Receive Lot Back</h2><div class="modal-subtitle" id="recv-subtitle"></div></div>
    <button class="close-btn" onclick="closeModal('receive')">✕</button>
  </div>
  <div class="modal-body">
    <div class="info-box blue" id="recv-issued" style="margin-bottom:14px"></div>
    <div class="form-row">
      <div><label>Pieces Received *</label><input id="r-pieces" type="number" oninput="previewRecv()" inputmode="numeric"></div>
      <div><label>Date Received *</label><input id="r-date" type="date"></div>
    </div>
    <div class="info-box green" id="recv-preview" style="display:none;margin-bottom:8px"></div>
  </div>
  <div class="modal-foot">
    <button class="btn" style="flex:1;justify-content:center" onclick="closeModal('receive')">Cancel</button>
    <button class="btn primary" style="flex:1;justify-content:center" onclick="receiveLot()">Confirm Receipt</button>
  </div>
</div></div>

<!-- CONFIRM DELETE MODAL -->
<div class="overlay" id="modal-confirm">
<div class="modal" style="max-width:420px"><div class="mhandle"></div>
  <div class="modal-header"><h2>Confirm Delete</h2><button class="close-btn" onclick="closeModal('confirm')">✕</button></div>
  <div class="modal-body"><div id="confirm-msg" class="confirm-body"></div></div>
  <div class="modal-foot" style="margin-top:16px">
    <button class="btn" style="flex:1;justify-content:center" onclick="closeModal('confirm')">Cancel</button>
    <button class="btn danger" style="flex:1;justify-content:center;background:#FEF2F2;font-weight:700" id="confirm-ok-btn">Yes, Delete</button>
  </div>
</div></div>

<!-- IMPORT EXCEL MODAL -->
<div class="overlay" id="modal-import">
<div class="modal" style="max-height:95vh"><div class="mhandle"></div>
  <div class="modal-header"><h2>📥 Import from Excel</h2><button class="close-btn" onclick="closeModal('import')">✕</button></div>
  <div class="modal-body" id="imp-body" style="overflow-y:auto">

    <div id="imp-step1">
      <div style="background:#F0F4FF;border:2px dashed #93C5FD;border-radius:12px;padding:24px;text-align:center;cursor:pointer;margin-bottom:14px" onclick="document.getElementById('imp-file').click()">
        <div style="font-size:36px;margin-bottom:8px">📊</div>
        <div style="font-size:14px;font-weight:700;color:#1F3864;margin-bottom:4px">Tap to select Excel file</div>
        <div style="font-size:12px;color:#888">.xlsx format</div>
      </div>
      <input type="file" id="imp-file" accept=".xlsx,.xls" style="display:none" onchange="readImportFile(this)">
      <div style="background:#FFFBEB;border:1px solid #FDE68A;border-radius:10px;padding:12px 14px;font-size:12px;color:#92400E">
        <strong>Expected Excel format:</strong><br>
        Columns: Lot ID · Contractor · Item · Pieces Issued · Rate · Date Issued · Pieces Received · Date Received · Status · Advance Paid<br><br>
        OR any sheet with columns containing lot/contractor/pieces/rate info. New lots not already in the app will be added.
      </div>
    </div>

    <div id="imp-step2" style="display:none;text-align:center;padding:24px">
      <div style="font-size:36px;margin-bottom:10px">⏳</div>
      <div style="font-size:14px;font-weight:600;color:#1F3864" id="imp-prog-text">Reading file...</div>
      <div style="background:#E5E7EB;border-radius:4px;height:8px;overflow:hidden;margin:12px 0">
        <div id="imp-prog-fill" style="background:#1F3864;height:8px;border-radius:4px;width:0%;transition:width .3s"></div>
      </div>
    </div>

    <div id="imp-step3" style="display:none">
      <div id="imp-summary" style="margin-bottom:12px"></div>
      <div style="font-size:12px;font-weight:700;color:#555;margin-bottom:8px">Preview — New lots to be added:</div>
      <div id="imp-preview" style="max-height:320px;overflow-y:auto"></div>
    </div>

    <div id="imp-step4" style="display:none;text-align:center;padding:20px">
      <div style="font-size:48px;margin-bottom:10px">🎉</div>
      <div style="font-size:16px;font-weight:700;color:#1F3864;margin-bottom:6px" id="imp-done-title"></div>
      <div style="font-size:13px;color:#888" id="imp-done-sub"></div>
    </div>

  </div>
  <div class="modal-foot" id="imp-foot">
    <button class="btn" style="flex:1;justify-content:center" onclick="closeModal('import')">Cancel</button>
  </div>
</div></div>

<script>
/* ══ FIREBASE ══ */
firebase.initializeApp({
  apiKey: "AIzaSyDRfRBzqdg8LCUjZKdGRNIsKCBszbtM7ok",
  authDomain: "challan-manager-85768.firebaseapp.com",
  projectId: "challan-manager-85768",
  storageBucket: "challan-manager-85768.firebasestorage.app",
  messagingSenderId: "667915790046",
  appId: "1:667915790046:web:92b61ba86c9facd620444f"
});
const auth = firebase.auth();
const db   = firebase.firestore();

/* ══ STATE ══ */
let state = { lots:[], pays:[], lotCtr:0, payCtr:0 };
let uid=null, unsubDB=null, appIsReady=false;

const pad    = n => String(n).padStart(3,'0');
const fmt    = n => 'Rs.'+new Intl.NumberFormat('en-IN').format(Math.round(n||0));
const fmtN   = n => new Intl.NumberFormat('en-IN').format(Math.round(n||0));
const fmtDate= d => d?new Date(d+'T00:00:00').toLocaleDateString('en-IN',{day:'2-digit',month:'short',year:'numeric'}):'-';
const today  = () => new Date().toISOString().split('T')[0];
const esc    = s => String(s||'').replace(/&/g,'&amp;').replace(/</g,'&lt;').replace(/>/g,'&gt;').replace(/"/g,'&quot;');

function advPaid(lid){ return state.pays.filter(p=>p.lotId===lid).reduce((s,p)=>s+p.amount,0); }
function balDue(lot){ return ((lot.piecesReceived!=null?lot.piecesReceived:lot.piecesIssued)*lot.rate)-advPaid(lot.id); }

/* ══ SYNC ══ */
function setSyncDot(s){
  const d=document.getElementById('sync-dot');
  d.className='sync-dot'+(s==='busy'?' busy':'');
  d.title=s==='busy'?'Saving...':'Synced ✓';
}
function subscribeData(){
  const ref=db.collection('users').doc(uid).collection('contractor').doc('main');
  setSyncDot('busy');
  unsubDB=ref.onSnapshot(snap=>{
    if(snap.exists){const d=snap.data();state.lots=d.lots||[];state.pays=d.pays||[];state.lotCtr=d.lotCtr||0;state.payCtr=d.payCtr||0;}
    setSyncDot('ok');render();
  },err=>{setSyncDot('err');console.error(err);});
}
async function saveToCloud(){
  if(!uid)return;
  setSyncDot('busy');
  try{
    await db.collection('users').doc(uid).collection('contractor').doc('main').set({
      lots:state.lots,pays:state.pays,lotCtr:state.lotCtr,payCtr:state.payCtr,updated:new Date().toISOString()
    });
    setSyncDot('ok');
  }catch(e){setSyncDot('err');alert('Sync error: '+e.message);}
}

/* ══ AUTH ══ */
function showApp(user){
  if(!user)return;
  uid=user.uid; appIsReady=true;
  document.getElementById('page-loading').style.display='none';
  document.getElementById('page-login').style.display='none';
  document.getElementById('page-app').style.display='block';
  document.getElementById('um-name').textContent=user.displayName||'';
  document.getElementById('um-email').textContent=user.email||'';
  const av=document.getElementById('avbtn');
  if(user.photoURL) av.innerHTML='<img src="'+user.photoURL+'" referrerpolicy="no-referrer">';
  else av.textContent=(user.displayName||'U')[0].toUpperCase();
  subscribeData();
}
function showLogin(){
  appIsReady=false; uid=null;
  if(unsubDB){unsubDB();unsubDB=null;}
  document.getElementById('page-loading').style.display='none';
  document.getElementById('page-app').style.display='none';
  document.getElementById('page-login').style.display='flex';
  document.getElementById('login-msg').textContent='';
  const btn=document.querySelector('.google-btn');
  if(btn){btn.disabled=false;btn.innerHTML='<svg width="20" height="20" viewBox="0 0 24 24"><path d="M22.56 12.25c0-.78-.07-1.53-.2-2.25H12v4.26h5.92c-.26 1.37-1.04 2.53-2.21 3.31v2.77h3.57c2.08-1.92 3.28-4.74 3.28-8.09z" fill="#4285F4"/><path d="M12 23c2.97 0 5.46-.98 7.28-2.66l-3.57-2.77c-.98.66-2.23 1.06-3.71 1.06-2.86 0-5.29-1.93-6.16-4.53H2.18v2.84C3.99 20.53 7.7 23 12 23z" fill="#34A853"/><path d="M5.84 14.09c-.22-.66-.35-1.36-.35-2.09s.13-1.43.35-2.09V7.07H2.18C1.43 8.55 1 10.22 1 12s.43 3.45 1.18 4.93l3.66-2.84z" fill="#FBBC05"/><path d="M12 5.38c1.62 0 3.06.56 4.21 1.64l3.15-3.15C17.45 2.09 14.97 1 12 1 7.7 1 3.99 3.47 2.18 7.07l3.66 2.84c.87-2.6 3.3-4.53 6.16-4.53z" fill="#EA4335"/></svg> Continue with Google';}
}
auth.onAuthStateChanged(user=>{ if(user){showApp(user);}else if(!appIsReady){showLogin();} });
auth.getRedirectResult().then(r=>{if(r&&r.user)showApp(r.user);}).catch(()=>{});

function doLogin(){
  const btn=document.querySelector('.google-btn'),msg=document.getElementById('login-msg');
  if(btn){btn.disabled=true;btn.innerHTML='⏳ Opening Google...';}
  if(msg)msg.textContent='';
  const provider=new firebase.auth.GoogleAuthProvider();
  provider.setCustomParameters({prompt:'select_account'});
  auth.signInWithPopup(provider)
    .then(r=>{if(r&&r.user)showApp(r.user);})
    .catch(e=>{
      if(e.code==='auth/popup-blocked'||e.code==='auth/popup-closed-by-user'){
        msg.textContent='Redirecting...';
        auth.signInWithRedirect(provider);
      } else {
        if(btn){btn.disabled=false;btn.innerHTML='Continue with Google';}
        if(msg){msg.textContent='Error: '+e.code;msg.style.color='#DC2626';}
      }
    });
}
function doLogout(){closeMenu();appIsReady=false;auth.signOut().then(()=>showLogin());}

/* ══ USER MENU ══ */
function toggleMenu(){document.getElementById('umenu').classList.toggle('open');}
function closeMenu(){document.getElementById('umenu').classList.remove('open');}
document.addEventListener('click',e=>{const m=document.getElementById('umenu'),b=document.getElementById('avbtn');if(m.classList.contains('open')&&!m.contains(e.target)&&!b.contains(e.target))closeMenu();});

/* ══ RENDER ══ */
let curFilter='ALL',curTab='lots',payLotFilter=null,currentReceiveLotId=null;
function render(){renderStats();renderLots();renderPays();renderTabCounts();}

function renderStats(){
  const adv=state.pays.reduce((s,p)=>s+p.amount,0);
  const bal=state.lots.reduce((s,l)=>s+Math.max(0,balDue(l)),0);
  document.getElementById('st-total').textContent=state.lots.length;
  document.getElementById('st-pending').textContent=state.lots.filter(l=>l.status==='PENDING').length;
  document.getElementById('st-adv').textContent=fmt(adv);
  document.getElementById('st-bal').textContent=fmt(bal);
}
function renderTabCounts(){
  document.getElementById('tab-lots-count').textContent=state.lots.length;
  document.getElementById('tab-pays-count').textContent=state.pays.length;
}
function renderLots(){
  const list=document.getElementById('lots-list');
  const vis=curFilter==='ALL'?state.lots:state.lots.filter(l=>l.status===curFilter);
  if(!vis.length){list.innerHTML='<div class="empty"><div class="empty-icon">📦</div>'+(curFilter==='ALL'?'No lots yet. Tap <b>+ New Lot</b> to start.':'No '+curFilter.toLowerCase()+' lots.')+'</div>';return;}
  list.innerHTML=[...vis].reverse().map(lot=>{
    const adv=advPaid(lot.id);
    const payable=(lot.piecesReceived!=null?lot.piecesReceived:lot.piecesIssued)*lot.rate;
    const bal=payable-adv;
    const miss=lot.piecesReceived!=null?lot.piecesIssued-lot.piecesReceived:null;
    const lotPayCount=state.pays.filter(p=>p.lotId===lot.id).length;
    const metaPcs=miss!=null?'<span class="'+(miss>0?'miss':'ok')+'">'+(miss>0?'⚠ ':'✓ ')+'Received: '+fmtN(lot.piecesReceived)+' pcs'+(miss>0?' ('+miss+' missing)':'')+'</span>':'';
    const metaNote=lot.remarks?'<span class="note">"'+esc(lot.remarks)+'"</span>':'';
    const actPay=(lot.status==='PENDING'||lot.status==='RECEIVED')?'<button class="btn sm" onclick="openModal(\'addPayment\',\''+lot.id+'\')">💵 + Payment</button>':'';
    const actRecv=lot.status==='PENDING'?'<button class="btn sm" onclick="openReceive(\''+lot.id+'\')">📥 Receive Lot</button>':'';
    const actSettle=lot.status==='RECEIVED'?'<button class="btn sm success" onclick="settleLot(\''+lot.id+'\')">✅ Mark Settled</button>':'';
    return '<div class="lot-card">'+
      '<div class="lot-card-header"><div style="display:flex;align-items:center;gap:10px"><span class="lot-id-badge">'+esc(lot.id)+'</span><div><div class="lot-name">'+esc(lot.contractorName)+'</div><div class="lot-item">'+esc(lot.item)+'</div></div></div><span class="status-badge status-'+lot.status+'">'+lot.status+'</span></div>'+
      '<div class="lot-nums">'+
        '<div class="num-box"><div class="nl">Issued</div><div class="nv">'+fmtN(lot.piecesIssued)+' pcs</div></div>'+
        '<div class="num-box"><div class="nl">Rate</div><div class="nv">Rs.'+lot.rate+'/pc</div></div>'+
        '<div class="num-box"><div class="nl">Total Agreed</div><div class="nv">'+fmt(lot.piecesIssued*lot.rate)+'</div></div>'+
        '<div class="num-box"><div class="nl">Advance Paid</div><div class="nv '+(adv>0?'warn':'')+'">'+fmt(adv)+'</div></div>'+
        '<div class="num-box"><div class="nl">'+(lot.status==='PENDING'?'Est. Balance':'Balance Due')+'</div><div class="nv '+(bal>0?'red':'')+'">'+fmt(bal)+'</div></div>'+
      '</div>'+
      '<div class="lot-meta"><span>📤 Out: '+fmtDate(lot.dateIssued)+'</span><span>📥 In: '+fmtDate(lot.dateReceived)+'</span>'+metaPcs+metaNote+'</div>'+
      '<div class="lot-actions">'+actPay+actRecv+actSettle+
        '<button class="btn sm print-btn" onclick="printLot(\''+lot.id+'\')">🖨 Print</button>'+
        '<button class="btn sm" style="margin-left:auto" onclick="viewLotPayments(\''+lot.id+'\')">🕒 Payments ('+lotPayCount+')</button>'+
        '<button class="btn sm danger" onclick="confirmDelete(\''+lot.id+'\')">🗑 Delete</button>'+
      '</div></div>';
  }).join('');
}
function renderPays(){
  const visPays=payLotFilter?state.pays.filter(p=>p.lotId===payLotFilter):state.pays;
  const total=visPays.reduce((s,p)=>s+p.amount,0);
  const tag=document.getElementById('pay-lot-tag');
  if(payLotFilter){tag.style.display='inline-flex';tag.innerHTML='Lot: '+esc(payLotFilter)+' <span class="remove" onclick="clearPayFilter()">×</span>';}
  else tag.style.display='none';
  document.getElementById('pay-meta').textContent=visPays.length+' record(s) · Total: '+fmt(total);
  const el=document.getElementById('pays-list');
  if(!visPays.length){el.innerHTML='<div class="empty"><div class="empty-icon">💵</div>No payments recorded yet.</div>';return;}
  const rows=[...visPays].reverse().map((p,i)=>{
    const lot=state.lots.find(l=>l.id===p.lotId);
    return '<tr class="'+(i%2?'alt':'')+'"><td style="color:#888">'+esc(p.id)+'</td><td>'+fmtDate(p.date)+'</td><td style="color:#1E40AF;font-weight:600">'+esc(p.lotId)+'</td><td>'+esc(lot?lot.contractorName:'-')+'</td><td style="font-weight:600">'+fmt(p.amount)+'</td><td><span class="mode-pill">'+esc(p.mode)+'</span></td><td style="color:#888">'+esc(p.remarks||'-')+'</td></tr>';
  }).join('');
  el.innerHTML='<div class="table-wrap"><table><thead><tr><th style="width:9%">Pay ID</th><th style="width:13%">Date</th><th style="width:9%">Lot</th><th style="width:18%">Contractor</th><th style="width:12%">Amount</th><th style="width:11%">Mode</th><th style="width:28%">Remarks</th></tr></thead><tbody>'+rows+'<tr class="total-row"><td colspan="4" style="text-align:right;padding-right:14px">Total</td><td>'+fmt(total)+'</td><td colspan="2"></td></tr></tbody></table></div>';
}

/* ══ TABS & FILTERS ══ */
function switchTab(t){
  curTab=t;
  document.getElementById('panel-lots').style.display=t==='lots'?'block':'none';
  document.getElementById('panel-pays').style.display=t==='pays'?'block':'none';
  document.getElementById('tab-lots-btn').className='tab-btn'+(t==='lots'?' active':'');
  document.getElementById('tab-pays-btn').className='tab-btn'+(t==='pays'?' active':'');
}
function setFilter(f,btn){curFilter=f;document.querySelectorAll('.filter-btn').forEach(b=>b.classList.remove('active'));btn.classList.add('active');renderLots();}
function viewLotPayments(lid){payLotFilter=lid;switchTab('pays');renderPays();}
function clearPayFilter(){payLotFilter=null;renderPays();}

/* ══ MODALS ══ */
function openModal(type,lotId){
  if(type==='addLot'){
    ['f-contractor','f-item','f-pieces','f-rate','f-remarks'].forEach(id=>document.getElementById(id).value='');
    document.getElementById('f-date').value=today();
    document.getElementById('lot-preview').style.display='none';
  }
  if(type==='addPayment'){
    const sel=document.getElementById('p-lotid');
    sel.innerHTML='<option value="">Select lot...</option>';
    state.lots.filter(l=>l.status!=='SETTLED').forEach(l=>{const o=document.createElement('option');o.value=l.id;o.textContent=l.id+' – '+l.contractorName+' ('+l.item+')';sel.appendChild(o);});
    if(lotId)sel.value=lotId;
    document.getElementById('p-date').value=today();
    document.getElementById('p-amount').value='';
    document.getElementById('p-mode').value='Cash';
    document.getElementById('p-remarks').value='';
    document.getElementById('pay-preview').style.display='none';
    previewPay();
  }
  document.getElementById('modal-'+type).classList.add('show');
}
function openReceive(lotId){
  currentReceiveLotId=lotId;
  const lot=state.lots.find(l=>l.id===lotId);
  document.getElementById('recv-subtitle').textContent=lot.id+' – '+lot.contractorName;
  document.getElementById('recv-issued').textContent='Issued: '+fmtN(lot.piecesIssued)+' pcs @ Rs.'+lot.rate+' = '+fmt(lot.piecesIssued*lot.rate);
  document.getElementById('r-pieces').value='';
  document.getElementById('r-pieces').placeholder=lot.piecesIssued;
  document.getElementById('r-date').value=today();
  document.getElementById('recv-preview').style.display='none';
  document.getElementById('modal-receive').classList.add('show');
}
function closeModal(type){document.getElementById('modal-'+type).classList.remove('show');}
document.querySelectorAll('.overlay').forEach(o=>o.addEventListener('click',e=>{if(e.target===o)o.classList.remove('show');}));

/* ══ PREVIEWS ══ */
function previewLot(){const pcs=+document.getElementById('f-pieces').value,rate=+document.getElementById('f-rate').value,box=document.getElementById('lot-preview');if(pcs&&rate){box.style.display='block';box.textContent='Total agreed: '+fmt(pcs*rate);}else box.style.display='none';}
function previewPay(){const lid=document.getElementById('p-lotid').value,amt=+document.getElementById('p-amount').value,box=document.getElementById('pay-preview');if(lid&&amt){box.style.display='block';box.textContent='Total advance after this: '+fmt(advPaid(lid)+amt);}else box.style.display='none';}
function previewRecv(){
  const lot=state.lots.find(l=>l.id===currentReceiveLotId),pcs=+document.getElementById('r-pieces').value,box=document.getElementById('recv-preview');
  if(!pcs||!lot){box.style.display='none';return;}
  const miss=lot.piecesIssued-pcs;box.style.display='block';
  if(miss>0){box.className='info-box red';box.textContent='⚠ '+miss+' pcs missing — Payable: '+fmt(pcs*lot.rate);}
  else{box.className='info-box green';box.textContent='✓ All pieces received — Payable: '+fmt(pcs*lot.rate);}
}

/* ══ ACTIONS ══ */
function addLot(){
  const c=document.getElementById('f-contractor').value.trim(),it=document.getElementById('f-item').value.trim(),
        pcs=+document.getElementById('f-pieces').value,rate=+document.getElementById('f-rate').value,
        dt=document.getElementById('f-date').value,rem=document.getElementById('f-remarks').value.trim();
  if(!c||!it||!pcs||!rate||!dt){alert('Please fill all required fields.');return;}
  state.lotCtr++;
  state.lots.push({id:'LOT-'+pad(state.lotCtr),contractorName:c,item:it,piecesIssued:pcs,rate,dateIssued:dt,piecesReceived:null,dateReceived:null,status:'PENDING',remarks:rem});
  saveToCloud();render();closeModal('addLot');
}
function addPayment(){
  const lid=document.getElementById('p-lotid').value,dt=document.getElementById('p-date').value,
        amt=+document.getElementById('p-amount').value,mode=document.getElementById('p-mode').value,
        rem=document.getElementById('p-remarks').value.trim();
  if(!lid||!dt||!amt){alert('Please fill all required fields.');return;}
  state.payCtr++;
  state.pays.push({id:'PAY-'+pad(state.payCtr),lotId:lid,date:dt,amount:amt,mode,remarks:rem});
  saveToCloud();render();closeModal('addPayment');
}
function receiveLot(){
  const pcs=+document.getElementById('r-pieces').value,dt=document.getElementById('r-date').value;
  if(!pcs||!dt){alert('Please fill all required fields.');return;}
  const lot=state.lots.find(l=>l.id===currentReceiveLotId);if(!lot)return;
  lot.piecesReceived=pcs;lot.dateReceived=dt;lot.status='RECEIVED';
  saveToCloud();render();closeModal('receive');
}
function settleLot(id){const lot=state.lots.find(l=>l.id===id);if(!lot)return;lot.status='SETTLED';saveToCloud();render();}

/* ══ DELETE WITH CONFIRMATION ══ */
function confirmDelete(lotId){
  const lot=state.lots.find(l=>l.id===lotId);if(!lot)return;
  const adv=advPaid(lotId),bal=balDue(lot),payCount=state.pays.filter(p=>p.lotId===lotId).length;
  const balMsg=bal>0
    ?'<div style="background:#FEF2F2;border:1.5px solid #FCA5A5;border-radius:10px;padding:10px 14px;margin:10px 0;font-size:13px;color:#B91C1C">⚠️ <strong>Balance Due: '+fmt(bal)+'</strong> — Outstanding payable to contractor.</div>'
    :'<div style="background:#ECFDF5;border:1.5px solid #A7F3D0;border-radius:10px;padding:10px 14px;margin:10px 0;font-size:13px;color:#065F46">✅ No outstanding balance on this lot.</div>';
  document.getElementById('confirm-msg').innerHTML='<div style="margin-bottom:10px"><span style="font-size:13px;font-weight:700;color:#1E40AF;background:#DBEAFE;padding:3px 10px;border-radius:20px">'+esc(lot.id)+'</span></div><div style="font-size:13px;color:#555;margin-bottom:4px">Contractor: <strong>'+esc(lot.contractorName)+'</strong><br>Item: <strong>'+esc(lot.item)+'</strong><br>Pieces: <strong>'+fmtN(lot.piecesIssued)+' pcs</strong></div>'+balMsg+'<div style="background:#FFF7ED;border:1px solid #FDE68A;border-radius:8px;padding:10px 14px;font-size:12px;color:#92400E;margin-top:4px">🗑 Also deletes <strong>'+payCount+' payment(s)</strong>. Cannot be undone.</div>';
  document.getElementById('confirm-ok-btn').onclick=function(){doDelete(lotId);closeModal('confirm');};
  document.getElementById('modal-confirm').classList.add('show');
}
function doDelete(id){
  state.lots=state.lots.filter(l=>l.id!==id);
  state.pays=state.pays.filter(p=>p.lotId!==id);
  if(payLotFilter===id)payLotFilter=null;
  saveToCloud();render();
}

/* ══ EXCEL IMPORT ══ */
let importData=null;
function openImportModal(){
  closeMenu();
  ['imp-step1','imp-step2','imp-step3','imp-step4'].forEach((id,i)=>document.getElementById(id).style.display=i===0?'block':'none');
  document.getElementById('imp-file').value='';
  document.getElementById('imp-foot').innerHTML='<button class="btn" style="flex:1;justify-content:center" onclick="closeModal(\'import\')">Cancel</button>';
  importData=null;
  document.getElementById('modal-import').classList.add('show');
}
function readImportFile(input){
  const file=input.files[0];if(!file)return;
  document.getElementById('imp-step1').style.display='none';
  document.getElementById('imp-step2').style.display='block';
  document.getElementById('imp-prog-text').textContent='Reading file...';
  document.getElementById('imp-prog-fill').style.width='20%';
  const reader=new FileReader();
  reader.onload=function(e){
    try{
      document.getElementById('imp-prog-fill').style.width='60%';
      const data=new Uint8Array(e.target.result);
      const wb=XLSX.read(data,{type:'array',cellDates:true});
      document.getElementById('imp-prog-fill').style.width='80%';
      const result=parseContractorExcel(wb);
      document.getElementById('imp-prog-fill').style.width='100%';
      importData=result;
      showImportPreview(result);
    }catch(err){
      document.getElementById('imp-step2').style.display='none';
      document.getElementById('imp-step1').style.display='block';
      alert('Error reading file: '+err.message);
    }
  };
  reader.readAsArrayBuffer(file);
}
function toDateStr(v){
  if(!v)return null;
  if(v instanceof Date)return v.toISOString().split('T')[0];
  const s=String(v);
  const m=s.match(/^(\d{1,2})[\/\-](\d{1,2})[\/\-](\d{2,4})$/);
  if(m){const yr=m[3].length===2?'20'+m[3]:m[3];return yr+'-'+m[2].padStart(2,'0')+'-'+m[1].padStart(2,'0');}
  return null;
}
function parseContractorExcel(wb){
  const newLots=[],newPays=[];
  let lotCtr=state.lotCtr,payCtr=state.payCtr,skipped=0;
  wb.SheetNames.forEach(sheetName=>{
    const ws=wb.Sheets[sheetName];
    const rows=XLSX.utils.sheet_to_json(ws,{header:1,defval:null,raw:false});
    if(rows.length<2)return;
    // Try to detect columns from header row
    const header=(rows[0]||[]).map(h=>String(h||'').toLowerCase());
    const ci={
      contractor: header.findIndex(h=>h.includes('contractor')||h.includes('name')),
      item:       header.findIndex(h=>h.includes('item')||h.includes('product')||h.includes('desc')),
      pieces:     header.findIndex(h=>h.includes('piece')||h.includes('qty')||h.includes('issued')),
      rate:       header.findIndex(h=>h.includes('rate')||h.includes('price')),
      dateIssued: header.findIndex(h=>h.includes('issued')||h.includes('out')||h.includes('date')),
      piecesRecv: header.findIndex(h=>h.includes('receiv')&&h.includes('piec')),
      dateRecv:   header.findIndex(h=>h.includes('receiv')&&(h.includes('date')||h.includes('in'))),
      status:     header.findIndex(h=>h.includes('status')),
      advance:    header.findIndex(h=>h.includes('advance')||h.includes('paid')||h.includes('payment')),
    };
    // If no header detected, try column positions directly
    const hasHeader=ci.contractor>=0||ci.pieces>=0||ci.rate>=0;
    const startRow=hasHeader?1:0;
    for(let i=startRow;i<rows.length;i++){
      const row=rows[i];if(!row||row.every(c=>!c))continue;
      const contractor=hasHeader&&ci.contractor>=0?String(row[ci.contractor]||'').trim():String(row[1]||'').trim();
      const item=hasHeader&&ci.item>=0?String(row[ci.item]||'').trim():String(row[2]||'').trim();
      const pieces=parseFloat(hasHeader&&ci.pieces>=0?row[ci.pieces]:row[3]);
      const rate=parseFloat(hasHeader&&ci.rate>=0?row[ci.rate]:row[4]);
      const dateIssued=toDateStr(hasHeader&&ci.dateIssued>=0?row[ci.dateIssued]:row[5])||today();
      const piecesRecv=parseFloat(hasHeader&&ci.piecesRecv>=0?row[ci.piecesRecv]:row[6])||null;
      const dateRecv=toDateStr(hasHeader&&ci.dateRecv>=0?row[ci.dateRecv]:row[7])||null;
      const advAmt=parseFloat(hasHeader&&ci.advance>=0?row[ci.advance]:row[9])||0;
      if(!contractor||!pieces||!rate||isNaN(pieces)||isNaN(rate))continue;
      // Check duplicate
      const dup=state.lots.find(l=>l.contractorName.toLowerCase()===contractor.toLowerCase()&&l.dateIssued===dateIssued&&Math.abs(l.piecesIssued-pieces)<2);
      if(dup){skipped++;continue;}
      lotCtr++;
      const lotId='LOT-'+pad(lotCtr);
      const status=piecesRecv!=null?'RECEIVED':'PENDING';
      newLots.push({id:lotId,contractorName:contractor,item:item||'Garment',piecesIssued:Math.round(pieces),rate,dateIssued,piecesReceived:isNaN(piecesRecv)?null:Math.round(piecesRecv),dateReceived:dateRecv,status,remarks:''});
      if(advAmt>0){payCtr++;newPays.push({id:'PAY-'+pad(payCtr),lotId,date:dateIssued,amount:advAmt,mode:'Cash',remarks:'Imported'});}
    }
  });
  return{newLots,newPays,lotCtr,payCtr,skipped};
}
function showImportPreview(r){
  document.getElementById('imp-step2').style.display='none';
  document.getElementById('imp-step3').style.display='block';
  const totalAdv=r.newPays.reduce((s,p)=>s+p.amount,0);
  document.getElementById('imp-summary').innerHTML=
    '<div style="display:grid;grid-template-columns:1fr 1fr;gap:8px;margin-bottom:10px">'+
    '<div style="background:#EFF6FF;border-radius:10px;padding:10px;text-align:center"><div style="font-size:20px;font-weight:700;color:#1F3864">'+r.newLots.length+'</div><div style="font-size:10px;color:#888">New Lots</div></div>'+
    '<div style="background:#ECFDF5;border-radius:10px;padding:10px;text-align:center"><div style="font-size:20px;font-weight:700;color:#059669">'+r.newPays.length+'</div><div style="font-size:10px;color:#888">Payments</div></div>'+
    '</div>'+
    '<div style="font-size:12px;color:#555;background:#F5F5F5;border-radius:8px;padding:10px 12px">'+
    '💰 Total Advance: <strong>'+fmt(totalAdv)+'</strong>'+
    (r.skipped>0?'<br>⏭ <strong>'+r.skipped+' lot(s) skipped</strong> (already exist)':'')+
    '</div>';
  if(!r.newLots.length){
    document.getElementById('imp-preview').innerHTML='<div style="text-align:center;padding:20px;color:#888;font-size:13px">No new lots to import — all already exist in the app.</div>';
    document.getElementById('imp-foot').innerHTML='<button class="btn primary" style="flex:1;justify-content:center" onclick="closeModal(\'import\')">OK</button>';
    return;
  }
  document.getElementById('imp-preview').innerHTML=r.newLots.map((l,i)=>{
    const pay=r.newPays.find(p=>p.lotId===l.id);
    return '<div style="background:'+(i%2?'#FAFAFA':'#fff')+';border:1px solid #E8E8E8;border-radius:8px;padding:10px 12px;margin-bottom:6px">'+
      '<div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:4px">'+
      '<div><strong>'+esc(l.contractorName)+'</strong> · '+esc(l.item)+'</div>'+
      '<span style="font-size:10px;font-weight:700;background:'+(l.status==='RECEIVED'?'#DBEAFE':'#FEF3C7')+';color:'+(l.status==='RECEIVED'?'#1E40AF':'#92400E')+';padding:2px 8px;border-radius:10px">'+l.status+'</span>'+
      '</div>'+
      '<div style="font-size:12px;color:#888">'+fmtDate(l.dateIssued)+' · '+fmtN(l.piecesIssued)+' pcs @ Rs.'+l.rate+' = '+fmt(l.piecesIssued*l.rate)+(pay?' · Adv: '+fmt(pay.amount):'')+'</div>'+
      '</div>';
  }).join('');
  document.getElementById('imp-foot').innerHTML=
    '<button class="btn" style="flex:1;justify-content:center" onclick="closeModal(\'import\')">Cancel</button>'+
    '<button class="btn primary" style="flex:1;justify-content:center" onclick="confirmImport()">✓ Import Now</button>';
}
async function confirmImport(){
  if(!importData)return;
  document.getElementById('imp-foot').innerHTML='<button class="btn" disabled style="flex:1;justify-content:center;opacity:.5">⏳ Saving...</button>';
  state.lots=[...state.lots,...importData.newLots];
  state.pays=[...state.pays,...importData.newPays];
  state.lotCtr=importData.lotCtr;state.payCtr=importData.payCtr;
  await saveToCloud();
  document.getElementById('imp-step3').style.display='none';
  document.getElementById('imp-step4').style.display='block';
  document.getElementById('imp-done-title').textContent='✅ Import Successful!';
  document.getElementById('imp-done-sub').textContent=importData.newLots.length+' new lots and '+importData.newPays.length+' payments added.';
  document.getElementById('imp-foot').innerHTML='<button class="btn primary" style="flex:1;justify-content:center" onclick="closeModal(\'import\')">Done</button>';
  render();
}

/* ══ PRINT LOT ══ */
function printLot(lotId){
  const lot=state.lots.find(l=>l.id===lotId);if(!lot)return;
  const lotPays=state.pays.filter(p=>p.lotId===lotId);
  const adv=lotPays.reduce((s,p)=>s+p.amount,0);
  const payable=(lot.piecesReceived!=null?lot.piecesReceived:lot.piecesIssued)*lot.rate;
  const bal=payable-adv;
  const miss=lot.piecesReceived!=null?lot.piecesIssued-lot.piecesReceived:null;
  const sc=lot.status==='PENDING'?'#92400E':lot.status==='RECEIVED'?'#1E40AF':'#065F46';
  const sb=lot.status==='PENDING'?'#FEF3C7':lot.status==='RECEIVED'?'#DBEAFE':'#D1FAE5';
  const payRows=lotPays.length?lotPays.map((p,i)=>`<tr style="background:${i%2?'#F9F9F9':'#fff'}"><td style="padding:8px 10px;border:1px solid #E0E0E0">${esc(p.id)}</td><td style="padding:8px 10px;border:1px solid #E0E0E0">${fmtDate(p.date)}</td><td style="padding:8px 10px;border:1px solid #E0E0E0;font-weight:600">${fmt(p.amount)}</td><td style="padding:8px 10px;border:1px solid #E0E0E0">${esc(p.mode)}</td><td style="padding:8px 10px;border:1px solid #E0E0E0;color:#666">${esc(p.remarks||'-')}</td></tr>`).join('')
    +`<tr style="background:#EFF6FF;font-weight:700"><td colspan="2" style="padding:8px 10px;border:1px solid #BFDBFE;text-align:right">Total Advance</td><td style="padding:8px 10px;border:1px solid #BFDBFE;color:#1E40AF">${fmt(adv)}</td><td colspan="2" style="border:1px solid #BFDBFE"></td></tr>`
    :`<tr><td colspan="5" style="padding:12px;text-align:center;color:#999;border:1px solid #E0E0E0">No advance payments recorded</td></tr>`;
  const recvSec=lot.piecesReceived!=null?`<div style="margin-top:20px"><div style="background:#1F3864;color:#fff;padding:8px 14px;border-radius:6px 6px 0 0;font-weight:700;font-size:13px">LOT RECEIPT</div><table style="width:100%;border-collapse:collapse;font-size:13px"><tr><td style="padding:9px 12px;border:1px solid #E0E0E0;width:35%;color:#555;font-weight:600">Date Received</td><td style="padding:9px 12px;border:1px solid #E0E0E0;font-weight:700">${fmtDate(lot.dateReceived)}</td></tr><tr style="background:#F9F9F9"><td style="padding:9px 12px;border:1px solid #E0E0E0;color:#555;font-weight:600">Pieces Received</td><td style="padding:9px 12px;border:1px solid #E0E0E0;font-weight:700">${fmtN(lot.piecesReceived)} pcs</td></tr><tr><td style="padding:9px 12px;border:1px solid #E0E0E0;color:#555;font-weight:600">Missing Pieces</td><td style="padding:9px 12px;border:1px solid #E0E0E0;font-weight:700;color:${miss>0?'#DC2626':'#059669'}">${miss>0?miss+' pcs SHORT':'NIL'}</td></tr><tr style="background:#F9F9F9"><td style="padding:9px 12px;border:1px solid #E0E0E0;color:#555;font-weight:600">Payable on Received</td><td style="padding:9px 12px;border:1px solid #E0E0E0;font-weight:700">${fmt(payable)}</td></tr></table></div>`:'';
  const w=window.open('','_blank');
  w.document.write(`<!DOCTYPE html><html><head><meta charset="UTF-8"><meta name="viewport" content="width=device-width,initial-scale=1"><title>${esc(lot.id)}</title><style>*{box-sizing:border-box;margin:0;padding:0}body{font-family:Arial,sans-serif;font-size:13px;color:#1A1A1A}@media print{.np{display:none!important}@page{margin:12mm;size:A4}}.pg{max-width:720px;margin:0 auto;padding:24px}.pb{background:#f0f0f0;padding:10px 16px;display:flex;gap:10px;align-items:center;margin:-24px -24px 24px}.pb button{padding:8px 20px;border-radius:6px;border:none;cursor:pointer;font-size:13px;font-family:Arial;font-weight:600}.top{display:flex;justify-content:space-between;border-bottom:3px solid #1F3864;padding-bottom:14px;margin-bottom:16px}.co{font-size:19px;font-weight:700;color:#1F3864}.cs{font-size:11px;color:#888;margin-top:2px}.meta{display:grid;grid-template-columns:1fr 1fr;border:1px solid #E0E0E0;border-radius:6px;overflow:hidden;margin-bottom:14px}.ml{padding:8px 12px;background:#F5F7FA;color:#555;font-weight:600;font-size:12px;border-bottom:1px solid #E8E8E8;border-right:1px solid #E8E8E8}.mv{padding:8px 12px;font-weight:700;border-bottom:1px solid #E8E8E8}.sec{background:#1F3864;color:#fff;padding:7px 12px;font-weight:700;font-size:12px;margin-top:14px}table{width:100%;border-collapse:collapse;font-size:12px}th{background:#E8EEF7;color:#1F3864;padding:8px 10px;text-align:left;border:1px solid #C8D5E8;font-size:11px}.sum{display:grid;grid-template-columns:1fr 1fr 1fr;gap:8px;margin:12px 0}.sb{border:1.5px solid #E0E0E0;border-radius:8px;padding:10px;text-align:center}.db{border:2px solid #1F3864;border-radius:8px;padding:13px 16px;display:flex;justify-content:space-between;align-items:center;margin:12px 0;background:#EFF6FF}.sig{display:grid;grid-template-columns:1fr 1fr;gap:24px;margin-top:28px;padding-top:16px;border-top:1px dashed #CCC}.sl{border-top:1.5px solid #1A1A1A;margin-top:36px;padding-top:5px;font-size:11px;font-weight:600;color:#555;text-align:center}.ft{text-align:center;font-size:10px;color:#AAA;padding:8px 0 0;border-top:1px solid #F0F0F0;margin-top:8px}</style></head><body><div class="pg">
<div class="pb np"><button onclick="window.print()" style="background:#1F3864;color:#fff">🖨 Print</button><button onclick="window.close()" style="background:#fff;border:1px solid #CCC">✕ Close</button></div>
<div class="top"><div><div class="co">🧵 Modrobe Impex</div><div class="cs">Garment Manufacturer · New Delhi</div></div><div style="text-align:right"><div style="font-size:13px;font-weight:700;color:#1F3864">JOB WORK VOUCHER</div><div style="font-size:22px;font-weight:700;color:#1E40AF">${esc(lot.id)}</div><div style="margin-top:5px;display:inline-block;background:${sb};color:${sc};padding:2px 12px;border-radius:20px;font-size:11px;font-weight:700">${lot.status}</div></div></div>
<div class="meta"><div class="ml">Contractor</div><div class="mv" style="color:#1F3864;font-size:14px">${esc(lot.contractorName)}</div><div class="ml">Item</div><div class="mv">${esc(lot.item)}</div><div class="ml">Date Issued</div><div class="mv">${fmtDate(lot.dateIssued)}</div><div class="ml">Date Received</div><div class="mv">${fmtDate(lot.dateReceived)}</div><div class="ml">Pieces Issued</div><div class="mv">${fmtN(lot.piecesIssued)} pcs</div><div class="ml">Rate per Piece</div><div class="mv">Rs. ${lot.rate} per piece</div></div>
${lot.remarks?`<div style="background:#FFFBEB;border:1px solid #FDE68A;border-radius:6px;padding:8px 12px;margin:10px 0;font-size:12px;color:#92400E"><strong>Remarks:</strong> ${esc(lot.remarks)}</div>`:''}
<div class="sum"><div class="sb"><div style="font-size:10px;color:#888;margin-bottom:4px">Total Agreed</div><div style="font-size:16px;font-weight:700;color:#1F3864">${fmt(lot.piecesIssued*lot.rate)}</div></div><div class="sb"><div style="font-size:10px;color:#888;margin-bottom:4px">Advance Paid</div><div style="font-size:16px;font-weight:700;color:#D97706">${fmt(adv)}</div></div><div class="sb"><div style="font-size:10px;color:#888;margin-bottom:4px">Balance Due</div><div style="font-size:16px;font-weight:700;color:${bal>0?'#DC2626':'#059669'}">${fmt(Math.abs(bal))}</div></div></div>
<div class="db"><div style="font-size:13px;font-weight:600;color:#1F3864">NET BALANCE ${bal>0?'PAYABLE TO CONTRACTOR':'(SETTLED)'}</div><div style="font-size:22px;font-weight:700;color:${bal>0?'#DC2626':'#059669'}">${fmt(Math.abs(bal))}${bal<0?' (CR)':''}</div></div>
<div class="sec">ADVANCE / PAYMENT HISTORY</div>
<table><thead><tr><th style="width:12%">Pay ID</th><th style="width:18%">Date</th><th style="width:18%">Amount</th><th style="width:15%">Mode</th><th>Remarks</th></tr></thead><tbody>${payRows}</tbody></table>
${recvSec}
<div class="sig"><div><div class="sl">Contractor's Signature<br><strong>${esc(lot.contractorName)}</strong></div></div><div><div class="sl">Authorised Signatory<br><strong>Modrobe Impex</strong></div></div></div>
<div class="ft">Generated: ${new Date().toLocaleString('en-IN')} · ${esc(lot.id)}</div>
</div></body></html>`);
  w.document.close();
}
</script>
</body>
</html>
