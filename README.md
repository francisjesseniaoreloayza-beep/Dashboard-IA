[index.html](https://github.com/user-attachments/files/33130418/index.html)
<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<title>Dashboard IA 2026 — Asistencia y Rentabilidad</title>
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<style>
  * { box-sizing: border-box; margin: 0; padding: 0; }
  body { font-family: 'Segoe UI', Roboto, sans-serif; background: #f4f6fa; color: #1f2d3d; padding: 20px; }
  header {
    display: flex; justify-content: space-between; align-items: center;
    background: linear-gradient(135deg, #1e3a8a, #3b82f6);
    color: white; padding: 18px 24px; border-radius: 14px; margin-bottom: 20px;
    box-shadow: 0 4px 14px rgba(0,0,0,0.1);
  }
  header h1 { font-size: 20px; font-weight: 600; }
  .acciones { display: flex; gap: 10px; }
  .btn {
    background: rgba(255,255,255,0.15); color: white;
    border: 1px solid rgba(255,255,255,0.3); padding: 8px 14px;
    border-radius: 8px; cursor: pointer; font-size: 13px; transition: all 0.2s;
  }
  .btn:hover { background: rgba(255,255,255,0.3); transform: translateY(-1px); }
  .filtros {
    display: grid; grid-template-columns: repeat(auto-fit, minmax(180px, 1fr));
    gap: 12px; margin-bottom: 20px; background: white; padding: 16px;
    border-radius: 12px; box-shadow: 0 2px 8px rgba(0,0,0,0.05);
  }
  .filtros label { display: block; font-size: 12px; color: #64748b; margin-bottom: 4px; font-weight: 600; }
  .filtros select, .filtros input {
    width: 100%; padding: 8px 10px; border: 1px solid #e2e8f0;
    border-radius: 8px; font-size: 13px; background: #f8fafc;
    transition: all 0.2s;
  }
  .filtros select:disabled {
    background: #f1f5f9; color: #94a3b8;
    cursor: not-allowed; opacity: 0.7;
  }
  .filtros select.destacado {
    border: 2px solid #3b82f6;
    background: #eff6ff;
    font-weight: 700;
    color: #1e40af;
  }
  .filtros select.autocompletado {
    border: 2px solid #22c55e;
    background: #f0fdf4;
    font-weight: 700;
    color: #166534;
  }
  .filtros small.helper {
    display: block;
    font-size: 10px;
    color: #22c55e;
    margin-top: 3px;
    font-weight: 600;
  }
  .kpis { display: grid; grid-template-columns: repeat(auto-fit, minmax(180px, 1fr)); gap: 14px; margin-bottom: 20px; }
  .kpi {
    background: white; padding: 16px; border-radius: 12px;
    box-shadow: 0 2px 8px rgba(0,0,0,0.05); border-left: 5px solid #3b82f6;
    transition: transform 0.2s;
  }
  .kpi:hover { transform: translateY(-3px); box-shadow: 0 6px 16px rgba(0,0,0,0.1); }
  .kpi .titulo { font-size: 12px; color: #64748b; font-weight: 600; text-transform: uppercase; }
  .kpi .valor { font-size: 24px; font-weight: 700; color: #1e293b; margin-top: 6px; }
  .kpi .sub { font-size: 11px; color: #94a3b8; margin-top: 4px; }
  .kpi.verde { border-left-color: #22c55e; }
  .kpi.amarillo { border-left-color: #eab308; }
  .kpi.rojo { border-left-color: #ef4444; }
  .kpi.azul { border-left-color: #3b82f6; }
  .kpi.gris { border-left-color: #94a3b8; }
  .estado-banner {
    padding: 16px 24px; border-radius: 12px; font-size: 18px;
    font-weight: 700; text-align: center; margin-bottom: 20px; color: white;
  }
  .estado-verde { background: linear-gradient(135deg, #16a34a, #22c55e); }
  .estado-amarillo { background: linear-gradient(135deg, #ca8a04, #eab308); }
  .estado-rojo { background: linear-gradient(135deg, #dc2626, #ef4444); }
  .estado-info { background: linear-gradient(135deg, #475569, #64748b); }
  .grid-graficos { display: grid; grid-template-columns: repeat(auto-fit, minmax(400px, 1fr)); gap: 20px; margin-bottom: 20px; }
  .card { background: white; padding: 18px; border-radius: 12px; box-shadow: 0 2px 8px rgba(0,0,0,0.05); }
  .card h3 {
    font-size: 14px; color: #334155; margin-bottom: 12px;
    font-weight: 600; border-bottom: 2px solid #f1f5f9; padding-bottom: 8px;
  }
  table { width: 100%; border-collapse: collapse; font-size: 13px; }
  th {
    background: #f1f5f9; color: #475569; padding: 10px; text-align: left;
    font-weight: 600; font-size: 12px; text-transform: uppercase;
  }
  td { padding: 9px 10px; border-bottom: 1px solid #f1f5f9; }
  tr:hover td { background: #f8fafc; }
  .badge { padding: 3px 10px; border-radius: 12px; font-size: 11px; font-weight: 700; display:inline-block; }
  .badge.verde { background: #dcfce7; color: #166534; }
  .badge.amarillo { background: #fef9c3; color: #854d0e; }
  .badge.rojo { background: #fee2e2; color: #991b1b; }
  .badge.gris { background: #f1f5f9; color: #475569; }
  .badge.azul { background: #dbeafe; color: #1e40af; }
  .fila-rentab { display: flex; align-items: center; gap: 10px; margin-bottom: 10px; font-size: 13px; }
  .fila-rentab .nombre { width: 90px; font-weight: 600; }
  .fila-rentab .barra-cont { flex: 1; height: 18px; background: #f1f5f9; border-radius: 9px; overflow: hidden; }
  .fila-rentab .barra-fill { height: 100%; border-radius: 9px; transition: width 0.5s; }
  .fila-rentab .pct { width: 70px; text-align: right; font-weight: 700; }
  .info-box {
    background: #eff6ff; border-left: 4px solid #3b82f6;
    padding: 12px 16px; border-radius: 8px; margin-bottom: 20px;
    font-size: 13px; color: #1e40af;
  }
  .aviso-box {
    background: #fef3c7; border-left: 4px solid #f59e0b;
    padding: 12px 16px; border-radius: 8px; margin-bottom: 20px;
    font-size: 13px; color: #78350f;
  }
  .fila-sin-datos td { opacity: 0.55; font-style: italic; }

  .panel-fechas {
    background: white; padding: 16px; border-radius: 12px;
    box-shadow: 0 2px 8px rgba(0,0,0,0.05); margin-bottom: 20px;
    border-left: 5px solid #3b82f6;
  }
  .panel-fechas h3 {
    font-size: 14px; color: #334155; margin-bottom: 12px; font-weight: 600;
  }
  .fechas-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(180px, 1fr));
    gap: 10px;
  }
  .fecha-card {
    background: #f8fafc; border: 2px solid #e2e8f0;
    border-radius: 10px; padding: 12px;
    cursor: pointer; transition: all 0.2s;
    text-align: center;
  }
  .fecha-card:hover { border-color: #3b82f6; transform: translateY(-2px); }
  .fecha-card.activa { border-color: #3b82f6; background: #eff6ff; }
  .fecha-card .fd { font-size: 12px; color: #64748b; font-weight: 600; }
  .fecha-card .fc { font-size: 22px; font-weight: 700; color: #1e293b; margin: 4px 0; }
  .fecha-card .fp { font-size: 11px; font-weight: 700; padding: 2px 8px; border-radius: 8px; display: inline-block; }
  .fecha-card .fp.verde { background: #dcfce7; color: #166534; }
  .fecha-card .fp.amarillo { background: #fef9c3; color: #854d0e; }
  .fecha-card .fp.rojo { background: #fee2e2; color: #991b1b; }

  .ficha-grupo {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
    gap: 10px;
    background: #f8fafc;
    padding: 12px;
    border-radius: 8px;
    margin-bottom: 14px;
    font-size: 12px;
  }
  .ficha-grupo > div { padding: 6px 8px; }
  .ficha-grupo .lbl { color: #64748b; font-weight: 600; font-size: 11px; text-transform: uppercase; }
  .ficha-grupo .val { color: #1e293b; font-weight: 700; font-size: 13px; margin-top: 2px; }
</style>
</head>
<body>

<header>
  <h1>📊 Dashboard IA 2026 — Asistencia y Rentabilidad</h1>
  <div class="acciones">
    <button class="btn" onclick="renderizar()">🔄 Actualizar</button>
    <button class="btn" onclick="exportarCSV()">📥 Exportar CSV</button>
  </div>
</header>

<div class="info-box">
  💡 <b>Precio del curso:</b> S/ 299 por alumno · <b>Tarifas docentes:</b> Luis Romero S/60/h · Javier Morán S/50/h · Maria Solis S/50/h · Wilton Torvisco S/50/h · Johan Jara S/50/h · Ricardo Vargas S/60/h
</div>

<div class="aviso-box">
  ⚠️ <b>Cómo funciona:</b> Selecciona un grupo → los demás filtros (Docente, Fecha, Estado, Turno) se <b>autocompletan y bloquean</b> con los datos reales de ese grupo. Para volver a explorar libremente, elige <b>"Todos los grupos"</b>.
</div>

<!-- FILTROS -->
<div class="filtros">
  <div>
    <label>🏷️ Código de Grupo</label>
    <select id="filtroGrupo" onchange="cambioGrupo()"></select>
    <small class="helper" id="helperGrupo"></small>
  </div>
  <div>
    <label>👨‍🏫 Docente</label>
    <select id="filtroDocente" onchange="renderizar()"></select>
    <small class="helper" id="helperDocente"></small>
  </div>
  <div>
    <label>📅 Fecha de clase</label>
    <select id="filtroFecha" onchange="renderizar()" disabled>
      <option value="">— Selecciona un grupo primero —</option>
    </select>
    <small class="helper" id="helperFecha"></small>
  </div>
  <div>
    <label>🚦 Estado</label>
    <select id="filtroEstado" onchange="renderizar()">
      <option value="EN CURSO" selected>En curso (con datos)</option>
      <option value="">Todos</option>
      <option value="SIN INICIAR">Sin iniciar</option>
    </select>
  </div>
  <div>
    <label>🕒 Turno</label>
    <select id="filtroTurno" onchange="renderizar()">
      <option value="">Todos</option>
      <option value="Mañana">Mañana</option>
      <option value="Tarde">Tarde</option>
      <option value="Noche">Noche</option>
    </select>
  </div>
</div>

<!-- FICHA DEL GRUPO SELECCIONADO -->
<div class="card" id="fichaGrupo" style="display:none; margin-bottom:20px;">
  <h3>📌 Ficha del grupo <span id="fichaCodigo" style="color:#3b82f6;"></span></h3>
  <div class="ficha-grupo" id="fichaContenido"></div>
</div>

<!-- PANEL DE FECHAS -->
<div class="panel-fechas" id="panelFechas" style="display:none;">
  <h3>📅 Fechas dictadas — clic para filtrar una fecha específica</h3>
  <div class="fechas-grid" id="fechasGrid"></div>
</div>

<div id="bannerEstado" class="estado-banner estado-info">📊 Analizando...</div>

<div class="kpis" id="kpis"></div>

<div class="grid-graficos">
  <div class="card">
    <h3>📈 Alumnos Conectados por Fecha</h3>
    <canvas id="graficoConectados" height="200"></canvas>
  </div>
  <div class="card">
    <h3>📊 Matriculados vs Prom. Conectados por Grupo</h3>
    <canvas id="graficoComparativo" height="200"></canvas>
  </div>
</div>

<div class="grid-graficos">
  <div class="card">
    <h3>🚦 Rentabilidad por Grupo</h3>
    <div id="rankingRentabilidad"></div>
  </div>
  <div class="card">
    <h3>💰 Costo Docente vs Ingresos</h3>
    <canvas id="graficoCostos" height="200"></canvas>
  </div>
</div>

<div class="card" style="margin-bottom:20px;">
  <h3>👨‍🏫 Análisis por Docente</h3>
  <div style="overflow-x:auto;">
    <table id="tablaDocentes">
      <thead>
        <tr>
          <th>Docente</th><th>Grupos</th><th>Matriculados</th>
          <th>Prom. Conectados</th><th>Horas</th><th>Costo Docente</th>
          <th>Ingreso</th><th>Margen</th><th>Rentab.</th><th>Estado</th>
        </tr>
      </thead>
      <tbody></tbody>
    </table>
  </div>
</div>

<div class="card" style="margin-bottom:20px;">
  <h3>📋 Detalle por Clase</h3>
  <div style="overflow-x:auto;">
    <table id="tablaDetalle">
      <thead>
        <tr>
          <th>Fecha</th><th>Grupo</th><th>Docente</th><th>Horario</th>
          <th>Horas</th><th>Matriculados</th><th>Conectados</th><th>% Asist.</th>
          <th>Costo Clase</th><th>C/Conectado</th>
        </tr>
      </thead>
      <tbody></tbody>
    </table>
  </div>
</div>

<div class="card" style="margin-bottom:20px;">
  <h3>📚 Todos los Grupos — Resumen Ejecutivo</h3>
  <div style="overflow-x:auto;">
    <table id="tablaGrupos">
      <thead>
        <tr>
          <th>Código</th><th>Docente</th><th>Días</th><th>Turno</th>
          <th>Matric.</th><th>Horas/Clase</th><th>Precio</th>
          <th>Ingreso potencial</th><th>Conect. promedio</th>
          <th>%Asist</th><th>Estado grupo</th><th>Rentab.</th>
        </tr>
      </thead>
      <tbody></tbody>
    </table>
  </div>
</div>

<script>
/* ============================================================
   CONFIGURACIÓN
   ============================================================ */
const PRECIO_CURSO = 299;
const PARAMS = { umbralVerde: 0.30, umbralAmarillo: 0.10 };

let grupos = [
  { n:1,  codigo:"IA-01-NOC-LV0926", matriculados:49, docente:"Luis Romero",     dias:"Lunes y Viernes",     turno:"Noche",  inicio:"2026-09-18", fin:"2026-10-12", horas:3, horario:"7:30 pm a 10:30 pm", estado:"EN CURSO" },
  { n:2,  codigo:"IA-02-NOC-LM0926", matriculados:40, docente:"Wilton Torvisco", dias:"Lunes y miércoles",   turno:"Noche",  inicio:"2026-09-21", fin:"2026-10-14", horas:3, horario:"7:30 pm a 10:30 pm", estado:"EN CURSO" },
  { n:3,  codigo:"IA-03-NOC-MV0926", matriculados:68, docente:"Johan Jara",      dias:"Miércoles y Viernes", turno:"Noche",  inicio:"2026-09-23", fin:"2026-10-16", horas:3, horario:"7:30 pm a 10:30 pm", estado:"EN CURSO" },
  { n:4,  codigo:"IA-04-NOC-MV0926", matriculados:54, docente:"Javier Morán",    dias:"Miércoles y Viernes", turno:"Noche",  inicio:"2026-09-25", fin:"2026-10-21", horas:3, horario:"7:00 pm a 10:00 pm", estado:"EN CURSO" },
  { n:5,  codigo:"IA-05-TAR-SD0926", matriculados:29, docente:"Johan Jara",      dias:"Sábado y Domingo",    turno:"Tarde",  inicio:"2026-10-03", fin:"2026-10-31", horas:4, horario:"3:00 pm a 7:00 pm",  estado:"EN CURSO" },
  { n:6,  codigo:"IA-06-MAÑ-SD1026", matriculados:72, docente:"Johan Jara",      dias:"Sábado y Domingo",    turno:"Mañana", inicio:"2026-10-03", fin:"2026-10-24", horas:4, horario:"9:00 am a 1:00 pm",  estado:"EN CURSO" },
  { n:8,  codigo:"IA-08-NOC-LM1026", matriculados:64, docente:"Ricardo Vargas",  dias:"Lunes y miércoles",   turno:"Noche",  inicio:"2026-10-05", fin:"2026-10-28", horas:3, horario:"7:00 pm a 10:00 pm", estado:"EN CURSO" },
  { n:14, codigo:"IA-14-MAÑ-SD1026", matriculados:47, docente:"Javier Morán",    dias:"Sábado y Domingo",    turno:"Mañana", inicio:"2026-10-10", fin:"2026-11-07", horas:4, horario:"9:00 am a 1:00 pm",  estado:"SIN INICIAR" },
  { n:7,  codigo:"IA-07-NOC-LM1026", matriculados:81, docente:"Maria Solis",     dias:"Lunes y miércoles",   turno:"Noche",  inicio:"2026-10-12", fin:"2026-11-04", horas:3, horario:"7:00 pm a 10:00 pm", estado:"SIN INICIAR" },
  { n:9,  codigo:"IA-09-NOC-LM1026", matriculados:59, docente:"Wilton Torvisco", dias:"Lunes y miércoles",   turno:"Noche",  inicio:"2026-10-19", fin:"2026-11-11", horas:3, horario:"7:30 pm a 10:30 pm", estado:"SIN INICIAR" },
  { n:10, codigo:"IA-10-NOC-MJ1026", matriculados:28, docente:"Johan Jara",      dias:"Martes y Jueves",     turno:"Noche",  inicio:"2026-10-20", fin:"2026-11-12", horas:3, horario:"7:30 pm a 10:30 pm", estado:"SIN INICIAR" },
  { n:11, codigo:"IA-11-NOC-MJ1026", matriculados:1,  docente:"Ricardo Vargas",  dias:"Martes y Jueves",     turno:"Noche",  inicio:"2026-10-27", fin:"2026-11-19", horas:3, horario:"7:00 pm a 10:00 pm", estado:"SIN INICIAR" },
  { n:12, codigo:"IA-12-NOC-MV1126", matriculados:1,  docente:"Javier Morán",    dias:"Miércoles y Viernes", turno:"Noche",  inicio:"2026-11-04", fin:"2026-11-27", horas:3, horario:"7:00 pm a 10:00 pm", estado:"SIN INICIAR" },
  { n:13, codigo:"IA-13-MAÑ-SD1126", matriculados:4,  docente:"Johan Jara",      dias:"Sábado y Domingo",    turno:"Mañana", inicio:"2026-11-07", fin:"2026-11-22", horas:4, horario:"9:00 am a 1:00 pm",  estado:"SIN INICIAR" }
];

const tarifas = {
  "Luis Romero":     60,
  "Javier Morán":    50,
  "Maria Solis":     50,
  "Wilton Torvisco": 50,
  "Johan Jara":      50,
  "Ricardo Vargas":  60
};

let clases = [
  { fecha:"2026-09-18", grupo:"IA-01-NOC-LV0926", conectados:29 },
  { fecha:"2026-09-21", grupo:"IA-01-NOC-LV0926", conectados:34 },
  { fecha:"2026-09-25", grupo:"IA-01-NOC-LV0926", conectados:32 },
  { fecha:"2026-09-28", grupo:"IA-01-NOC-LV0926", conectados:36 },
  { fecha:"2026-09-21", grupo:"IA-02-NOC-LM0926", conectados:38 },
  { fecha:"2026-09-23", grupo:"IA-02-NOC-LM0926", conectados:32 },
  { fecha:"2026-09-28", grupo:"IA-02-NOC-LM0926", conectados:22 },
  { fecha:"2026-09-30", grupo:"IA-02-NOC-LM0926", conectados:18 },
  { fecha:"2026-09-23", grupo:"IA-03-NOC-MV0926", conectados:73 },
  { fecha:"2026-09-25", grupo:"IA-03-NOC-MV0926", conectados:46 },
  { fecha:"2026-09-30", grupo:"IA-03-NOC-MV0926", conectados:37 },
  { fecha:"2026-09-25", grupo:"IA-04-NOC-MV0926", conectados:62 },
  { fecha:"2026-09-30", grupo:"IA-04-NOC-MV0926", conectados:62 }
];

/* ============================================================
   HELPERS
   ============================================================ */
const getGrupo = (cod) => grupos.find(g => g.codigo === cod) || {};
const getClasesGrupo = (cod) => clases.filter(c => c.grupo === cod);
const tarifaDocente = (nombre) => tarifas[nombre] || 50;
const tieneDatos = (cod) => getClasesGrupo(cod).length > 0;

function costoClase(c) {
  const g = getGrupo(c.grupo);
  return (g.horas || 0) * tarifaDocente(g.docente);
}
function costoTotalGrupo(cod, filtroFecha) {
  return getClasesGrupo(cod)
    .filter(c => !filtroFecha || c.fecha === filtroFecha)
    .reduce((s,c) => s + costoClase(c), 0);
}
function ingresoGrupo(cod) { return (getGrupo(cod).matriculados || 0) * PRECIO_CURSO; }
function promConectados(cod, filtroFecha) {
  const cs = getClasesGrupo(cod).filter(c => !filtroFecha || c.fecha === filtroFecha);
  return cs.length ? cs.reduce((s,c)=>s+c.conectados,0) / cs.length : null;
}
function rentabilidadGrupo(cod, filtroFecha) {
  if (!tieneDatos(cod)) return null;
  const costo = costoTotalGrupo(cod, filtroFecha);
  const ingreso = ingresoGrupo(cod);
  const margen = ingreso - costo;
  const rentab = ingreso > 0 ? margen / ingreso : 0;
  return { costo, ingreso, margen, rentab };
}
function obtenerEstado(rentab) {
  if (rentab === null || rentab === undefined) return { txt:"⚪ SIN DATOS", clase:"gris" };
  if (rentab >= PARAMS.umbralVerde)    return { txt:"🟢 RENTABLE",   clase:"verde" };
  if (rentab >= PARAMS.umbralAmarillo) return { txt:"🟡 REVISAR",    clase:"amarillo" };
  return { txt:"🔴 NO RENTABLE", clase:"rojo" };
}

/* ============================================================
   FILTRADO
   ============================================================ */
function gruposFiltrados() {
  const fg = document.getElementById("filtroGrupo").value;
  const fd = document.getElementById("filtroDocente").value;
  const fe = document.getElementById("filtroEstado").value;
  const ft = document.getElementById("filtroTurno").value;
  return grupos.filter(g => {
    if (fg && g.codigo !== fg) return false;
    if (fd && g.docente !== fd) return false;
    if (fe && g.estado !== fe) return false;
    if (ft && g.turno !== ft) return false;
    return true;
  });
}

function fechaSeleccionada() {
  return document.getElementById("filtroFecha").value || "";
}

/* ============================================================
   CAMBIO DE GRUPO → AUTOCOMPLETAR
   ============================================================ */
function cambioGrupo() {
  const cod = document.getElementById("filtroGrupo").value;
  const selDoc = document.getElementById("filtroDocente");
  const selFecha = document.getElementById("filtroFecha");
  const selEstado = document.getElementById("filtroEstado");
  const selTurno = document.getElementById("filtroTurno");
  const panelFechas = document.getElementById("panelFechas");
  const fechasGrid = document.getElementById("fechasGrid");
  const fichaGrupo = document.getElementById("fichaGrupo");
  const fichaCont = document.getElementById("fichaContenido");
  const fichaCod = document.getElementById("fichaCodigo");

  // Limpiar helpers
  ["helperGrupo","helperDocente","helperFecha"].forEach(id => {
    document.getElementById(id).textContent = "";
  });

  // ============ SIN GRUPO: todo libre ============
  if (!cod) {
    selDoc.innerHTML = '<option value="">Todos los docentes</option>' +
      [...new Set(grupos.map(g => g.docente))].map(d => `<option value="${d}">${d}</option>`).join("");
    selDoc.disabled = false;
    selDoc.classList.remove("autocompletado");

    selFecha.innerHTML = '<option value="">— Selecciona un grupo primero —</option>';
    selFecha.disabled = true;
    selFecha.classList.remove("destacado");

    selEstado.disabled = false;
    selTurno.disabled = false;

    panelFechas.style.display = "none";
    fichaGrupo.style.display = "none";
    renderizar();
    return;
  }

  // ============ CON GRUPO: autocompletar y bloquear ============
  const g = getGrupo(cod);

  // Docente
  selDoc.innerHTML = `<option value="${g.docente}">${g.docente}</option>`;
  selDoc.value = g.docente;
  selDoc.disabled = true;
  selDoc.classList.add("autocompletado");
  document.getElementById("helperDocente").textContent = "✅ Autocompletado por el grupo";

  // Estado
  selEstado.value = g.estado;
  selEstado.disabled = true;

  // Turno
  selTurno.value = g.turno;
  selTurno.disabled = true;

  // Fecha
  const fechas = getClasesGrupo(cod).map(c => c.fecha).sort();
  if (fechas.length === 0) {
    selFecha.innerHTML = '<option value="">— Sin clases registradas —</option>';
    selFecha.disabled = true;
    selFecha.classList.remove("destacado");
    panelFechas.style.display = "none";
  } else {
    selFecha.innerHTML = '<option value="">📅 Todas las fechas ('+fechas.length+')</option>' +
      fechas.map(f => {
        const c = getClasesGrupo(cod).find(x => x.fecha === f);
        return `<option value="${f}">${f.split("-").reverse().join("/")} — ${c.conectados} conectados</option>`;
      }).join("");
    selFecha.disabled = false;
    selFecha.classList.add("destacado");
    document.getElementById("helperFecha").textContent = "✅ " + fechas.length + " fechas disponibles";

    // Tarjetas
    fechasGrid.innerHTML = fechas.map(f => {
      const c = getClasesGrupo(cod).find(x => x.fecha === f);
      const pct = g.matriculados ? ((c.conectados/g.matriculados)*100) : 0;
      const clase = pct >= 60 ? "verde" : pct >= 40 ? "amarillo" : "rojo";
      return `
        <div class="fecha-card" onclick="seleccionarFecha('${f}')" data-fecha="${f}">
          <div class="fd">${f.split("-").reverse().join("/")}</div>
          <div class="fc">${c.conectados}</div>
          <div class="fp ${clase}">${pct.toFixed(0)}% asist.</div>
        </div>
      `;
    }).join("");
    panelFechas.style.display = "block";
  }

  // Ficha del grupo
  fichaCod.textContent = cod;
  const r = rentabilidadGrupo(cod);
  const estado = obtenerEstado(r ? r.rentab : null);
  fichaCont.innerHTML = `
    <div><div class="lbl">Docente</div><div class="val">${g.docente}</div></div>
    <div><div class="lbl">Días</div><div class="val">${g.dias}</div></div>
    <div><div class="lbl">Horario</div><div class="val">${g.horario}</div></div>
    <div><div class="lbl">Turno</div><div class="val">${g.turno}</div></div>
    <div><div class="lbl">Matriculados</div><div class="val">${g.matriculados}</div></div>
    <div><div class="lbl">Horas/clase</div><div class="val">${g.horas}h</div></div>
    <div><div class="lbl">Inicio</div><div class="val">${g.inicio.split("-").reverse().join("/")}</div></div>
    <div><div class="lbl">Fin</div><div class="val">${g.fin.split("-").reverse().join("/")}</div></div>
    <div><div class="lbl">Ingreso potencial</div><div class="val">S/ ${(g.matriculados*PRECIO_CURSO).toFixed(0)}</div></div>
    <div><div class="lbl">Estado del grupo</div><div class="val">${g.estado}</div></div>
    <div><div class="lbl">Rentabilidad</div><div class="val"><span class="badge ${estado.clase}">${estado.txt}</span></div></div>
    <div><div class="lbl">Tarifa docente</div><div class="val">S/ ${tarifaDocente(g.docente)}/h</div></div>
  `;
  fichaGrupo.style.display = "block";

  renderizar();
}

function seleccionarFecha(fecha) {
  const selFecha = document.getElementById("filtroFecha");
  selFecha.value = (selFecha.value === fecha) ? "" : fecha;
  document.querySelectorAll(".fecha-card").forEach(el => {
    el.classList.toggle("activa", el.dataset.fecha === selFecha.value);
  });
  renderizar();
}

/* ============================================================
   KPIs
   ============================================================ */
function calcularKPIs() {
  const gf = gruposFiltrados();
  const ff = fechaSeleccionada();

  const gruposConDatos = gf.filter(g => tieneDatos(g.codigo)).map(g => g.codigo);
  let cf = clases.filter(c => gruposConDatos.includes(c.grupo));
  if (ff) cf = cf.filter(c => c.fecha === ff);

  const matConDatos = gruposConDatos.reduce((s,cod)=>s+(getGrupo(cod).matriculados||0), 0);
  const totalConect = cf.reduce((s,c)=>s+c.conectados, 0);
  const promConect = cf.length ? totalConect / cf.length : 0;
  const asistProm = cf.length
    ? cf.reduce((s,c) => s + (c.conectados / (getGrupo(c.grupo).matriculados||1)), 0) / cf.length
    : 0;
  const horasTotales = cf.reduce((s,c)=>s+(getGrupo(c.grupo).horas||0), 0);
  const costoTotal = cf.reduce((s,c)=>s+costoClase(c), 0);
  const ingresoTotal = gruposConDatos.reduce((s,cod)=>s+ingresoGrupo(cod), 0);
  const margen = ingresoTotal - costoTotal;
  const rentab = ingresoTotal > 0 ? margen/ingresoTotal : 0;
  const costoPorMatric = matConDatos > 0 ? costoTotal/matConDatos : 0;
  const costoPorConect = promConect > 0 ? costoTotal/promConect : 0;
  const ingresoPorAlumno = matConDatos > 0 ? ingresoTotal/matConDatos : PRECIO_CURSO;
  const puntoEq = ingresoPorAlumno > 0 ? Math.ceil(costoTotal / ingresoPorAlumno) : 0;

  return {
    matriculados: matConDatos,
    totalConectados: totalConect,
    promConectados: promConect,
    porcentajeAsistencia: asistProm * 100,
    numClases: cf.length,
    horasTotales, costoTotal, costoPorMatric, costoPorConect,
    ingresoTotal, margen, rentabilidad: rentab, puntoEq,
    gruposConDatos: gruposConDatos.length
  };
}

/* ============================================================
   RENDERIZADO
   ============================================================ */
let chartConectados, chartComparativo, chartCostos;

function renderizar() {
  const k = calcularKPIs();
  const ff = fechaSeleccionada();
  const banner = document.getElementById("bannerEstado");

  if (k.gruposConDatos === 0 || k.numClases === 0) {
    banner.textContent = ff
      ? `⚪ No hay clases el ${ff.split("-").reverse().join("/")}`
      : "⚪ No hay grupos con datos reales en el filtro seleccionado";
    banner.className = "estado-banner estado-info";
  } else {
    const estado = obtenerEstado(k.rentabilidad);
    const signo = k.margen >= 0 ? "+" : "";
    const etiquetaFecha = ff ? ` · 📅 ${ff.split("-").reverse().join("/")}` : "";
    banner.textContent = `${estado.txt} — Margen: S/ ${signo}${k.margen.toFixed(0)} | Rentabilidad: ${(k.rentabilidad*100).toFixed(1)}%${etiquetaFecha}`;
    banner.className = "estado-banner estado-" + estado.clase;
  }

  const kpisHTML = [
    { t:"👥 Matriculados", v:k.matriculados, s:`${k.gruposConDatos} grupos con datos`, c:"azul" },
    { t:"🟢 Prom. Conectados", v:k.promConectados.toFixed(1), s:"por clase", c:"verde" },
    { t:"📈 % Asistencia", v:k.porcentajeAsistencia.toFixed(1)+"%", s:"meta 60%", c: k.porcentajeAsistencia >= 60 ? "verde" : "amarillo" },
    { t:"📚 Clases", v:k.numClases, s: ff ? "esa fecha" : "realizadas", c:"azul" },
    { t:"⏱️ Horas dictadas", v:k.horasTotales, s:"en el período", c:"azul" },
    { t:"💰 Costo Docente", v:"S/ "+k.costoTotal.toFixed(0), s:"total período", c:"rojo" },
    { t:"🎯 C/Matriculado", v:"S/ "+k.costoPorMatric.toFixed(2), s:"por alumno matric.", c:"amarillo" },
    { t:"🎯 C/Conectado", v:"S/ "+k.costoPorConect.toFixed(2), s:"por conectado", c:"amarillo" },
    { t:"💵 Ingreso", v:"S/ "+k.ingresoTotal.toFixed(0), s:"grupos con datos", c:"verde" },
    { t:"📊 Margen", v:"S/ "+k.margen.toFixed(0), s:"ingreso − costo", c: k.margen >= 0 ? "verde" : "rojo" },
    { t:"📉 Rentabilidad", v:(k.rentabilidad*100).toFixed(1)+"%", s:"margen/ingreso", c: obtenerEstado(k.rentabilidad).clase },
    { t:"⚖️ P.Equilibrio", v:k.puntoEq, s:"alumnos mínimos", c:"azul" }
  ];
  document.getElementById("kpis").innerHTML = kpisHTML.map(x => `
    <div class="kpi ${x.c}">
      <div class="titulo">${x.t}</div>
      <div class="valor">${x.v}</div>
      <div class="sub">${x.s}</div>
    </div>
  `).join("");

  renderizarGraficos();
  renderizarRanking();
  renderizarTablaDocentes();
  renderizarTablaDetalle();
  renderizarTablaGrupos();
}

function renderizarGraficos() {
  const ff = fechaSeleccionada();
  const gf = gruposFiltrados().filter(g => tieneDatos(g.codigo)).map(g => g.codigo);
  let cf = clases.filter(c => gf.includes(c.grupo));
  if (ff) cf = cf.filter(c => c.fecha === ff);

  const porFecha = {};
  cf.forEach(d => { porFecha[d.fecha] = (porFecha[d.fecha]||0) + d.conectados; });
  const fechas = Object.keys(porFecha).sort();
  const valores = fechas.map(f => porFecha[f]);

  if (chartConectados) chartConectados.destroy();
  chartConectados = new Chart(document.getElementById("graficoConectados"), {
    type: "line",
    data: {
      labels: fechas.map(f => f.slice(5).split("-").reverse().join("/")),
      datasets: [{
        label:"Conectados",
        data: valores,
        borderColor:"#3b82f6",
        backgroundColor:"rgba(59,130,246,0.15)",
        tension:0.35, fill:true, pointRadius:5, pointBackgroundColor:"#1e40af"
      }]
    },
    options: { responsive:true, plugins:{ legend:{ display:false } } }
  });

  const mats = gf.map(g => getGrupo(g).matriculados);
  const prom = gf.map(g => {
    const cs = cf.filter(c => c.grupo === g);
    return cs.length ? Math.round(cs.reduce((s,c)=>s+c.conectados,0)/cs.length) : 0;
  });
  const etiquetas = gf.map(g => g.split("-").slice(0,2).join("-"));

  if (chartComparativo) chartComparativo.destroy();
  chartComparativo = new Chart(document.getElementById("graficoComparativo"), {
    type: "bar",
    data: {
      labels: etiquetas,
      datasets: [
        { label:"Matriculados", data:mats, backgroundColor:"#93c5fd" },
        { label:"Prom. Conectados", data:prom, backgroundColor:"#22c55e" }
      ]
    },
    options: { responsive:true }
  });

  const costos = gf.map(g => costoTotalGrupo(g, ff));
  const ingresos = gf.map(g => ingresoGrupo(g));

  if (chartCostos) chartCostos.destroy();
  chartCostos = new Chart(document.getElementById("graficoCostos"), {
    type: "bar",
    data: {
      labels: etiquetas,
      datasets: [
        { label:"Costo Docente", data:costos, backgroundColor:"#ef4444" },
        { label:"Ingreso", data:ingresos, backgroundColor:"#22c55e" }
      ]
    },
    options: { responsive:true }
  });
}

function renderizarRanking() {
  const ff = fechaSeleccionada();
  const gf = gruposFiltrados().filter(g => tieneDatos(g.codigo));
  const lista = gf.map(g => {
    const r = rentabilidadGrupo(g.codigo, ff);
    return { grupo:g.codigo.split("-").slice(0,2).join("-"), rentab: r ? r.rentab : 0 };
  }).sort((a,b) => b.rentab - a.rentab);

  document.getElementById("rankingRentabilidad").innerHTML = lista.map(x => {
    const pct = (x.rentab*100).toFixed(1);
    const color = x.rentab>=PARAMS.umbralVerde?"#22c55e":x.rentab>=PARAMS.umbralAmarillo?"#eab308":"#ef4444";
    return `
      <div class="fila-rentab">
        <span class="nombre">${x.grupo}</span>
        <div class="barra-cont">
          <div class="barra-fill" style="width:${Math.max(0,Math.min(100,x.rentab*100))}%;background:${color};"></div>
        </div>
        <span class="pct" style="color:${color}">${pct}%</span>
      </div>
    `;
  }).join("");
}

function renderizarTablaDocentes() {
  const ff = fechaSeleccionada();
  const gf = gruposFiltrados();
  const docentes = [...new Set(gf.map(g => g.docente))];

  document.querySelector("#tablaDocentes tbody").innerHTML = docentes.map(doc => {
    const gruposDoc = gf.filter(g => g.docente === doc);
    const gruposConDatos = gruposDoc.filter(g => tieneDatos(g.codigo));
    const gruposConDatosCod = gruposConDatos.map(g=>g.codigo);
    let clasesDoc = clases.filter(c => gruposConDatosCod.includes(c.grupo));
    if (ff) clasesDoc = clasesDoc.filter(c => c.fecha === ff);

    const totalMatric = gruposConDatos.reduce((s,g)=>s+g.matriculados, 0);
    const promConect = clasesDoc.length ? clasesDoc.reduce((s,c)=>s+c.conectados,0)/clasesDoc.length : 0;
    const horas = clasesDoc.reduce((s,c)=>s+(getGrupo(c.grupo).horas||0), 0);
    const costo = clasesDoc.reduce((s,c)=>s+costoClase(c), 0);
    const ingreso = gruposConDatos.reduce((s,g)=>s+ingresoGrupo(g.codigo), 0);
    const margen = ingreso - costo;
    const rentab = ingreso > 0 ? margen/ingreso : null;
    const estado = obtenerEstado(rentab);

    return `
      <tr>
        <td><b>${doc}</b></td>
        <td>${gruposDoc.length} (${gruposConDatos.length} con datos)</td>
        <td>${totalMatric}</td>
        <td>${promConect.toFixed(1)}</td>
        <td>${horas}</td>
        <td>S/ ${costo.toFixed(0)}</td>
        <td>S/ ${ingreso.toFixed(0)}</td>
        <td>S/ ${margen.toFixed(0)}</td>
        <td>${rentab !== null ? (rentab*100).toFixed(1)+"%" : "—"}</td>
        <td><span class="badge ${estado.clase}">${estado.txt}</span></td>
      </tr>
    `;
  }).join("");
}

function renderizarTablaDetalle() {
  const ff = fechaSeleccionada();
  const gf = gruposFiltrados().filter(g => tieneDatos(g.codigo)).map(g => g.codigo);
  let cf = clases.filter(c => gf.includes(c.grupo)).sort((a,b) => a.fecha.localeCompare(b.fecha));
  if (ff) cf = cf.filter(c => c.fecha === ff);

  document.querySelector("#tablaDetalle tbody").innerHTML = cf.map(d => {
    const g = getGrupo(d.grupo);
    const costo = costoClase(d);
    const asist = g.matriculados ? ((d.conectados/g.matriculados)*100).toFixed(1) : "—";
    const cConect = d.conectados ? (costo/d.conectados).toFixed(2) : "—";
    const color = parseFloat(asist)>=60?"verde":parseFloat(asist)>=40?"amarillo":"rojo";
    return `
      <tr>
        <td>${d.fecha.split("-").reverse().join("/")}</td>
        <td><b>${d.grupo.split("-").slice(0,2).join("-")}</b></td>
        <td>${g.docente}</td>
        <td style="font-size:11px;color:#64748b;">${g.horario}</td>
        <td>${g.horas}</td>
        <td>${g.matriculados}</td>
        <td>${d.conectados}</td>
        <td><span class="badge ${color}">${asist}%</span></td>
        <td>S/ ${costo.toFixed(0)}</td>
        <td>S/ ${cConect}</td>
      </tr>
    `;
  }).join("");
}

function renderizarTablaGrupos() {
  const gf = gruposFiltrados();
  document.querySelector("#tablaGrupos tbody").innerHTML = gf.map(g => {
    const sinDatos = !tieneDatos(g.codigo);
    const prom = promConectados(g.codigo);
    const asist = (prom !== null && g.matriculados)
      ? ((prom/g.matriculados)*100).toFixed(1)+"%" : "—";
    const r = rentabilidadGrupo(g.codigo);
    const estado = obtenerEstado(r ? r.rentab : null);
    return `
      <tr class="${sinDatos ? 'fila-sin-datos' : ''}">
        <td><b>${g.codigo}</b></td>
        <td>${g.docente}</td>
        <td style="font-size:11px;">${g.dias}</td>
        <td>${g.turno}</td>
        <td>${g.matriculados}</td>
        <td>${g.horas}</td>
        <td>S/ ${PRECIO_CURSO}</td>
        <td>S/ ${(g.matriculados*PRECIO_CURSO).toFixed(0)}</td>
        <td>${prom !== null ? prom.toFixed(1) : "—"}</td>
        <td>${asist}</td>
        <td><span class="badge ${g.estado==='EN CURSO'?'verde':'gris'}">${g.estado}</span></td>
        <td><span class="badge ${estado.clase}">${estado.txt}</span></td>
      </tr>
    `;
  }).join("");
}

function poblarFiltros() {
  document.getElementById("filtroGrupo").innerHTML =
    '<option value="">Todos los grupos</option>' +
    grupos.map(g => `<option value="${g.codigo}">${g.codigo} — ${g.docente}</option>`).join("");
  document.getElementById("filtroDocente").innerHTML =
    '<option value="">Todos los docentes</option>' +
    [...new Set(grupos.map(g => g.docente))].map(d => `<option value="${d}">${d}</option>`).join("");
}

function exportarCSV() {
  const ff = fechaSeleccionada();
  const gf = gruposFiltrados().filter(g => tieneDatos(g.codigo)).map(g => g.codigo);
  let cf = clases.filter(c => gf.includes(c.grupo));
  if (ff) cf = cf.filter(c => c.fecha === ff);
  let csv = "Fecha,Grupo,Docente,Horario,Horas,Matriculados,Conectados,%Asistencia,CostoClase\n";
  cf.forEach(d => {
    const g = getGrupo(d.grupo);
    const costo = costoClase(d);
    const asist = g.matriculados ? ((d.conectados/g.matriculados)*100).toFixed(1) : 0;
    csv += `${d.fecha},${d.grupo},${g.docente},${g.horario},${g.horas},${g.matriculados},${d.conectados},${asist}%,${costo.toFixed(2)}\n`;
  });
  const blob = new Blob([csv], { type:"text/csv;charset=utf-8;" });
  const url = URL.createObjectURL(blob);
  const a = document.createElement("a");
  a.href = url; a.download = "asistencia_IA.csv"; a.click();
}

/* INICIALIZAR */
poblarFiltros();
cambioGrupo();
</script>
</body>
</html>
