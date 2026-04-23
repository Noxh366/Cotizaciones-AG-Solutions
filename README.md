<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>AG Solutions — Cotizaciones</title>
<link href="https://fonts.googleapis.com/css2?family=Barlow:wght@400;500;600;700&family=Barlow+Condensed:wght@600;700&display=swap" rel="stylesheet">
<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
<style>
  *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

  :root {
    --blue: #1A42C0;
    --blue-dark: #122fa0;
    --blue-light: #e8edfb;
    --orange: #FC7723;
    --orange-dark: #e06010;
    --orange-light: #fff3ea;
    --white: #ffffff;
    --gray-50: #f7f8fc;
    --gray-100: #eef0f7;
    --gray-200: #dde1f0;
    --gray-400: #8b92b8;
    --gray-700: #2a2f50;
    --gray-900: #0e1126;
    --font: 'Barlow', sans-serif;
    --font-cond: 'Barlow Condensed', sans-serif;
    --radius: 8px;
    --radius-lg: 12px;
    --shadow: 0 2px 12px rgba(26,66,192,0.10);
    --shadow-lg: 0 8px 32px rgba(26,66,192,0.14);
  }

  body {
    font-family: var(--font);
    background: var(--gray-50);
    color: var(--gray-900);
    min-height: 100vh;
  }

  /* ── SIDEBAR ── */
  .layout { display: flex; min-height: 100vh; }

  .sidebar {
    width: 260px;
    background: var(--blue);
    color: var(--white);
    flex-shrink: 0;
    display: flex;
    flex-direction: column;
    position: fixed;
    top: 0; left: 0; bottom: 0;
    z-index: 100;
    overflow: hidden;
  }

  .sidebar-top {
    padding: 28px 20px 20px;
    border-bottom: 1px solid rgba(255,255,255,0.12);
  }

  .logo-area {
    display: flex;
    align-items: center;
    gap: 10px;
    margin-bottom: 4px;
  }

  .logo-box {
    width: 38px; height: 38px;
    background: var(--orange);
    border-radius: 8px;
    display: flex; align-items: center; justify-content: center;
    font-family: var(--font-cond);
    font-size: 20px; font-weight: 700;
    color: white;
    letter-spacing: -1px;
  }

  .logo-text {
    font-family: var(--font-cond);
    font-size: 17px;
    font-weight: 700;
    letter-spacing: 1.5px;
    text-transform: uppercase;
    line-height: 1.1;
    color: white;
  }

  .logo-sub {
    font-size: 10px;
    color: rgba(255,255,255,0.55);
    letter-spacing: 2px;
    text-transform: uppercase;
  }

  .sidebar-label {
    font-size: 10px;
    letter-spacing: 2px;
    text-transform: uppercase;
    color: rgba(255,255,255,0.45);
    padding: 18px 20px 8px;
    font-weight: 600;
  }

  .new-btn {
    margin: 0 16px 4px;
    background: var(--orange);
    color: white;
    border: none;
    border-radius: var(--radius);
    padding: 10px 14px;
    font-family: var(--font);
    font-size: 13px;
    font-weight: 600;
    cursor: pointer;
    display: flex; align-items: center; gap: 8px;
    transition: background 0.15s;
    width: calc(100% - 32px);
  }

  .new-btn:hover { background: var(--orange-dark); }

  .cotiz-list {
    flex: 1;
    overflow-y: auto;
    padding: 4px 0 20px;
  }

  .cotiz-item {
    padding: 10px 20px;
    cursor: pointer;
    border-left: 3px solid transparent;
    transition: all 0.15s;
    border-bottom: 1px solid rgba(255,255,255,0.07);
  }

  .cotiz-item:hover { background: rgba(255,255,255,0.08); }
  .cotiz-item.active { border-left-color: var(--orange); background: rgba(255,255,255,0.12); }

  .cotiz-item-folio {
    font-family: var(--font-cond);
    font-size: 13px;
    font-weight: 700;
    color: var(--orange);
    letter-spacing: 0.5px;
  }

  .cotiz-item-client {
    font-size: 13px;
    font-weight: 500;
    color: white;
    margin: 2px 0 1px;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
  }

  .cotiz-item-date {
    font-size: 11px;
    color: rgba(255,255,255,0.4);
  }

  .sidebar-empty {
    padding: 16px 20px;
    font-size: 12px;
    color: rgba(255,255,255,0.35);
    font-style: italic;
  }

  /* ── MAIN ── */
  .main {
    margin-left: 260px;
    flex: 1;
    display: flex;
    flex-direction: column;
  }

  .topbar {
    background: var(--white);
    border-bottom: 1px solid var(--gray-200);
    padding: 16px 32px;
    display: flex;
    align-items: center;
    justify-content: space-between;
    position: sticky;
    top: 0;
    z-index: 50;
  }

  .topbar-title {
    font-family: var(--font-cond);
    font-size: 20px;
    font-weight: 700;
    color: var(--blue);
    letter-spacing: 0.5px;
    text-transform: uppercase;
  }

  .topbar-actions { display: flex; gap: 10px; }

  .btn {
    padding: 8px 18px;
    border-radius: var(--radius);
    font-family: var(--font);
    font-size: 13px;
    font-weight: 600;
    cursor: pointer;
    border: none;
    transition: all 0.15s;
    display: flex; align-items: center; gap: 7px;
  }

  .btn-primary { background: var(--blue); color: white; }
  .btn-primary:hover { background: var(--blue-dark); }

  .btn-orange { background: var(--orange); color: white; }
  .btn-orange:hover { background: var(--orange-dark); }

  .btn-ghost { background: transparent; color: var(--gray-700); border: 1px solid var(--gray-200); }
  .btn-ghost:hover { background: var(--gray-100); }

  .btn-danger { background: transparent; color: #c0392b; border: 1px solid #fadbd8; }
  .btn-danger:hover { background: #fdedec; }

  /* ── CONTENT ── */
  .content { padding: 28px 32px; flex: 1; }

  /* ── EMPTY STATE ── */
  .empty-state {
    text-align: center;
    padding: 80px 32px;
    color: var(--gray-400);
  }

  .empty-icon {
    width: 64px; height: 64px;
    background: var(--blue-light);
    border-radius: 50%;
    display: flex; align-items: center; justify-content: center;
    margin: 0 auto 20px;
    font-size: 28px;
  }

  .empty-state h2 {
    font-family: var(--font-cond);
    font-size: 22px;
    font-weight: 700;
    color: var(--blue);
    margin-bottom: 8px;
  }

  .empty-state p { font-size: 14px; color: var(--gray-400); }

  /* ── FORM ── */
  .form-card {
    background: var(--white);
    border-radius: var(--radius-lg);
    border: 1px solid var(--gray-200);
    overflow: hidden;
    margin-bottom: 24px;
    box-shadow: var(--shadow);
  }

  .form-card-header {
    background: var(--blue);
    padding: 12px 20px;
    display: flex; align-items: center; gap: 10px;
  }

  .form-card-header.orange { background: var(--orange); }

  .form-card-title {
    font-family: var(--font-cond);
    font-size: 14px;
    font-weight: 700;
    letter-spacing: 2px;
    text-transform: uppercase;
    color: white;
  }

  .form-card-body { padding: 20px; }

  .form-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
    gap: 14px;
  }

  .form-group { display: flex; flex-direction: column; gap: 5px; }
  .form-group.full { grid-column: 1 / -1; }

  label {
    font-size: 11px;
    font-weight: 700;
    letter-spacing: 1.5px;
    text-transform: uppercase;
    color: var(--gray-400);
  }

  input, textarea, select {
    font-family: var(--font);
    font-size: 13px;
    color: var(--gray-900);
    background: var(--gray-50);
    border: 1.5px solid var(--gray-200);
    border-radius: var(--radius);
    padding: 8px 12px;
    transition: border-color 0.15s, background 0.15s;
    outline: none;
    width: 100%;
  }

  input:focus, textarea:focus, select:focus {
    border-color: var(--blue);
    background: white;
  }

  textarea { resize: vertical; min-height: 90px; line-height: 1.5; }

  /* ── ITEMS TABLE ── */
  .items-section { margin-bottom: 24px; }

  .items-header {
    display: flex;
    align-items: center;
    justify-content: space-between;
    margin-bottom: 12px;
  }

  .items-header h3 {
    font-family: var(--font-cond);
    font-size: 14px;
    font-weight: 700;
    letter-spacing: 2px;
    text-transform: uppercase;
    color: var(--blue);
  }

  .items-table { width: 100%; border-collapse: collapse; }

  .items-table th {
    font-size: 10px;
    font-weight: 700;
    letter-spacing: 1.5px;
    text-transform: uppercase;
    color: var(--gray-400);
    text-align: left;
    padding: 8px 10px;
    border-bottom: 2px solid var(--gray-200);
    background: var(--gray-50);
  }

  .items-table td {
    padding: 6px 8px;
    border-bottom: 1px solid var(--gray-100);
    vertical-align: middle;
  }

  .items-table td input {
    font-size: 13px;
    padding: 6px 8px;
  }

  .items-table td input.num {
    text-align: right;
  }

  .total-display {
    font-weight: 700;
    font-size: 13px;
    color: var(--gray-900);
    text-align: right;
    padding-right: 4px;
    min-width: 90px;
    display: inline-block;
  }

  .remove-row {
    background: none;
    border: none;
    color: #e74c3c;
    cursor: pointer;
    font-size: 16px;
    padding: 4px 6px;
    border-radius: 4px;
    line-height: 1;
  }

  .remove-row:hover { background: #fdedec; }

  /* ── TOTALS ── */
  .totals-box {
    background: var(--white);
    border: 1px solid var(--gray-200);
    border-radius: var(--radius-lg);
    overflow: hidden;
    box-shadow: var(--shadow);
    max-width: 340px;
    margin-left: auto;
    margin-bottom: 24px;
  }

  .totals-row {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 10px 16px;
    border-bottom: 1px solid var(--gray-100);
    font-size: 14px;
  }

  .totals-row:last-child { border-bottom: none; }

  .totals-row.total-final {
    background: var(--blue);
    color: white;
  }

  .totals-row.total-final .totals-label { color: rgba(255,255,255,0.75); font-size: 12px; letter-spacing: 1px; text-transform: uppercase; font-weight: 700; }
  .totals-row.total-final .totals-value { font-size: 20px; font-weight: 700; color: var(--orange); font-family: var(--font-cond); letter-spacing: 0.5px; }
  .totals-label { color: var(--gray-400); font-size: 12px; font-weight: 600; letter-spacing: 0.5px; text-transform: uppercase; }
  .totals-value { font-weight: 700; color: var(--gray-900); }



  /* Util */
  .flex { display: flex; }
  .gap-2 { gap: 8px; }
  .mt-4 { margin-top: 16px; }
  .section-divider { height: 1px; background: var(--gray-100); margin: 8px 0 20px; }
</style>
</head>
<body>

<div class="layout">

  <!-- SIDEBAR -->
  <aside class="sidebar">
    <div class="sidebar-top">
      <div class="logo-area">
        <div class="logo-box">AG</div>
        <div>
          <div class="logo-text">Solutions</div>
          <div class="logo-sub">Cotizaciones</div>
        </div>
      </div>
    </div>

    <div style="padding: 14px 16px 6px;">
      <button class="new-btn" onclick="newQuote()">
        <span style="font-size:18px;line-height:1">+</span> Nueva Cotización
      </button>
    </div>

    <div class="sidebar-label">Historial</div>

    <div class="cotiz-list" id="cotiz-list">
      <div class="sidebar-empty">Sin cotizaciones aún.</div>
    </div>
  </aside>

  <!-- MAIN -->
  <main class="main">
    <div class="topbar">
      <div class="topbar-title" id="topbar-title">Nueva Cotización</div>
      <div class="topbar-actions" id="topbar-actions">
        <button class="btn btn-ghost" onclick="clearForm()">Limpiar</button>
        <button class="btn btn-orange" onclick="exportPDF()">&#8681; Exportar PDF</button>
        <button class="btn btn-primary" onclick="saveQuote()">&#10003; Guardar</button>
      </div>
    </div>

    <div class="content" id="main-content">

      <!-- EMPTY STATE -->
      <div class="empty-state" id="empty-state" style="display:none;">
        <div class="empty-icon">📄</div>
        <h2>Sin cotización seleccionada</h2>
        <p>Crea una nueva cotización o selecciona una del historial.</p>
      </div>

      <!-- FORM -->
      <div id="form-wrapper">

        <!-- INFO HEADER -->
        <div class="form-card">
          <div class="form-card-header">
            <div class="form-card-title">&#9632; Información General</div>
          </div>
          <div class="form-card-body">
            <div class="form-grid">
              <div class="form-group">
                <label>Folio</label>
                <input type="text" id="f_folio" placeholder="Ej. B1004">
              </div>
              <div class="form-group">
                <label>Fecha</label>
                <input type="date" id="f_fecha">
              </div>
              <div class="form-group">
                <label>Técnico</label>
                <input type="text" id="f_tecnico" placeholder="Nombre del técnico">
              </div>
            </div>
          </div>
        </div>

        <!-- CLIENTE -->
        <div class="form-card">
          <div class="form-card-header">
            <div class="form-card-title">&#9632; Datos del Cliente</div>
          </div>
          <div class="form-card-body">
            <div class="form-grid">
              <div class="form-group">
                <label>Atención</label>
                <input type="text" id="f_atencion" placeholder="Contacto">
              </div>
              <div class="form-group">
                <label>Facturación</label>
                <input type="text" id="f_facturacion" placeholder="Razón social">
              </div>
              <div class="form-group">
                <label>Correo</label>
                <input type="email" id="f_correo" placeholder="correo@empresa.com">
              </div>
              <div class="form-group">
                <label>Teléfono</label>
                <input type="tel" id="f_telefono" placeholder="(81) 0000-0000">
              </div>
            </div>
          </div>
        </div>

        <!-- UNIDAD -->
        <div class="form-card">
          <div class="form-card-header orange">
            <div class="form-card-title">&#9632; Datos de la Unidad</div>
          </div>
          <div class="form-card-body">
            <div class="form-grid">
              <div class="form-group">
                <label>Marca</label>
                <input type="text" id="f_marca" placeholder="Toyota, Nissan...">
              </div>
              <div class="form-group">
                <label>Modelo</label>
                <input type="text" id="f_modelo" placeholder="Hilux, Urvan...">
              </div>
              <div class="form-group">
                <label>Kilometraje</label>
                <input type="text" id="f_km" placeholder="Km">
              </div>
              <div class="form-group">
                <label>No. de Serie</label>
                <input type="text" id="f_serie" placeholder="VIN / Serie">
              </div>
              <div class="form-group">
                <label>Placas</label>
                <input type="text" id="f_placas" placeholder="ABC-123">
              </div>
              <div class="form-group">
                <label>No. de Unidad</label>
                <input type="text" id="f_nounidad" placeholder="B1003">
              </div>
              <div class="form-group">
                <label>Color</label>
                <input type="text" id="f_color" placeholder="Blanco, Rojo...">
              </div>
              <div class="form-group">
                <label>Fecha de Evaluación</label>
                <input type="date" id="f_fechaeval">
              </div>
            </div>
          </div>
        </div>

        <!-- DIAGNÓSTICO -->
        <div class="form-card">
          <div class="form-card-header orange">
            <div class="form-card-title">&#9632; Diagnóstico</div>
          </div>
          <div class="form-card-body">
            <div class="form-group">
              <label>Evaluación Técnica</label>
              <textarea id="f_diagnostico" placeholder="Describe los hallazgos del diagnóstico, uno por línea.&#10;• No tenía bandas de alternador&#10;• Motor condensador dañado"></textarea>
            </div>
          </div>
        </div>

        <!-- SERVICIOS -->
        <div class="form-card">
          <div class="form-card-header">
            <div class="form-card-title">&#9632; Servicios y Refacciones</div>
          </div>
          <div class="form-card-body" style="padding-bottom:0;">

            <div class="items-section">
              <table class="items-table" style="margin-bottom:0;">
                <thead>
                  <tr>
                    <th style="width:50px;">Cant.</th>
                    <th style="width:100px;">Código</th>
                    <th>Descripción</th>
                    <th style="width:120px;text-align:right;">P. Unit.</th>
                    <th style="width:120px;text-align:right;">Total</th>
                    <th style="width:40px;"></th>
                  </tr>
                </thead>
                <tbody id="items-body"></tbody>
              </table>

              <div style="padding:10px 0 16px;">
                <button class="btn btn-ghost" onclick="addItem()" style="font-size:12px;padding:7px 14px;">
                  + Agregar línea
                </button>
              </div>
            </div>

          </div>
        </div>

        <!-- NOTAS -->
        <div class="form-card">
          <div class="form-card-header">
            <div class="form-card-title">&#9632; Notas y Advertencias</div>
          </div>
          <div class="form-card-body">
            <div class="form-group">
              <textarea id="f_notas" placeholder="Esta cotización puede cambiar al momento de realizar la reparación.&#10;No aplica garantía en piezas eléctricas." style="min-height:70px;"></textarea>
            </div>
          </div>
        </div>

        <!-- TOTALS -->
        <div class="totals-box">
          <div class="totals-row">
            <span class="totals-label">Subtotal</span>
            <span class="totals-value" id="disp-subtotal">$0.00</span>
          </div>
          <div class="totals-row">
            <span class="totals-label">IVA 16%</span>
            <span class="totals-value" id="disp-iva">$0.00</span>
          </div>
          <div class="totals-row total-final">
            <span class="totals-label">Total</span>
            <span class="totals-value" id="disp-total">$0.00</span>
          </div>
        </div>

      </div><!-- /form-wrapper -->

    </div><!-- /content -->
  </main>
</div>

<script>
let quotes = JSON.parse(localStorage.getItem('ag_quotes') || '[]');
let currentId = null;
let items = [];

function fmt(n) {
  return '$' + Number(n).toLocaleString('es-MX', {minimumFractionDigits:2,maximumFractionDigits:2});
}

function today() {
  return new Date().toISOString().split('T')[0];
}

function init() {
  document.getElementById('f_fecha').value = today();
  document.getElementById('f_fechaeval').value = today();
  document.getElementById('f_notas').value = 'Esta cotización puede cambiar al momento de realizar la reparación.\nNo aplica garantía en piezas eléctricas.';
  if (items.length === 0) addItem();
  renderSidebar();
}

function renderSidebar() {
  const list = document.getElementById('cotiz-list');
  if (quotes.length === 0) {
    list.innerHTML = '<div class="sidebar-empty">Sin cotizaciones aún.</div>';
    return;
  }
  list.innerHTML = quotes.slice().reverse().map(q => `
    <div class="cotiz-item ${q.id === currentId ? 'active' : ''}" onclick="loadQuote('${q.id}')">
      <div class="cotiz-item-folio">${q.folio || 'Sin folio'}</div>
      <div class="cotiz-item-client">${q.facturacion || q.atencion || 'Sin cliente'}</div>
      <div class="cotiz-item-date">${q.fecha || ''}</div>
    </div>
  `).join('');
}

function addItem(item = {}) {
  const id = Date.now() + Math.random();
  const obj = { id, cant: item.cant || 1, codigo: item.codigo || '', desc: item.desc || '', punit: item.punit || 0 };
  items.push(obj);
  const tbody = document.getElementById('items-body');
  const tr = document.createElement('tr');
  tr.id = 'item-' + id;
  tr.innerHTML = `
    <td><input type="number" class="num" min="1" value="${obj.cant}" onchange="updateItem('${id}','cant',this.value)" style="width:50px;"></td>
    <td><input type="text" value="${obj.codigo}" placeholder="—" onchange="updateItem('${id}','codigo',this.value)" style="width:90px;"></td>
    <td><input type="text" value="${obj.desc}" placeholder="Descripción del servicio o refacción" onchange="updateItem('${id}','desc',this.value)"></td>
    <td><input type="number" class="num" min="0" step="0.01" value="${obj.punit || ''}" placeholder="0.00" onchange="updateItem('${id}','punit',this.value)" style="width:110px;text-align:right;"></td>
    <td><span class="total-display" id="itot-${id}">${fmt(obj.cant * obj.punit)}</span></td>
    <td><button class="remove-row" onclick="removeItem('${id}')">✕</button></td>
  `;
  tbody.appendChild(tr);
  recalc();
}

function updateItem(id, field, val) {
  const item = items.find(i => i.id == id);
  if (!item) return;
  if (field === 'cant') item.cant = parseFloat(val) || 0;
  else if (field === 'punit') item.punit = parseFloat(val) || 0;
  else item[field] = val;
  const tot = document.getElementById('itot-' + id);
  if (tot) tot.textContent = fmt(item.cant * item.punit);
  recalc();
}

function removeItem(id) {
  items = items.filter(i => i.id != id);
  const tr = document.getElementById('item-' + id);
  if (tr) tr.remove();
  recalc();
}

function recalc() {
  const sub = items.reduce((a, i) => a + (i.cant * i.punit), 0);
  const iva = sub * 0.16;
  const total = sub + iva;
  document.getElementById('disp-subtotal').textContent = fmt(sub);
  document.getElementById('disp-iva').textContent = fmt(iva);
  document.getElementById('disp-total').textContent = fmt(total);
}

function getFormData() {
  return {
    id: currentId || String(Date.now()),
    folio: document.getElementById('f_folio').value,
    fecha: document.getElementById('f_fecha').value,
    tecnico: document.getElementById('f_tecnico').value,
    atencion: document.getElementById('f_atencion').value,
    facturacion: document.getElementById('f_facturacion').value,
    correo: document.getElementById('f_correo').value,
    telefono: document.getElementById('f_telefono').value,
    marca: document.getElementById('f_marca').value,
    modelo: document.getElementById('f_modelo').value,
    km: document.getElementById('f_km').value,
    serie: document.getElementById('f_serie').value,
    placas: document.getElementById('f_placas').value,
    nounidad: document.getElementById('f_nounidad').value,
    color: document.getElementById('f_color').value,
    fechaeval: document.getElementById('f_fechaeval').value,
    diagnostico: document.getElementById('f_diagnostico').value,
    notas: document.getElementById('f_notas').value,
    items: items.map(i => ({ cant: i.cant, codigo: i.codigo, desc: i.desc, punit: i.punit }))
  };
}

function setFormData(q) {
  document.getElementById('f_folio').value = q.folio || '';
  document.getElementById('f_fecha').value = q.fecha || today();
  document.getElementById('f_tecnico').value = q.tecnico || '';
  document.getElementById('f_atencion').value = q.atencion || '';
  document.getElementById('f_facturacion').value = q.facturacion || '';
  document.getElementById('f_correo').value = q.correo || '';
  document.getElementById('f_telefono').value = q.telefono || '';
  document.getElementById('f_marca').value = q.marca || '';
  document.getElementById('f_modelo').value = q.modelo || '';
  document.getElementById('f_km').value = q.km || '';
  document.getElementById('f_serie').value = q.serie || '';
  document.getElementById('f_placas').value = q.placas || '';
  document.getElementById('f_nounidad').value = q.nounidad || '';
  document.getElementById('f_color').value = q.color || '';
  document.getElementById('f_fechaeval').value = q.fechaeval || today();
  document.getElementById('f_diagnostico').value = q.diagnostico || '';
  document.getElementById('f_notas').value = q.notas || '';

  items = [];
  document.getElementById('items-body').innerHTML = '';
  (q.items || []).forEach(it => addItem(it));
  if (items.length === 0) addItem();
}

function saveQuote() {
  const data = getFormData();
  if (!currentId) {
    currentId = data.id;
  }
  data.id = currentId;
  const idx = quotes.findIndex(q => q.id === currentId);
  if (idx >= 0) quotes[idx] = data;
  else quotes.push(data);
  localStorage.setItem('ag_quotes', JSON.stringify(quotes));
  document.getElementById('topbar-title').textContent = data.folio ? 'Cotización ' + data.folio : 'Nueva Cotización';
  renderSidebar();
  showToast('Cotización guardada');
}

function loadQuote(id) {
  const q = quotes.find(x => x.id === id);
  if (!q) return;
  currentId = id;
  setFormData(q);
  document.getElementById('topbar-title').textContent = q.folio ? 'Cotización ' + q.folio : 'Cotización';
  renderSidebar();
  document.getElementById('form-wrapper').style.display = '';
  document.getElementById('empty-state').style.display = 'none';
}

function newQuote() {
  currentId = null;
  items = [];
  document.getElementById('items-body').innerHTML = '';
  document.getElementById('f_folio').value = '';
  document.getElementById('f_fecha').value = today();
  document.getElementById('f_tecnico').value = '';
  document.getElementById('f_atencion').value = '';
  document.getElementById('f_facturacion').value = '';
  document.getElementById('f_correo').value = '';
  document.getElementById('f_telefono').value = '';
  document.getElementById('f_marca').value = '';
  document.getElementById('f_modelo').value = '';
  document.getElementById('f_km').value = '';
  document.getElementById('f_serie').value = '';
  document.getElementById('f_placas').value = '';
  document.getElementById('f_nounidad').value = '';
  document.getElementById('f_color').value = '';
  document.getElementById('f_fechaeval').value = today();
  document.getElementById('f_diagnostico').value = '';
  document.getElementById('f_notas').value = 'Esta cotización puede cambiar al momento de realizar la reparación.\nNo aplica garantía en piezas eléctricas.';
  addItem();
  recalc();
  document.getElementById('topbar-title').textContent = 'Nueva Cotización';
  renderSidebar();
  document.getElementById('form-wrapper').style.display = '';
  document.getElementById('empty-state').style.display = 'none';
}

function clearForm() {
  if (!confirm('¿Limpiar todos los campos?')) return;
  newQuote();
}

function formatFecha(d) {
  if (!d) return '';
  const [y,m,day] = d.split('-');
  return `${day} / ${m} / ${y}`;
}

// ── jsPDF pixel-perfect export ──────────────────────────────────────────────

function hexToRgb(hex) {
  const r = parseInt(hex.slice(0,2),16);
  const g = parseInt(hex.slice(2,4),16);
  const b = parseInt(hex.slice(4,6),16);
  return [r,g,b];
}

function exportPDF() {
  const q = getFormData();
  const { jsPDF } = window.jspdf;
  const doc = new jsPDF({ orientation:'portrait', unit:'mm', format:'letter' });

  const W = 215.9, H = 279.4;
  const ML = 15, MR = W - 15; // margins
  const CW = MR - ML; // content width = 185.9

  const BLUE = '1A42C0', ORANGE = 'FC7723', WHITE = 'FFFFFF';
  const LGRAY = 'F4F5FA', MGRAY = 'E2E5F0', DGRAY = '444455';
  const BLUE_BG = 'EEF1FB', ORANGE_BG = 'FFF0E0';

  function setFill(hex) { const [r,g,b]=hexToRgb(hex); doc.setFillColor(r,g,b); }
  function setDraw(hex) { const [r,g,b]=hexToRgb(hex); doc.setDrawColor(r,g,b); }
  function setTxt(hex)  { const [r,g,b]=hexToRgb(hex); doc.setTextColor(r,g,b); }

  function rect(x,y,w,h,fill,stroke){
    if(fill){ setFill(fill); }
    if(stroke){ setDraw(stroke); doc.setLineWidth(0.2); }
    if(fill && stroke) doc.rect(x,y,w,h,'FD');
    else if(fill) doc.rect(x,y,w,h,'F');
    else if(stroke) doc.rect(x,y,w,h,'S');
  }

  function txt(text, x, y, opts={}){
    const { size=9, bold=false, color='333333', align='left', maxW } = opts;
    doc.setFontSize(size);
    doc.setFont('helvetica', bold?'bold':'normal');
    setTxt(color);
    const options = {};
    if(align==='right') options.align='right';
    if(align==='center') options.align='center';
    if(maxW) options.maxWidth = maxW;
    doc.text(String(text||''), x, y, options);
  }

  function line(x1,y1,x2,y2,hex='CCCCCC',lw=0.2){
    setDraw(hex); doc.setLineWidth(lw);
    doc.line(x1,y1,x2,y2);
  }

  let y = 0;

  // ── TOP STRIPE ───────────────────────────────────────────────────────────
  rect(0, 0, W*0.70, 6, BLUE);
  rect(W*0.70, 0, W*0.30, 6, ORANGE);
  y = 6;

  // ── HEADER ───────────────────────────────────────────────────────────────
  // Logo box (orange square with "AG")
  const logoX = ML, logoY = y + 5;
  rect(logoX, logoY, 18, 18, ORANGE);
  txt('AG', logoX+9, logoY+13, { size:13, bold:true, color:WHITE, align:'center' });

  // "SOLUTIONS" text
  txt('SOLUTIONS', logoX+21, logoY+8, { size:12, bold:true, color:BLUE });
  txt('Servicio Automotriz Profesional', logoX+21, logoY+14, { size:7, color:'888888' });

  // Folio box (right side)
  const fbX = W - ML - 65, fbY = y + 3, fbW = 65, fbH = 26;
  rect(fbX, fbY, fbW, fbH, BLUE);

  // Title row inside folio box
  txt('C O T I Z A C I Ó N', fbX + fbW/2, fbY + 5.5, { size:8.5, bold:true, color:WHITE, align:'center' });

  // Divider lines inside folio box
  const rowH = (fbH - 7) / 3;
  ['FECHA','FOLIO','TÉCNICO'].forEach((key, i) => {
    const ry = fbY + 7 + i * rowH;
    // separator line
    line(fbX, ry, fbX+fbW, ry, 'FFFFFF', 0.15);
    // key cell
    rect(fbX, ry, 22, rowH, BLUE);
    txt(key, fbX+11, ry + rowH*0.65, { size:6.5, bold:true, color:'AAAACC', align:'center' });
    // value cell
    rect(fbX+22, ry, fbW-22, rowH, WHITE);
    const values = [formatFecha(q.fecha), q.folio||'—', q.tecnico||'—'];
    const valColor = i===1 ? ORANGE : '222222';
    const valBold = i===1;
    txt(values[i], fbX+24, ry + rowH*0.65, { size:7.5, bold:valBold, color:valColor });
  });

  y = logoY + 20 + 3;

  // ── ACCENT STRIPE 2 ──────────────────────────────────────────────────────
  rect(ML, y, CW*0.70, 3, BLUE);
  rect(ML + CW*0.70, y, CW*0.30, 3, ORANGE);
  y += 6;

  // ── INTRO TEXT ───────────────────────────────────────────────────────────
  txt('Estimado cliente,', ML, y, { size:8, bold:false, color:'555555' });
  doc.setFont('helvetica','italic'); doc.setFontSize(8); setTxt('555555');
  y += 4;
  const introText = `A continuación presentamos la cotización formal por los servicios de diagnóstico y reparación de la unidad correspondiente al folio ${q.folio||'—'}. Los trabajos fueron evaluados por nuestro técnico especialista.`;
  const introLines = doc.splitTextToSize(introText, CW);
  doc.setFont('helvetica','normal'); doc.setFontSize(8); setTxt('333333');
  doc.text(introLines, ML, y);
  y += introLines.length * 4 + 4;

  // ── DATOS DEL CLIENTE ────────────────────────────────────────────────────
  const secH = 5.5;
  rect(ML, y, CW, secH, BLUE);
  txt('DATOS DEL CLIENTE', ML + CW/2, y + 3.8, { size:8, bold:true, color:WHITE, align:'center' });
  y += secH;

  const clientRows = [
    [['ATENCIÓN:', q.atencion||''], ['FACTURACIÓN:', q.facturacion||'']],
    [['CORREO:', q.correo||''],     ['TELÉFONO:', q.telefono||'']]
  ];
  const colW = CW/2;
  clientRows.forEach((row, ri) => {
    const rh = 5.5;
    const ry = y;
    rect(ML, ry, CW, rh, ri%2===0 ? BLUE_BG : WHITE, MGRAY);
    row.forEach((cell, ci) => {
      const cx = ML + ci * colW;
      rect(cx, ry, colW, rh, null, MGRAY);
      txt(cell[0], cx+2, ry+3.8, { size:7, bold:true, color:BLUE });
      txt(cell[1], cx+2+doc.getTextWidth(cell[0])+1.5, ry+3.8, { size:7, color:'222222' });
    });
    y += rh;
  });
  y += 3;

  // ── DATOS DE LA UNIDAD ───────────────────────────────────────────────────
  rect(ML, y, CW*0.60, secH, ORANGE);
  txt('DATOS DE LA UNIDAD', ML + CW*0.30, y + 3.8, { size:8, bold:true, color:WHITE, align:'center' });
  y += secH;

  const unitRows = [
    [['Marca:', q.marca||''],   ['Modelo:', q.modelo||''],   ['Kilometraje:', q.km||'']],
    [['Serie:', q.serie||''],   ['Placas:', q.placas||''],   ['No. de Unidad:', q.nounidad||'']],
    [['Color:', q.color||''],   ['Técnico:', q.tecnico||''], ['Fecha eval.:', formatFecha(q.fechaeval)]]
  ];
  const ucW = CW/3;
  unitRows.forEach((row, ri) => {
    const rh = 5.5;
    const ry = y;
    row.forEach((cell, ci) => {
      const cx = ML + ci * ucW;
      rect(cx, ry, ucW, rh, ri%2===0 ? ORANGE_BG : WHITE, MGRAY);
      txt(cell[0], cx+2, ry+3.8, { size:7, bold:true, color:ORANGE });
      txt(cell[1], cx+2+doc.getTextWidth(cell[0])+1.5, ry+3.8, { size:7, color:'222222' });
    });
    y += rh;
  });
  y += 3;

  // ── DIAGNÓSTICO ──────────────────────────────────────────────────────────
  if (q.diagnostico && q.diagnostico.trim()) {
    const diagLines = (q.diagnostico||'').split('\n').filter(l=>l.trim()).map(l=>'• '+l.replace(/^[•\-\*]\s*/,''));
    const diagText = diagLines.join('\n');
    const diagTextLines = doc.splitTextToSize(diagText, CW - 38);
    const diagH = Math.max(16, diagTextLines.length * 4 + 6);

    // Orange label box
    rect(ML, y, 35, diagH, ORANGE);
    txt('DIAGNÓSTICO', ML+17.5, y+diagH/2-1, { size:8, bold:true, color:WHITE, align:'center' });
    txt('Evaluación técnica', ML+17.5, y+diagH/2+3, { size:6.5, color:'FFE0C0', align:'center' });
    // Content box
    rect(ML+35, y, CW-35, diagH, ORANGE_BG);
    doc.setFont('helvetica','normal'); doc.setFontSize(7.5); setTxt('333333');
    doc.text(diagTextLines, ML+37, y+5);
    y += diagH + 3;
  }

  // ── SERVICIOS HEADER ─────────────────────────────────────────────────────
  rect(ML, y, CW*0.65, 6, BLUE_BG, MGRAY);
  txt('Servicios y Refacciones', ML+2, y+4, { size:8.5, bold:true, color:BLUE });

  doc.setFont('helvetica','italic'); doc.setFontSize(7); setTxt('888888');
  doc.text('Detalle de trabajos a realizar y materiales requeridos.', ML+2, y+7.5);

  // Folio badge
  const fbadgeW = 34, fbadgeX = MR - fbadgeW;
  rect(fbadgeX, y, fbadgeW, 9, ORANGE);
  txt('Folio  '+( q.folio||'—'), fbadgeX + fbadgeW/2, y+5.8, { size:9, bold:true, color:WHITE, align:'center' });
  y += 10;

  // ── ITEMS TABLE ──────────────────────────────────────────────────────────
  const cols = { cant:14, cod:22, desc:0, punit:28, total:28 };
  cols.desc = CW - cols.cant - cols.cod - cols.punit - cols.total;
  const tHeaderH = 6;

  // Table header
  rect(ML, y, CW, tHeaderH, BLUE);
  let cx = ML;
  [['CANT.',cols.cant,'center'],['CÓDIGO',cols.cod,'left'],['DESCRIPCIÓN',cols.desc,'left'],['P. UNIT.',cols.punit,'right'],['TOTAL',cols.total,'right']].forEach(([label,w,al])=>{
    const tx = al==='right' ? cx+w-2 : al==='center' ? cx+w/2 : cx+2;
    txt(label, tx, y+4.2, { size:6.5, bold:true, color:WHITE, align:al });
    cx += w;
  });
  y += tHeaderH;

  const sub = (q.items||[]).reduce((a,i)=>a+(i.cant*i.punit),0);
  const iva = sub*0.16, total = sub+iva;

  (q.items||[]).forEach((item,idx)=>{
    const rh = 6;
    const rowBg = idx%2===0 ? WHITE : LGRAY;
    rect(ML, y, CW, rh, rowBg, MGRAY);

    let cx2 = ML;
    // Cant
    txt(String(item.cant), cx2+cols.cant/2, y+4, { size:8, color:'222222', align:'center' });
    cx2 += cols.cant;
    line(cx2, y, cx2, y+rh, MGRAY);
    // Codigo
    txt(item.codigo||'—', cx2+2, y+4, { size:7.5, color:'222222' });
    cx2 += cols.cod;
    line(cx2, y, cx2, y+rh, MGRAY);
    // Desc
    const descLines = doc.splitTextToSize(item.desc||'', cols.desc-4);
    doc.setFont('helvetica','normal'); doc.setFontSize(7.5); setTxt('222222');
    doc.text(descLines[0]||'', cx2+2, y+4);
    cx2 += cols.desc;
    line(cx2, y, cx2, y+rh, MGRAY);
    // P.Unit
    txt(fmt(item.punit), cx2+cols.punit-2, y+4, { size:7.5, color:'222222', align:'right' });
    cx2 += cols.punit;
    line(cx2, y, cx2, y+rh, MGRAY);
    // Total
    txt(fmt(item.cant*item.punit), cx2+cols.total-2, y+4, { size:7.5, bold:true, color:'222222', align:'right' });

    y += rh;
  });

  // Bottom border of table
  line(ML, y, MR, y, MGRAY);
  y += 4;

  // ── TOTALS ───────────────────────────────────────────────────────────────
  const totW = 70, totX = MR - totW;
  const totRows = [['Subtotal', fmt(sub)], ['IVA 16%', fmt(iva)]];
  totRows.forEach(([label,val],i)=>{
    rect(totX, y, totW, 5.5, i===0?LGRAY:WHITE, MGRAY);
    txt(label, totX+2, y+3.8, { size:7, color:'666666' });
    txt(val, totX+totW-2, y+3.8, { size:7.5, bold:true, color:'222222', align:'right' });
    y += 5.5;
  });
  // Total row - blue bg
  rect(totX, y, totW, 7, BLUE);
  txt('TOTAL', totX+2, y+4.5, { size:7, bold:true, color:'AAAACC' });
  txt(fmt(total), totX+totW-2, y+4.8, { size:10, bold:true, color:ORANGE, align:'right' });
  y += 9;

  // ── NOTAS ────────────────────────────────────────────────────────────────
  if (q.notas && q.notas.trim()) {
    const notaLines = (q.notas||'').split('\n').filter(l=>l.trim());
    notaLines.forEach(nota => {
      rect(ML, y, 2, 4, ORANGE);
      const clean = nota.replace(/^[!•\-\*]\s*/,'');
      txt('! '+clean, ML+4, y+3, { size:7, color:'555555' });
      y += 5;
    });
    y += 2;
  }

  // ── TÉRMINOS Y CONDICIONES ────────────────────────────────────────────────
  txt('TÉRMINOS Y CONDICIONES', ML, y, { size:8, bold:true, color:BLUE });
  line(ML, y+1, MR, y+1, BLUE, 0.4);
  y += 5;

  const terms = [
    ['01','Condiciones de pago','Se requiere un anticipo del 50% para iniciar los trabajos. El saldo restante se liquida contra entrega del vehículo con los trabajos concluidos.'],
    ['02','Vigencia de la cotización','La presente cotización tiene una vigencia de 15 días naturales a partir de la fecha de emisión.'],
    ['03','Garantía en mano de obra','Se otorga garantía de 30 días en mano de obra. No aplica garantía en piezas eléctricas ni en refacciones proporcionadas por el cliente.']
  ];

  terms.forEach(([num,title,body])=>{
    // Number circle
    setFill(BLUE); doc.circle(ML+3, y+2, 3, 'F');
    txt(num, ML+3, y+3, { size:6, bold:true, color:WHITE, align:'center' });
    // Title + body
    txt(title, ML+8, y+1.5, { size:7.5, bold:true, color:'111111' });
    const bodyLines = doc.splitTextToSize(body, CW-8);
    doc.setFont('helvetica','normal'); doc.setFontSize(7); setTxt('444444');
    doc.text(bodyLines, ML+8, y+5.5);
    y += 5 + bodyLines.length*3.5 + 2;
  });

  y += 2;

  // ── FIRMAS ───────────────────────────────────────────────────────────────
  const sigW = (CW - 20) / 2;
  // Left sig
  line(ML, y, ML+sigW, y, '999999', 0.4);
  txt('Firma del Cliente', ML + sigW/2, y+4, { size:7, color:'666666', align:'center' });
  // Right sig
  const sigX2 = MR - sigW;
  line(sigX2, y, MR, y, '999999', 0.4);
  txt('AG Solutions — Autorización', sigX2 + sigW/2, y+4, { size:7, color:'666666', align:'center' });
  y += 12;

  // ── FOOTER ───────────────────────────────────────────────────────────────
  rect(0, y, W, 10, BLUE);
  txt('AG Solutions  ·  Servicio Automotriz Profesional  ·  cotizaciones@agsolutions.mx', W/2, y+6, { size:7.5, color:'AABBDD', align:'center' });

  // Save
  const filename = `Cotizacion_${(q.folio||'SinFolio').replace(/\s/g,'_')}.pdf`;
  doc.save(filename);
}


function showToast(msg) {
  const t = document.createElement('div');
  t.textContent = msg;
  Object.assign(t.style, {
    position: 'fixed', bottom: '24px', right: '24px',
    background: '#1A42C0', color: 'white',
    padding: '10px 20px', borderRadius: '8px',
    fontSize: '13px', fontFamily: 'Barlow, sans-serif',
    fontWeight: '600', zIndex: '9999',
    boxShadow: '0 4px 16px rgba(26,66,192,0.3)',
    transition: 'opacity 0.4s'
  });
  document.body.appendChild(t);
  setTimeout(() => { t.style.opacity = '0'; setTimeout(() => t.remove(), 400); }, 2000);
}

init();
</script>
</body>
</html>
