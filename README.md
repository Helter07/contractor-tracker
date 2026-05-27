[Garment_Contractor_Tracker.html](https://github.com/user-attachments/files/28304485/Garment_Contractor_Tracker.html)
# contractor-tracker<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Garment Contractor Tracker</title>
<style>
  * { box-sizing: border-box; margin: 0; padding: 0; }
  body { font-family: Arial, sans-serif; background: #F5F5F5; color: #1A1A1A; font-size: 14px; }
  .header { background: #1F3864; color: #fff; padding: 16px 24px; display: flex; align-items: center; justify-content: space-between; }
  .header p { font-size: 12px; color: #B0C4DE; margin-top: 2px; }
  .container { max-width: 1100px; margin: 0 auto; padding: 20px 16px; }
  .stats { display: grid; grid-template-columns: repeat(4,1fr); gap: 12px; margin-bottom: 20px; }
  .stat-card { background: #fff; border-radius: 10px; padding: 14px 16px; border: 1px solid #E0E0E0; }
  .stat-label { font-size: 11px; color: #888; margin-bottom: 6px; }
  .stat-val { font-size: 22px; font-weight: 600; }
  .stat-val.warn { color: #D97706; } .stat-val.red { color: #DC2626; }
  .tabs { display: flex; border-bottom: 2px solid #E0E0E0; margin-bottom: 16px; }
  .tab-btn { background: none; border: none; padding: 10px 20px; font-size: 13px; cursor: pointer; color: #888; border-bottom: 2px solid transparent; margin-bottom: -2px; font-family: Arial; }
  .tab-btn.active { color: #1F3864; font-weight: 600; border-bottom: 2px solid #1F3864; }
  .filter-bar { display: flex; gap: 6px; margin-bottom: 14px; flex-wrap: wrap; }
  .filter-btn { background: #fff; border: 1px solid #D0D0D0; border-radius: 20px; padding: 4px 14px; font-size: 12px; cursor: pointer; font-family: Arial; color: #555; }
  .filter-btn.active { background: #1F3864; color: #fff; border-color: #1F3864; font-weight: 600; }
  .btn { border: 1px solid #C0C0C0; border-radius: 7px; padding: 7px 14px; font-size: 13px; background: #fff; cursor: pointer; font-family: Arial; display: inline-flex; align-items: center; gap: 5px; }
  .btn:hover { background: #F0F0F0; }
  .btn.primary { background: #1F3864; color: #fff; border-color: #1F3864; font-weight: 600; }
  .btn.primary:hover { background: #16305A; }
  .btn.success { color: #065F46; border-color: #6EE7B7; }
  .btn.danger { color: #DC2626; border-color: #FCA5A5; }
  .btn.print-btn { color: #6D28D9; border-color: #C4B5FD; background: #F5F3FF; }
  .btn.print-btn:hover { background: #EDE9FE; }
  .btn.sm { padding: 5px 11px; font-size: 12px; }
  .lot-card { background: #fff; border: 1px solid #E0E0E0; border-radius: 12px; padding: 16px 18px; margin-bottom: 10px; }
  .lot-card-header { display: flex; align-items: flex-start; justify-content: space-between; margin-bottom: 12px; }
  .lot-id-badge { background: #DBEAFE; color: #1E40AF; font-size: 12px; font-weight: 600; padding: 3px 12px; border-radius: 20px; white-space: nowrap; }
  .lot-name { font-size: 15px; font-weight: 600; }
  .lot-item { font-size: 12px; color: #888; margin-top: 1px; }
  .status-badge { font-size: 11px; font-weight: 600; padding: 3px 12px; border-radius: 20px; flex-shrink: 0; }
  .status-PENDING { background: #FEF3C7; color: #92400E; }
  .status-RECEIVED { background: #DBEAFE; color: #1E40AF; }
  .status-SETTLED { background: #D1FAE5; color: #065F46; }
  .lot-nums { display: grid; grid-template-columns: repeat(5,1fr); gap: 6px; margin-bottom: 10px; }
  .num-box { background: #F7F7F7; border-radius: 8px; padding: 8px 10px; }
  .num-box .nl { font-size: 10px; color: #888; margin-bottom: 3px; }
  .num-box .nv { font-size: 13px; font-weight: 600; }
  .num-box .nv.warn { color: #D97706; } .num-box .nv.red { color: #DC2626; }
  .lot-meta { display: flex; gap: 16px; font-size: 12px; color: #888; margin-bottom: 10px; flex-wrap: wrap; align-items: center; }
  .lot-meta .ok { color: #059669; font-weight: 600; }
  .lot-meta .miss { color: #DC2626; font-weight: 600; }
  .lot-meta .note { font-style: italic; color: #AAA; }
  .lot-actions { display: flex; gap: 6px; flex-wrap: wrap; align-items: center; }
  .table-wrap { border: 1px solid #E0E0E0; border-radius: 10px; overflow: hidden; }
  table { width: 100%; border-collapse: collapse; font-size: 13px; table-layout: fixed; }
  thead tr { background: #F5F5F5; }
  th { padding: 10px 11px; text-align: left; font-weight: 600; color: #666; font-size: 11px; border-bottom: 1px solid #E0E0E0; }
  td { padding: 9px 11px; border-bottom: 1px solid #F0F0F0; overflow: hidden; text-overflow: ellipsis; white-space: nowrap; }
  tr:last-child td { border-bottom: none; }
  tr.alt { background: #FAFAFA; }
  .mode-pill { background: #F0F0F0; border: 1px solid #DDD; padding: 2px 8px; border-radius: 12px; font-size: 11px; }
  .total-row td { font-weight: 600; background: #F0F4FF; }
  .empty { text-align: center; padding: 3rem; color: #AAA; border: 1px solid #E0E0E0; border-radius: 12px; background: #fff; }
  .empty-icon { font-size: 40px; margin-bottom: 8px; }
  .overlay { display: none; position: fixed; inset: 0; background: rgba(0,0,0,.45); z-index: 200; align-items: flex-start; justify-content: center; padding-top: 40px; overflow-y: auto; }
  .overlay.show { display: flex; }
  .modal { background: #fff; border-radius: 14px; border: 1px solid #DDD; padding: 24px; width: 100%; max-width: 480px; margin: 0 16px 40px; }
  .modal-header { display: flex; justify-content: space-between; align-items: flex-start; margin-bottom: 20px; }
  .modal-subtitle { font-size: 12px; color: #888; margin-top: 2px; }
  .close-btn { background: none; border: none; cursor: pointer; font-size: 22px; color: #888; padding: 0 4px; line-height: 1; }
  .close-btn:hover { color: #333; }
  .form-group { margin-bottom: 13px; }
  .form-row { display: grid; grid-template-columns: 1fr 1fr; gap: 12px; margin-bottom: 13px; }
  label { font-size: 12px; color: #666; display: block; margin-bottom: 4px; font-weight: 500; }
  input, select { width: 100%; padding: 8px 10px; font-size: 13px; border: 1px solid #CCC; border-radius: 7px; font-family: Arial; background: #fff; color: #1A1A1A; outline: none; }
  input:focus, select:focus { border-color: #1F3864; box-shadow: 0 0 0 2px #DBEAFE; }
  .form-actions { display: flex; gap: 8px; justify-content: flex-end; margin-top: 16px; }
  .info-box { padding: 10px 14px; border-radius: 8px; font-size: 13px; margin-top: 2px; font-weight: 600; }
  .info-box.blue { background: #EFF6FF; color: #1D4ED8; }
  .info-box.yellow { background: #FFFBEB; color: #92400E; }
  .info-box.green { background: #ECFDF5; color: #065F46; }
  .info-box.red { background: #FEF2F2; color: #B91C1C; }
  .pay-header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 12px; flex-wrap: wrap; gap: 8px; }
  .lot-filter-tag { background: #DBEAFE; color: #1E40AF; font-size: 12px; padding: 3px 10px; border-radius: 20px; display: inline-flex; align-items: center; gap: 6px; }
  .lot-filter-tag .remove { cursor: pointer; font-weight: 700; font-size: 14px; }
  .confirm-body { font-size: 13px; color: #555; line-height: 1.6; margin-bottom: 18px; }
  h1 { font-size: 20px; font-weight: 600; }
  h2 { font-size: 16px; font-weight: 600; }
  @media(max-width:640px) {
    .stats { grid-template-columns: 1fr 1fr; }
    .lot-nums { grid-template-columns: repeat(3,1fr); }
    .form-row { grid-template-columns: 1fr; }
    .header { flex-direction: column; align-items: flex-start; gap: 8px; }
  }
</style>
</head>
<body>

<div class="header">
  <div>
    <h1>&#x1F9F5; Garment Contractor Tracker</h1>
    <p>Track lots &middot; advance payments &middot; settlements</p>
  </div>
  <button class="btn primary" onclick="openModal('addLot')">+ New Lot</button>
</div>

<div class="container">
  <div class="stats">
    <div class="stat-card"><div class="stat-label">&#x1F4E6; Total Lots</div><div class="stat-val" id="st-total">0</div></div>
    <div class="stat-card"><div class="stat-label">&#x23F3; Pending</div><div class="stat-val warn" id="st-pending">0</div></div>
    <div class="stat-card"><div class="stat-label">&#x1F4B5; Advance Paid</div><div class="stat-val" id="st-adv">Rs.0</div></div>
    <div class="stat-card"><div class="stat-label">&#x1F4B0; Balance Due</div><div class="stat-val red" id="st-bal">Rs.0</div></div>
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

<!-- ADD LOT MODAL -->
<div class="overlay" id="modal-addLot">
  <div class="modal">
    <div class="modal-header">
      <h2>New Cutting Lot</h2>
      <button class="close-btn" onclick="closeModal('addLot')">&#x2715;</button>
    </div>
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
    <div class="form-actions">
      <button class="btn" onclick="closeModal('addLot')">Cancel</button>
      <button class="btn primary" onclick="addLot()">Create Lot</button>
    </div>
  </div>
</div>

<!-- ADD PAYMENT MODAL -->
<div class="overlay" id="modal-addPayment">
  <div class="modal">
    <div class="modal-header">
      <h2>Record Payment</h2>
      <button class="close-btn" onclick="closeModal('addPayment')">&#x2715;</button>
    </div>
    <div class="form-group">
      <label>Lot No. *</label>
      <select id="p-lotid" onchange="previewPay()"><option value="">Select lot...</option></select>
    </div>
    <div class="form-row">
      <div><label>Payment Date *</label><input id="p-date" type="date"></div>
      <div><label>Amount (Rs.) *</label><input id="p-amount" type="number" placeholder="2000" oninput="previewPay()"></div>
    </div>
    <div class="form-group">
      <label>Mode of Payment</label>
      <select id="p-mode"><option>Cash</option><option>UPI</option><option>Bank Transfer</option><option>Cheque</option><option>Other</option></select>
    </div>
    <div class="form-group"><label>Remarks</label><input id="p-remarks" placeholder="e.g. Running advance"></div>
    <div class="info-box yellow" id="pay-preview" style="display:none;margin-bottom:8px"></div>
    <div class="form-actions">
      <button class="btn" onclick="closeModal('addPayment')">Cancel</button>
      <button class="btn primary" onclick="addPayment()">Save Payment</button>
    </div>
  </div>
</div>

<!-- RECEIVE LOT MODAL -->
<div class="overlay" id="modal-receive">
  <div class="modal">
    <div class="modal-header">
      <div><h2>Receive Lot Back</h2><div class="modal-subtitle" id="recv-subtitle"></div></div>
      <button class="close-btn" onclick="closeModal('receive')">&#x2715;</button>
    </div>
    <div class="info-box blue" id="recv-issued" style="margin-bottom:14px"></div>
    <div class="form-row">
      <div><label>Pieces Received *</label><input id="r-pieces" type="number" oninput="previewRecv()"></div>
      <div><label>Date Received *</label><input id="r-date" type="date"></div>
    </div>
    <div class="info-box green" id="recv-preview" style="display:none;margin-bottom:8px"></div>
    <div class="form-actions">
      <button class="btn" onclick="closeModal('receive')">Cancel</button>
      <button class="btn primary" onclick="receiveLot()">Confirm Receipt</button>
    </div>
  </div>
</div>

<!-- CONFIRM DELETE -->
<div class="overlay" id="modal-confirm">
  <div class="modal" style="max-width:380px">
    <div class="modal-header">
      <h2>Delete lot?</h2>
      <button class="close-btn" onclick="closeModal('confirm')">&#x2715;</button>
    </div>
    <p class="confirm-body" id="confirm-msg"></p>
    <div class="form-actions">
      <button class="btn" onclick="closeModal('confirm')">Cancel</button>
      <button class="btn danger" id="confirm-ok-btn">Delete</button>
    </div>
  </div>
</div>

<script>
const LSKEY = 'gmt_html_v1';
let state = { lots: [], pays: [], lotCtr: 0, payCtr: 0 };
function load() { try { const d = localStorage.getItem(LSKEY); if (d) state = JSON.parse(d); } catch {} }
function save() { try { localStorage.setItem(LSKEY, JSON.stringify(state)); } catch {} }

const pad = n => String(n).padStart(3,'0');
const fmt = n => 'Rs.' + new Intl.NumberFormat('en-IN').format(Math.round(n||0));
const fmtN = n => new Intl.NumberFormat('en-IN').format(Math.round(n||0));
const fmtDate = d => d ? new Date(d+'T00:00:00').toLocaleDateString('en-IN',{day:'2-digit',month:'short',year:'numeric'}) : '-';
const today = () => new Date().toISOString().split('T')[0];
const esc = s => String(s||'').replace(/&/g,'&amp;').replace(/</g,'&lt;').replace(/>/g,'&gt;').replace(/"/g,'&quot;');

function advPaid(lid) { return state.pays.filter(p=>p.lotId===lid).reduce((s,p)=>s+p.amount,0); }
function balDue(lot) { return ((lot.piecesReceived!=null?lot.piecesReceived:lot.piecesIssued)*lot.rate) - advPaid(lot.id); }

let curFilter='ALL', curTab='lots', payLotFilter=null, currentReceiveLotId=null;

function render() { renderStats(); renderLots(); renderPays(); renderTabCounts(); }

function renderStats() {
  const adv = state.pays.reduce((s,p)=>s+p.amount,0);
  const bal = state.lots.reduce((s,l)=>s+Math.max(0,balDue(l)),0);
  document.getElementById('st-total').textContent = state.lots.length;
  document.getElementById('st-pending').textContent = state.lots.filter(l=>l.status==='PENDING').length;
  document.getElementById('st-adv').textContent = fmt(adv);
  document.getElementById('st-bal').textContent = fmt(bal);
}
function renderTabCounts() {
  document.getElementById('tab-lots-count').textContent = state.lots.length;
  document.getElementById('tab-pays-count').textContent = state.pays.length;
}

function renderLots() {
  const list = document.getElementById('lots-list');
  const vis = curFilter==='ALL' ? state.lots : state.lots.filter(l=>l.status===curFilter);
  if (!vis.length) {
    list.innerHTML = '<div class="empty"><div class="empty-icon">&#x1F4E6;</div>'+(curFilter==='ALL'?'No lots yet. Click <b>New Lot</b> to get started.':'No '+curFilter.toLowerCase()+' lots.')+'</div>';
    return;
  }
  list.innerHTML = [...vis].reverse().map(lot => {
    const adv = advPaid(lot.id);
    const payable = (lot.piecesReceived!=null?lot.piecesReceived:lot.piecesIssued)*lot.rate;
    const bal = payable - adv;
    const miss = lot.piecesReceived!=null ? lot.piecesIssued - lot.piecesReceived : null;
    const lotPayCount = state.pays.filter(p=>p.lotId===lot.id).length;
    const metaPcs = miss!=null ? '<span class="'+(miss>0?'miss':'ok')+'">'+(miss>0?'&#9888; ':'&#10003; ')+'Received: '+fmtN(lot.piecesReceived)+' pcs'+(miss>0?' ('+miss+' missing)':'')+'</span>' : '';
    const metaNote = lot.remarks ? '<span class="note">"'+esc(lot.remarks)+'"</span>' : '';
    const actPay = (lot.status==='PENDING'||lot.status==='RECEIVED') ? '<button class="btn sm" onclick="openModal(\'addPayment\',\''+lot.id+'\')">&#x1F4B5; + Payment</button>' : '';
    const actRecv = lot.status==='PENDING' ? '<button class="btn sm" onclick="openReceive(\''+lot.id+'\')">&#x1F4E5; Receive Lot</button>' : '';
    const actSettle = lot.status==='RECEIVED' ? '<button class="btn sm success" onclick="settleLot(\''+lot.id+'\')">&#x2705; Mark Settled</button>' : '';
    return '<div class="lot-card">'+
      '<div class="lot-card-header"><div style="display:flex;align-items:center;gap:10px"><span class="lot-id-badge">'+esc(lot.id)+'</span><div><div class="lot-name">'+esc(lot.contractorName)+'</div><div class="lot-item">'+esc(lot.item)+'</div></div></div><span class="status-badge status-'+lot.status+'">'+lot.status+'</span></div>'+
      '<div class="lot-nums">'+
        '<div class="num-box"><div class="nl">Issued</div><div class="nv">'+fmtN(lot.piecesIssued)+' pcs</div></div>'+
        '<div class="num-box"><div class="nl">Rate</div><div class="nv">Rs.'+lot.rate+'/pc</div></div>'+
        '<div class="num-box"><div class="nl">Total Agreed</div><div class="nv">'+fmt(lot.piecesIssued*lot.rate)+'</div></div>'+
        '<div class="num-box"><div class="nl">Advance Paid</div><div class="nv '+(adv>0?'warn':'')+'">'+fmt(adv)+'</div></div>'+
        '<div class="num-box"><div class="nl">'+(lot.status==='PENDING'?'Est. Balance':'Balance Due')+'</div><div class="nv '+(bal>0?'red':'')+'">'+fmt(bal)+'</div></div>'+
      '</div>'+
      '<div class="lot-meta"><span>&#x1F4E4; Out: '+fmtDate(lot.dateIssued)+'</span><span>&#x1F4E5; In: '+fmtDate(lot.dateReceived)+'</span>'+metaPcs+metaNote+'</div>'+
      '<div class="lot-actions">'+actPay+actRecv+actSettle+
        '<button class="btn sm print-btn" onclick="printLot(\''+lot.id+'\')" title="Print contractor copy">&#x1F5A8; Print Copy</button>'+
        '<button class="btn sm" style="margin-left:auto" onclick="viewLotPayments(\''+lot.id+'\')">&#x1F552; Payments ('+lotPayCount+')</button>'+
        '<button class="btn sm danger" onclick="confirmDelete(\''+lot.id+'\')">&#x1F5D1; Delete</button>'+
      '</div>'+
    '</div>';
  }).join('');
}

function renderPays() {
  const visPays = payLotFilter ? state.pays.filter(p=>p.lotId===payLotFilter) : state.pays;
  const total = visPays.reduce((s,p)=>s+p.amount,0);
  const tag = document.getElementById('pay-lot-tag');
  if (payLotFilter) { tag.style.display='inline-flex'; tag.innerHTML='Lot: '+esc(payLotFilter)+' <span class="remove" onclick="clearPayFilter()">&#x00D7;</span>'; }
  else { tag.style.display='none'; }
  document.getElementById('pay-meta').textContent = visPays.length+' record(s) · Total: '+fmt(total);
  const el = document.getElementById('pays-list');
  if (!visPays.length) { el.innerHTML='<div class="empty"><div class="empty-icon">&#x1F4B5;</div>No payments recorded yet.</div>'; return; }
  const rows = [...visPays].reverse().map((p,i) => {
    const lot = state.lots.find(l=>l.id===p.lotId);
    return '<tr class="'+(i%2?'alt':'')+'"><td style="color:#888">'+esc(p.id)+'</td><td>'+fmtDate(p.date)+'</td><td style="color:#1E40AF;font-weight:600">'+esc(p.lotId)+'</td><td>'+esc(lot?lot.contractorName:'-')+'</td><td style="font-weight:600">'+fmt(p.amount)+'</td><td><span class="mode-pill">'+esc(p.mode)+'</span></td><td style="color:#888;font-style:'+(p.remarks?'italic':'normal')+'">'+esc(p.remarks||'-')+'</td></tr>';
  }).join('');
  el.innerHTML='<div class="table-wrap"><table><thead><tr><th style="width:9%">Pay ID</th><th style="width:13%">Date</th><th style="width:9%">Lot</th><th style="width:18%">Contractor</th><th style="width:12%">Amount</th><th style="width:11%">Mode</th><th style="width:28%">Remarks</th></tr></thead><tbody>'+rows+'<tr class="total-row"><td colspan="4" style="text-align:right;padding-right:14px">Total</td><td>'+fmt(total)+'</td><td colspan="2"></td></tr></tbody></table></div>';
}

function switchTab(t) {
  curTab=t;
  document.getElementById('panel-lots').style.display=t==='lots'?'block':'none';
  document.getElementById('panel-pays').style.display=t==='pays'?'block':'none';
  document.getElementById('tab-lots-btn').className='tab-btn'+(t==='lots'?' active':'');
  document.getElementById('tab-pays-btn').className='tab-btn'+(t==='pays'?' active':'');
}

function setFilter(f,btn) {
  curFilter=f;
  document.querySelectorAll('.filter-btn').forEach(b=>b.classList.remove('active'));
  btn.classList.add('active');
  renderLots();
}

function viewLotPayments(lid) { payLotFilter=lid; switchTab('pays'); renderPays(); }
function clearPayFilter() { payLotFilter=null; renderPays(); }

function openModal(type, lotId) {
  if (type==='addLot') {
    ['f-contractor','f-item','f-pieces','f-rate','f-remarks'].forEach(id=>document.getElementById(id).value='');
    document.getElementById('f-date').value=today();
    document.getElementById('lot-preview').style.display='none';
  }
  if (type==='addPayment') {
    const sel=document.getElementById('p-lotid');
    sel.innerHTML='<option value="">Select lot...</option>';
    state.lots.filter(l=>l.status!=='SETTLED').forEach(l=>{const o=document.createElement('option');o.value=l.id;o.textContent=l.id+' - '+l.contractorName+' ('+l.item+')';sel.appendChild(o);});
    if (lotId) sel.value=lotId;
    document.getElementById('p-date').value=today();
    document.getElementById('p-amount').value='';
    document.getElementById('p-mode').value='Cash';
    document.getElementById('p-remarks').value='';
    document.getElementById('pay-preview').style.display='none';
    previewPay();
  }
  document.getElementById('modal-'+type).classList.add('show');
}

function openReceive(lotId) {
  currentReceiveLotId=lotId;
  const lot=state.lots.find(l=>l.id===lotId);
  document.getElementById('recv-subtitle').textContent=lot.id+' - '+lot.contractorName;
  document.getElementById('recv-issued').textContent='Issued: '+fmtN(lot.piecesIssued)+' pcs @ Rs.'+lot.rate+' = '+fmt(lot.piecesIssued*lot.rate);
  document.getElementById('r-pieces').value='';
  document.getElementById('r-pieces').placeholder=lot.piecesIssued;
  document.getElementById('r-date').value=today();
  document.getElementById('recv-preview').style.display='none';
  document.getElementById('modal-receive').classList.add('show');
}

function closeModal(type) { document.getElementById('modal-'+type).classList.remove('show'); }

function confirmDelete(lotId) {
  document.getElementById('confirm-msg').innerHTML='This will permanently delete <strong>'+esc(lotId)+'</strong> and all its payments. This cannot be undone.';
  document.getElementById('confirm-ok-btn').onclick=function(){doDelete(lotId);closeModal('confirm');};
  document.getElementById('modal-confirm').classList.add('show');
}

document.querySelectorAll('.overlay').forEach(o=>{ o.addEventListener('click',e=>{if(e.target===o)o.classList.remove('show');}); });

function previewLot() {
  const pcs=+document.getElementById('f-pieces').value, rate=+document.getElementById('f-rate').value, box=document.getElementById('lot-preview');
  if(pcs&&rate){box.style.display='block';box.textContent='Total agreed amount: '+fmt(pcs*rate);}else box.style.display='none';
}

function previewPay() {
  const lid=document.getElementById('p-lotid').value, amt=+document.getElementById('p-amount').value, box=document.getElementById('pay-preview');
  if(lid&&amt){box.style.display='block';box.textContent='Total advance after this payment: '+fmt(advPaid(lid)+amt);}else box.style.display='none';
}

function previewRecv() {
  const lot=state.lots.find(l=>l.id===currentReceiveLotId), pcs=+document.getElementById('r-pieces').value, box=document.getElementById('recv-preview');
  if(!pcs||!lot){box.style.display='none';return;}
  const miss=lot.piecesIssued-pcs; box.style.display='block';
  if(miss>0){box.className='info-box red';box.textContent='Warning: '+miss+' pcs missing - Payable: '+fmt(pcs*lot.rate);}
  else{box.className='info-box green';box.textContent='All pieces received - Payable: '+fmt(pcs*lot.rate);}
}

function addLot() {
  const c=document.getElementById('f-contractor').value.trim(), it=document.getElementById('f-item').value.trim(),
        pcs=+document.getElementById('f-pieces').value, rate=+document.getElementById('f-rate').value,
        dt=document.getElementById('f-date').value, rem=document.getElementById('f-remarks').value.trim();
  if(!c||!it||!pcs||!rate||!dt){alert('Please fill all required fields.');return;}
  state.lotCtr++;
  state.lots.push({id:'LOT-'+pad(state.lotCtr),contractorName:c,item:it,piecesIssued:pcs,rate:rate,dateIssued:dt,piecesReceived:null,dateReceived:null,status:'PENDING',remarks:rem});
  save();render();closeModal('addLot');
}

function addPayment() {
  const lid=document.getElementById('p-lotid').value, dt=document.getElementById('p-date').value,
        amt=+document.getElementById('p-amount').value, mode=document.getElementById('p-mode').value,
        rem=document.getElementById('p-remarks').value.trim();
  if(!lid||!dt||!amt){alert('Please fill all required fields.');return;}
  state.payCtr++;
  state.pays.push({id:'PAY-'+pad(state.payCtr),lotId:lid,date:dt,amount:amt,mode:mode,remarks:rem});
  save();render();closeModal('addPayment');
}

function receiveLot() {
  const pcs=+document.getElementById('r-pieces').value, dt=document.getElementById('r-date').value;
  if(!pcs||!dt){alert('Please fill all required fields.');return;}
  const lot=state.lots.find(l=>l.id===currentReceiveLotId);
  if(!lot)return;
  lot.piecesReceived=pcs;lot.dateReceived=dt;lot.status='RECEIVED';
  save();render();closeModal('receive');
}

function settleLot(id) {
  const lot=state.lots.find(l=>l.id===id);
  if(!lot)return;
  lot.status='SETTLED';save();render();
}

function doDelete(id) {
  state.lots=state.lots.filter(l=>l.id!==id);
  state.pays=state.pays.filter(p=>p.lotId!==id);
  if(payLotFilter===id)payLotFilter=null;
  save();render();
}

/* ─────────────────────────────────────────────
   PRINT FUNCTION
   Opens a new window with a formatted voucher
   ready to print and hand to the contractor.
───────────────────────────────────────────── */
function printLot(lotId) {
  const lot = state.lots.find(l=>l.id===lotId);
  if(!lot) return;

  const lotPays = state.pays.filter(p=>p.lotId===lotId);
  const adv     = lotPays.reduce((s,p)=>s+p.amount,0);
  const payable = (lot.piecesReceived!=null ? lot.piecesReceived : lot.piecesIssued) * lot.rate;
  const bal     = payable - adv;
  const miss    = lot.piecesReceived!=null ? lot.piecesIssued - lot.piecesReceived : null;

  const statusColor = lot.status==='PENDING'?'#92400E': lot.status==='RECEIVED'?'#1E40AF':'#065F46';
  const statusBg    = lot.status==='PENDING'?'#FEF3C7': lot.status==='RECEIVED'?'#DBEAFE':'#D1FAE5';

  const payRows = lotPays.length
    ? lotPays.map((p,i)=>`
        <tr style="background:${i%2?'#F9F9F9':'#fff'}">
          <td style="padding:8px 10px;border:1px solid #E0E0E0">${esc(p.id)}</td>
          <td style="padding:8px 10px;border:1px solid #E0E0E0">${fmtDate(p.date)}</td>
          <td style="padding:8px 10px;border:1px solid #E0E0E0;font-weight:600">${fmt(p.amount)}</td>
          <td style="padding:8px 10px;border:1px solid #E0E0E0">${esc(p.mode)}</td>
          <td style="padding:8px 10px;border:1px solid #E0E0E0;color:#666;font-style:italic">${esc(p.remarks||'-')}</td>
        </tr>`).join('')+`
        <tr style="background:#EFF6FF;font-weight:700">
          <td colspan="2" style="padding:8px 10px;border:1px solid #BFDBFE;text-align:right">Total Advance Paid</td>
          <td style="padding:8px 10px;border:1px solid #BFDBFE;color:#1E40AF">${fmt(adv)}</td>
          <td colspan="2" style="border:1px solid #BFDBFE"></td>
        </tr>`
    : `<tr><td colspan="5" style="padding:12px;text-align:center;color:#999;border:1px solid #E0E0E0">No advance payments recorded</td></tr>`;

  const receivedSection = lot.piecesReceived!=null ? `
    <div style="margin-top:24px">
      <div style="background:#1F3864;color:#fff;padding:8px 14px;border-radius:6px 6px 0 0;font-weight:700;font-size:13px;letter-spacing:.5px">LOT RECEIPT DETAILS</div>
      <table style="width:100%;border-collapse:collapse;font-size:13px">
        <tr>
          <td style="padding:10px 14px;border:1px solid #E0E0E0;width:35%;color:#555;font-weight:600">Date Received</td>
          <td style="padding:10px 14px;border:1px solid #E0E0E0;font-weight:700">${fmtDate(lot.dateReceived)}</td>
        </tr>
        <tr style="background:#F9F9F9">
          <td style="padding:10px 14px;border:1px solid #E0E0E0;color:#555;font-weight:600">Pieces Received</td>
          <td style="padding:10px 14px;border:1px solid #E0E0E0;font-weight:700">${fmtN(lot.piecesReceived)} pcs</td>
        </tr>
        <tr>
          <td style="padding:10px 14px;border:1px solid #E0E0E0;color:#555;font-weight:600">Missing Pieces</td>
          <td style="padding:10px 14px;border:1px solid #E0E0E0;font-weight:700;color:${miss>0?'#DC2626':'#059669'}">${miss>0?miss+' pcs SHORT':'NIL - All pieces received'}</td>
        </tr>
        <tr style="background:#F9F9F9">
          <td style="padding:10px 14px;border:1px solid #E0E0E0;color:#555;font-weight:600">Payable on Received</td>
          <td style="padding:10px 14px;border:1px solid #E0E0E0;font-weight:700">${fmt(payable)}</td>
        </tr>
      </table>
    </div>` : '';

  const printedOn = new Date().toLocaleDateString('en-IN',{day:'2-digit',month:'long',year:'numeric',hour:'2-digit',minute:'2-digit'});

  const html = `<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Job Work Voucher - ${esc(lot.id)}</title>
<style>
  * { box-sizing: border-box; margin: 0; padding: 0; }
  body { font-family: Arial, sans-serif; color: #1A1A1A; font-size: 13px; background: #fff; }
  @media print {
    body { margin: 0; }
    .no-print { display: none !important; }
    .page { box-shadow: none !important; margin: 0 !important; max-width: 100% !important; }
    @page { margin: 14mm 12mm; size: A4; }
  }
  .page { max-width: 720px; margin: 24px auto; background: #fff; box-shadow: 0 2px 20px rgba(0,0,0,.12); }
  .print-bar { background: #f0f0f0; padding: 10px 16px; display: flex; gap: 10px; align-items: center; }
  .print-bar button { padding: 8px 20px; border-radius: 6px; border: none; cursor: pointer; font-size: 13px; font-family: Arial; font-weight: 600; }
  .btn-print { background: #1F3864; color: #fff; }
  .btn-close { background: #fff; border: 1px solid #CCC !important; color: #333; }
  .voucher { padding: 28px 32px; }
  .top-header { display: flex; justify-content: space-between; align-items: flex-start; border-bottom: 3px solid #1F3864; padding-bottom: 16px; margin-bottom: 20px; }
  .company-name { font-size: 22px; font-weight: 700; color: #1F3864; }
  .company-sub { font-size: 12px; color: #888; margin-top: 3px; }
  .voucher-title { text-align: right; }
  .voucher-title h2 { font-size: 17px; color: #1F3864; font-weight: 700; }
  .voucher-title .lot-id { font-size: 22px; font-weight: 700; color: #1E40AF; margin-top: 2px; }
  .meta-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 0; margin-bottom: 20px; border: 1px solid #E0E0E0; border-radius: 6px; overflow: hidden; }
  .meta-row { display: contents; }
  .meta-label { padding: 9px 14px; background: #F5F7FA; color: #555; font-weight: 600; font-size: 12px; border-bottom: 1px solid #E8E8E8; border-right: 1px solid #E8E8E8; }
  .meta-value { padding: 9px 14px; font-weight: 700; font-size: 13px; border-bottom: 1px solid #E8E8E8; }
  .meta-label:nth-last-child(-n+2) { border-bottom: none; }
  .meta-value:nth-last-child(-n+1) { border-bottom: none; }
  .section-title { background: #1F3864; color: #fff; padding: 8px 14px; border-radius: 6px 6px 0 0; font-weight: 700; font-size: 13px; letter-spacing: .5px; margin-top: 20px; }
  .summary-grid { display: grid; grid-template-columns: repeat(3,1fr); gap: 10px; margin: 16px 0; }
  .summary-box { border: 1.5px solid #E0E0E0; border-radius: 8px; padding: 12px 14px; text-align: center; }
  .summary-box .sb-label { font-size: 11px; color: #888; margin-bottom: 5px; }
  .summary-box .sb-val { font-size: 18px; font-weight: 700; }
  .balance-box { border: 2px solid #1F3864; border-radius: 8px; padding: 14px 18px; display: flex; justify-content: space-between; align-items: center; margin: 16px 0; background: #F0F4FF; }
  .balance-box .bl-label { font-size: 14px; font-weight: 600; color: #1F3864; }
  .balance-box .bl-val { font-size: 24px; font-weight: 700; color: ${bal>0?'#DC2626':'#059669'}; }
  .sig-section { display: grid; grid-template-columns: 1fr 1fr; gap: 30px; margin-top: 36px; padding-top: 20px; border-top: 1px dashed #CCC; }
  .sig-box { text-align: center; }
  .sig-line { border-top: 1.5px solid #1A1A1A; margin-top: 40px; padding-top: 6px; font-size: 12px; font-weight: 600; color: #555; }
  .footer { text-align: center; font-size: 10px; color: #AAA; padding: 10px 32px 20px; border-top: 1px solid #F0F0F0; margin-top: 10px; }
  .remarks-box { background: #FFFBEB; border: 1px solid #FDE68A; border-radius: 6px; padding: 10px 14px; margin: 12px 0; font-size: 12px; color: #92400E; }
</style>
</head>
<body>

<div class="print-bar no-print">
  <button class="btn-print" onclick="window.print()">&#x1F5A8; Print Now</button>
  <button class="btn-close" onclick="window.close()">&#x2715; Close</button>
  <span style="font-size:12px;color:#666;margin-left:6px">Use Ctrl+P / Cmd+P to print · Set paper size to A4</span>
</div>

<div class="page">
  <div class="voucher">

    <!-- Header -->
    <div class="top-header">
      <div>
        <div class="company-name">&#x1F9F5; Modrobe Impex</div>
        <div class="company-sub">Garment Manufacturer &amp; Exporter</div>
        <div class="company-sub" style="margin-top:2px">New Delhi, India</div>
      </div>
      <div class="voucher-title">
        <h2>JOB WORK VOUCHER</h2>
        <div class="lot-id">${esc(lot.id)}</div>
        <div style="margin-top:6px;display:inline-block;background:${statusBg};color:${statusColor};padding:3px 14px;border-radius:20px;font-size:12px;font-weight:700">${lot.status}</div>
      </div>
    </div>

    <!-- Lot Details -->
    <div style="font-weight:700;font-size:13px;color:#1F3864;margin-bottom:8px;text-transform:uppercase;letter-spacing:.5px">Lot Details</div>
    <div class="meta-grid">
      <div class="meta-label">Contractor Name</div><div class="meta-value" style="color:#1F3864;font-size:15px">${esc(lot.contractorName)}</div>
      <div class="meta-label">Item / Product</div><div class="meta-value">${esc(lot.item)}</div>
      <div class="meta-label">Date Issued (Out)</div><div class="meta-value">${fmtDate(lot.dateIssued)}</div>
      <div class="meta-label">Expected / In Date</div><div class="meta-value">${fmtDate(lot.dateReceived)}</div>
      <div class="meta-label">Pieces Issued</div><div class="meta-value">${fmtN(lot.piecesIssued)} pcs</div>
      <div class="meta-label">Rate per Piece</div><div class="meta-value">Rs. ${lot.rate} per piece</div>
    </div>

    ${lot.remarks ? `<div class="remarks-box"><strong>Remarks:</strong> ${esc(lot.remarks)}</div>` : ''}

    <!-- Financial Summary -->
    <div class="summary-grid">
      <div class="summary-box">
        <div class="sb-label">Total Agreed Amount</div>
        <div class="sb-val" style="color:#1F3864">${fmt(lot.piecesIssued*lot.rate)}</div>
      </div>
      <div class="summary-box">
        <div class="sb-label">Total Advance Paid</div>
        <div class="sb-val" style="color:#D97706">${fmt(adv)}</div>
      </div>
      <div class="summary-box">
        <div class="sb-label">Pieces ${lot.piecesReceived!=null?'Received':'(Issued)'}</div>
        <div class="sb-val">${fmtN(lot.piecesReceived!=null?lot.piecesReceived:lot.piecesIssued)} pcs</div>
      </div>
    </div>

    <div class="balance-box">
      <div class="bl-label">NET BALANCE ${bal>0?'PAYABLE TO CONTRACTOR':'(OVERPAID / SETTLED)'}</div>
      <div class="bl-val">${fmt(Math.abs(bal))}${bal<0?' (CR)':''}</div>
    </div>

    <!-- Advance Payment History -->
    <div class="section-title">ADVANCE / INTERIM PAYMENT HISTORY</div>
    <table style="width:100%;border-collapse:collapse;font-size:13px">
      <thead>
        <tr style="background:#E8EEF7">
          <th style="padding:9px 10px;text-align:left;border:1px solid #C8D5E8;font-size:11px;width:12%">Pay ID</th>
          <th style="padding:9px 10px;text-align:left;border:1px solid #C8D5E8;font-size:11px;width:18%">Date</th>
          <th style="padding:9px 10px;text-align:left;border:1px solid #C8D5E8;font-size:11px;width:18%">Amount</th>
          <th style="padding:9px 10px;text-align:left;border:1px solid #C8D5E8;font-size:11px;width:15%">Mode</th>
          <th style="padding:9px 10px;text-align:left;border:1px solid #C8D5E8;font-size:11px">Remarks</th>
        </tr>
      </thead>
      <tbody>${payRows}</tbody>
    </table>

    ${receivedSection}

    <!-- Signatures -->
    <div class="sig-section">
      <div class="sig-box">
        <div class="sig-line">Contractor's Signature<br><span style="font-weight:700;color:#1A1A1A">${esc(lot.contractorName)}</span></div>
      </div>
      <div class="sig-box">
        <div class="sig-line">Authorised Signatory<br><span style="font-weight:700;color:#1A1A1A">Modrobe Impex</span></div>
      </div>
    </div>

  </div>

  <div class="footer">
    This is a computer-generated job work voucher. &nbsp;&bull;&nbsp; Printed on: ${printedOn}
    &nbsp;&bull;&nbsp; ${esc(lot.id)} / ${esc(lot.contractorName)}
  </div>
</div>

</body>
</html>`;

  const w = window.open('','_blank','width=820,height=900,scrollbars=yes');
  w.document.write(html);
  w.document.close();
}

load();render();
</script>
</body>
</html>
