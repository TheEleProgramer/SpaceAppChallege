<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
  <title>Selene: compare Moon landing sites</title>
  <meta name="description" content="Selene compares Moon landing sites and dates: Sun and Earth positions, sunlight, communication windows and power potential, especially near the lunar south pole.">
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Manrope:wght@400;600;700&family=Unbounded:wght@500;600;700&display=swap">
  <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css">
  <style>
:root {
  color-scheme: dark;
  --bg: #04060b;
  --bg0: #04060b;
  --fg: #e7ecf5;
  --mut: #8f9bb0;
  --card: rgba(14, 21, 35, .72);
  --bd: rgba(150, 175, 220, .17);
  --sun: #ffb938;
  --ea: #4fa8ff;
  --gr: #262d37;
  --ok: #5ff0a8;
  --wa: #ffb938;
  --no: #ff7a6b;
  --day: rgba(255, 185, 56, .13);
  --sky: #050911;
  --neb1: rgba(52, 96, 178, .32);
  --neb2: rgba(26, 72, 140, .18);
  --neb3: rgba(70, 82, 170, .16);
  --star: #ffffff;
  --ac1: #6ee7c8;
  --ac2: #b69cff;
  --glow: rgba(79, 168, 255, .5);
  --hero1: rgba(2, 5, 12, .5);
  --hero2: rgba(10, 22, 42, .78);
  box-sizing: border-box;
  padding-top: env(safe-area-inset-top, 0px);
  padding-bottom: env(safe-area-inset-bottom, 0px);
}
:root[data-theme="nebula"] {
  --bg0: #07040f;
  --neb1: rgba(150, 76, 255, .34);
  --neb2: rgba(255, 72, 156, .2);
  --neb3: rgba(86, 64, 210, .22);
  --star: #f4e9ff;
  --ac1: #ff8fd0;
  --ac2: #9cc6ff;
  --glow: rgba(196, 112, 255, .55);
  --hero1: rgba(8, 3, 16, .5);
  --hero2: rgba(32, 14, 56, .78);
}
:root[data-theme="aurora"] {
  --bg0: #030a0b;
  --neb1: rgba(30, 204, 164, .26);
  --neb2: rgba(60, 120, 224, .18);
  --neb3: rgba(22, 168, 124, .18);
  --star: #ecfff8;
  --ac1: #7ef0c5;
  --ac2: #8fd0ff;
  --glow: rgba(60, 230, 170, .45);
  --hero1: rgba(2, 10, 10, .5);
  --hero2: rgba(8, 36, 38, .78);
}
* {
  box-sizing: border-box;
}
html {
  scroll-padding-top: env(safe-area-inset-top, 0px);
}
body {
  margin: 0;
  color: var(--fg);
  font: 15px/1.55 "Manrope", "Avenir Next", "Segoe UI", system-ui, sans-serif;
  background:
    radial-gradient(900px 520px at 85% -5%, var(--neb1), transparent 65%),
    radial-gradient(820px 620px at 4% 42%, var(--neb2), transparent 65%),
    radial-gradient(760px 520px at 72% 108%, var(--neb3), transparent 62%),
    var(--bg0);
  background-attachment: fixed;
}
#space {
  position: fixed;
  inset: 0;
  width: 100%;
  height: 100%;
  z-index: -2;
  pointer-events: none;
}
body::before {
  content: "";
  position: fixed;
  left: -15%;
  right: -15%;
  bottom: -44vh;
  height: 54vh;
  z-index: -1;
  pointer-events: none;
  border-radius: 50%;
  background: radial-gradient(ellipse at 50% 0, rgba(190, 205, 232, .3), rgba(70, 82, 104, .28) 14%, rgba(20, 26, 38, .55) 40%, transparent 70%);
  box-shadow: 0 -2px 60px var(--glow);
}
main {
  max-width: 1120px;
  margin: 0 auto;
  padding: 16px 14px 44px;
}
.top {
  position: relative;
  overflow: hidden;
  border: 1px solid var(--bd);
  border-radius: 18px;
  min-height: 230px;
  margin-bottom: 14px;
  background: linear-gradient(var(--hero1), var(--hero2));
  backdrop-filter: blur(2px);
}
.art {
  position: absolute;
  inset: 0;
  width: 100%;
  height: 100%;
}
.rise {
  animation: rise 3s cubic-bezier(.2, .7, .2, 1) both;
}
@keyframes rise {
  from {
    transform: translateY(90px);
  }
}
@media (prefers-reduced-motion: reduce) {
  .rise {
    animation: none;
  }
}
.ht {
  position: relative;
  padding: 24px 22px 76px;
  max-width: 640px;
}
h1 {
  font:
    700 clamp(32px, 6vw, 50px)/1 "Unbounded",
    "Trebuchet MS",
    "Segoe UI",
    sans-serif;
  letter-spacing: .03em;
  margin: 0 0 10px;
  text-shadow: 0 0 28px var(--glow);
}
.lead {
  color: #b4bfd2;
  margin: 0;
  max-width: 62ch;
}
.lg {
  position: absolute;
  right: 14px;
  top: 12px;
  display: flex;
  gap: 8px;
}
.lg select {
  width: auto;
  background: rgba(5, 9, 17, .7);
}
.bar {
  position: sticky;
  top: 8px;
  z-index: 5;
  display: flex;
  flex-wrap: wrap;
  gap: 10px 14px;
  align-items: center;
  background: rgba(9, 14, 24, .88);
  backdrop-filter: blur(10px);
  border: 1px solid var(--bd);
  border-radius: 14px;
  padding: 10px 14px;
  margin-bottom: 14px;
}
.bar label {
  display: flex;
  gap: 8px;
  align-items: center;
  font-size: 13px;
  color: var(--mut);
}
.bar input[type=range] {
  flex: 1;
  min-width: 160px;
  accent-color: var(--sun);
}
button {
  font: inherit;
  font-weight: 600;
  background: linear-gradient(#ffcf6b, #ffb938);
  color: #1a1200;
  border: 0;
  border-radius: 9px;
  padding: 6px 14px;
  cursor: pointer;
}
button:hover {
  filter: brightness(1.08);
}
#horizonsStatus {
  flex-basis: 100%;
  color: var(--mut);
  font-size: 12px;
  line-height: 1.45;
}
#horizonsStatus.ok { color: var(--ok); }
#horizonsStatus.err { color: var(--no); }
.terrain-panel {
  margin-top: 16px;
  padding: 16px;
  border: 1px solid var(--bd);
  border-radius: 16px;
  background: var(--card);
}
.terrain-panel h2 {
  margin: 0 0 8px;
  font: 600 16px/1.3 "Unbounded", "Trebuchet MS", sans-serif;
  color: var(--ac1);
}
.terrain-panel p { color: #c2cde0; margin: 6px 0 12px; font-size: 13px; }
.terrain-links { display:flex; flex-wrap:wrap; gap:8px; margin: 10px 0 12px; }
.terrain-links a {
  display:inline-block; padding:7px 11px; border:1px solid #33425e;
  border-radius:8px; color:var(--fg); text-decoration:none; font-size:13px;
  background:#0a1019;
}
.terrain-links a:hover { border-color:var(--ea); }
.south-map { width:100%; height:min(62vh,560px); min-height:380px; border:1px solid var(--bd); border-radius:12px; background:#070b13; overflow:hidden; }
.terrain-3d { width:100%; height:440px; border:1px solid var(--bd); border-radius:12px; background:#050912; overflow:hidden; position:relative; }
.terrain-3d canvas { display:block; width:100%; height:100%; }
.terrain-controls { display:flex; flex-wrap:wrap; align-items:center; gap:9px; margin:10px 0; }
.terrain-controls select,.terrain-controls button { background:#0a1019; color:var(--fg); border:1px solid #33425e; border-radius:8px; padding:8px 10px; }
.terrain-status { color:var(--mut); font-size:12px !important; }
.terrain-note { font-size:12px !important; color:var(--mut) !important; }
.leaflet-container { background:#080d16; font:12px Manrope,sans-serif; }
.leaflet-control-attribution { font-size:10px !important; }
.countdown-grid { display:grid; grid-template-columns:repeat(auto-fit,minmax(190px,1fr)); gap:8px; margin:8px 0 12px; }
.countdown-item { border:1px solid var(--bd); border-radius:10px; padding:9px 11px; background:rgba(255,255,255,.025); }
.countdown-item b { display:block; font-size:16px; color:var(--fg); }
.countdown-item span { display:block; color:var(--mut); font-size:11px; margin-top:3px; }
@media (max-width:600px) { .south-map { min-height:330px; height:50vh; } .terrain-3d { height:340px; } }
button:focus-visible,
select:focus-visible,
input:focus-visible,
summary:focus-visible {
  outline: 2px solid var(--ea);
  outline-offset: 2px;
}
#why,
#share,
#csv {
  background: transparent;
  color: var(--fg);
  border: 1px solid #33425e;
}
#whyt {
  background: var(--card);
  border: 1px solid #3a4d73;
  border-left: 3px solid var(--sun);
  border-radius: 12px;
  padding: 12px 14px;
  margin: 0 0 14px;
  color: #cdd6e6;
}
#cards {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(340px, 1fr));
  gap: 14px;
}
.card {
  background: linear-gradient(180deg, rgba(20, 29, 47, .72), rgba(9, 14, 25, .78));
  backdrop-filter: blur(16px) saturate(1.2);
  border: 1px solid var(--bd);
  border-top: 2px solid var(--ac);
  border-radius: 18px;
  padding: 16px;
  min-width: 0;
  box-shadow:
    0 22px 60px rgba(0, 0, 0, .5),
    inset 0 1px 0 rgba(255, 255, 255, .07),
    0 -8px 40px -18px var(--ac);
}
#c0 {
  --ac: var(--ac1);
}
#c1 {
  --ac: var(--ac2);
}
.card h2 {
  font:
    600 15px/1 "Unbounded",
    "Trebuchet MS",
    sans-serif;
  color: var(--ac);
  margin: 0 0 12px;
  letter-spacing: .04em;
}
.row {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 8px;
  margin-bottom: 8px;
}
.row label {
  display: flex;
  flex-direction: column;
  font-size: 12.5px;
  color: var(--mut);
  gap: 3px;
}
.row .w {
  grid-column: 1/-1;
}
select,
input[type=number],
input[type=date] {
  font: inherit;
  color: var(--fg);
  background: #0a1019;
  border: 1px solid #2a3750;
  border-radius: 8px;
  padding: 6px 8px;
  width: 100%;
}
.row input[type=range] {
  accent-color: var(--ac);
}
.sh {
  display: flex;
  gap: 6px;
  flex-wrap: wrap;
  align-items: center;
  margin-bottom: 10px;
}
.sh button {
  padding: 3px 10px;
  font-size: 12.5px;
  background: #0a1019;
  color: var(--fg);
  border: 1px solid #2a3750;
}
.v {
  display: inline-block;
  font-weight: 700;
  padding: 4px 16px;
  border-radius: 99px;
  margin: 4px 0 6px;
  border: 1px solid;
}
.ok {
  color: var(--ok);
  background: #0c2a20;
  border-color: #1f7a56;
  box-shadow: 0 0 20px rgba(95, 240, 168, .2);
}
.wa {
  color: var(--wa);
  background: #2c200a;
  border-color: #8a5d10;
  box-shadow: 0 0 20px rgba(255, 185, 56, .18);
}
.no {
  color: var(--no);
  background: #2d1311;
  border-color: #8a2f26;
  box-shadow: 0 0 20px rgba(255, 122, 107, .18);
}
.g {
  display: flex;
  align-items: center;
  font-size: 13px;
  font-weight: 600;
  color: #c2cde0;
  margin: 10px 0 5px;
}
.ic {
  width: 10px;
  height: 10px;
  border-radius: 50%;
  margin-right: 8px;
}
.ic.s {
  background: var(--sun);
  box-shadow: 0 0 10px var(--sun);
}
.ic.e {
  background: var(--ea);
  box-shadow: 0 0 10px var(--ea);
}
.ic.b {
  background:
    linear-gradient(
      90deg,
      var(--sun) 50%,
      var(--ea) 50%);
}
.tiles {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
  gap: 7px;
  margin-bottom: 6px;
}
.t {
  cursor: help;
  border: 1px solid var(--bd);
  border-radius: 11px;
  padding: 8px 10px;
  background: rgba(255, 255, 255, .025);
}
.t b {
  display: block;
  font:
    500 19px/1.2 "Unbounded",
    "Trebuchet MS",
    sans-serif;
  font-variant-numeric: tabular-nums;
}
.t span {
  font-size: 12px;
  color: var(--mut);
}
.t em {
  display: block;
  font-style: normal;
  font-size: 12px;
  margin-top: 3px;
  color: #b4bfd2;
}
.tiles.s .t:first-child {
  border-color: rgba(255, 185, 56, .55);
  background: rgba(255, 185, 56, .07);
}
.tiles.e .t:first-child {
  border-color: rgba(79, 168, 255, .55);
  background: rgba(79, 168, 255, .07);
}
svg {
  width: 100%;
  height: auto;
  display: block;
  margin-bottom: 6px;
  border-radius: 10px;
}
.ch {
  background: #070b13;
  border: 1px solid var(--bd);
}
.cap {
  font-size: 12px;
  color: var(--mut);
  margin: 8px 0 6px;
}
.sw {
  display: inline-block;
  width: 10px;
  height: 10px;
  border-radius: 50%;
  margin: 0 4px 0 6px;
}
.fin h3 {
  font:
    600 14px/1.2 "Unbounded",
    "Trebuchet MS",
    sans-serif;
  margin: 16px 0 2px;
}
details {
  background: var(--card);
  border: 1px solid var(--bd);
  border-radius: 12px;
  padding: 10px 14px;
  margin-bottom: 14px;
}
summary {
  cursor: pointer;
  font-weight: 600;
}
details ul {
  margin: 8px 0 2px;
  padding-left: 18px;
}
details li {
  margin: 5px 0;
  font-size: 13.5px;
  color: #c2cde0;
}
.sec {
  font: 600 13px/1.2 "Manrope", sans-serif;
  color: #c2cde0;
  margin: 16px 0 6px;
}
.lgd {
  display: flex;
  flex-wrap: wrap;
  gap: 6px 14px;
  font-size: 12px;
  color: var(--mut);
  margin: 0 0 6px;
}
.lgd span {
  display: inline-flex;
  align-items: center;
  gap: 6px;
}
.lgd i {
  display: inline-block;
}
.ln {
  width: 18px;
  height: 3px;
  border-radius: 2px;
}
.ln.dash {
  background:
    repeating-linear-gradient(
      90deg,
      var(--mut) 0 4px,
      transparent 4px 7px);
}
.bx {
  width: 14px;
  height: 10px;
  border-radius: 2px;
  background:
    linear-gradient(
      90deg,
      rgba(255, 185, 56, .45),
      rgba(79, 168, 255, .45));
}
.cu {
  width: 2px;
  height: 12px;
  background: var(--fg);
}
.dome,
svg[data-ch] {
  touch-action: none;
  user-select: none;
}
.dome {
  cursor: grab;
  background:
    radial-gradient(
      circle at 50% 30%,
      #0e1a2e,
      #04070d);
  border: 1px solid var(--bd);
  border-radius: 12px;
}
svg[data-ch] {
  cursor: col-resize;
}
.dv {
  display: flex;
  gap: 6px;
  padding: 8px 10px 0;
}
.dv button {
  padding: 3px 10px;
  font-size: 12px;
  background: #0a1019;
  color: var(--fg);
  border: 1px solid #2a3750;
}
#cards.one {
  grid-template-columns: minmax(0, 800px);
  justify-content: center;
}
.sr {
  position: absolute;
  left: -9999px;
}
  </style>
</head>

<body>
  <canvas id="space" aria-hidden="true"></canvas>
  <main>
    <div class="top" id="top"></div>
    <div class="bar" id="bar"></div>
    <p id="horizonsStatus" role="status" aria-live="polite"></p>
    <div id="guide"></div>
    <p id="whyt" class="lead" hidden></p>
    <div id="cards"></div>
    <section class="terrain-panel" aria-labelledby="terrainTitle">
      <h2 id="terrainTitle">Mapa y relieve real · Polo sur lunar</h2>
      <p id="terrainCoordinates">Coordenadas del escenario A: —</p>
      <div class="terrain-links">
        <a href="https://trek.nasa.gov/moon/" target="_blank" rel="noopener noreferrer">Moon Trek (NASA)</a>
        <a href="https://quickmap.lroc.im-ldi.com/" target="_blank" rel="noopener noreferrer">LROC QuickMap</a>
        <a href="https://pgda.gsfc.nasa.gov/data/LOLA_5mpp/" target="_blank" rel="noopener noreferrer">Modelos LOLA 5 m/píxel (NASA)</a>
        <a href="https://pgda.gsfc.nasa.gov/products/90" target="_blank" rel="noopener noreferrer">DEM LOLA de gran área</a>
      </div>
      <h3 class="sec">Mapa de la región sur</h3>
      <div id="southMap" class="south-map" role="application" aria-label="Mapa de la superficie lunar del hemisferio sur"></div>
      <p class="terrain-note">Cartografía de la Luna obtenida del mosaico WAC de NASA/JPL mediante el servicio WMTS Moon Trek. El mapa está limitado a latitudes al sur de 60° S. Haz clic en el mapa para trasladar las coordenadas al escenario A.</p>
      <h3 class="sec">Modelo 3D del terreno medido por LOLA</h3>
      <div class="terrain-controls">
        <label for="lolaSite">Zona topográfica:</label>
        <select id="lolaSite">
          <option value="Site01">Connecting ridge (Site 01)</option>
          <option value="Site04">Borde del cráter Shackleton (Site 04)</option>
          <option value="Site07">Pico cerca de Shackleton (Site 07)</option>
          <option value="Site11">Borde de de Gerlache (Site 11)</option>
          <option value="Site20">Meseta Leibnitz beta (Site 20)</option>
          <option value="Site23">Macizo Malapert (Site 23)</option>
        </select>
        <label for="lolaTarget">Horizonte para:</label>
        <select id="lolaTarget"><option value="0">Escenario A</option><option value="1">Escenario B</option></select>
        <button id="loadLola" type="button">Cargar relieve LOLA</button>
      </div>
      <div id="lola3d" class="terrain-3d" aria-label="Modelo tridimensional de elevación lunar"></div>
      <p id="lolaStatus" class="terrain-status" role="status" aria-live="polite">Selecciona una zona y pulsa “Cargar relieve LOLA”. Se consultará el GeoTIFF topográfico oficial de NASA.</p>
      <p class="terrain-note">El modelo 3D se construye con las alturas del GeoTIFF LOLA de 5 m/píxel para las seis zonas de aterrizaje publicadas por NASA. El relieve que se muestra corresponde a la zona elegida; no se debe interpretar como un DEM global del polo sur ni como el relieve exacto de cualquier coordenada. El archivo se consulta desde el servidor de NASA y requiere conexión a internet y permisos CORS del servidor.</p>
    </section>
    <section class="terrain-panel" aria-labelledby="dataTitle">
      <h2 id="dataTitle">Datos de NASA usados y validación</h2>
      <p><b>Relieve:</b> GeoTIFF LOLA (LRO) de 5 m/píxel, de PGDA/GSFC. Al cargarlo, Selene calcula el horizonte real (hasta ~4,5 km) y lo usa en el escenario A: el Sol o la Tierra “salen” cuando superan el relieve, no un ángulo fijo.</p>
      <p><b>Mapa:</b> mosaico LRO WAC (Moon Trek, NASA/JPL). <b>Sol y Tierra:</b> cálculo propio aproximado (serie corta ELP/Meeus), no son datos directos de NASA.</p>
      <p><b>Validar con JPL Horizons</b> (Observer Table; cuerpo central: Moon 301; Sol = 10, Tierra = 399; sitio en coordenadas geodéticas; cantidad 4, acimut y elevación). Compara con esta línea:</p>
      <p id="valLine" class="terrain-note">—</p>
      <p class="terrain-note">Límites: el horizonte se calcula desde las coordenadas del escenario solo si caen dentro del recorte LOLA elegido (si no, desde su centro) y asume proyección polar estereográfica con esfera de 1737,4 km. Horizons: JPL no permite leer su API desde páginas web; si el botón no logra comparar, abre los enlaces que muestra. Todavía no hay una comparación numérica guardada.</p>
    </section>
  </main>

  <script>
const D = Math.PI / 180;
const R = 1737.4;
const AU = 149597870.7;
const sin = x => Math.sin(x * D);
const cos = x => Math.cos(x * D);
const mod = x => ((x % 360) + 360) % 360;
const LT = [
  [0, 0, 1, 0, 6288774],
  [2, 0, -1, 0, 1274027],
  [2, 0, 0, 0, 658314],
  [0, 0, 2, 0, 213618],
  [0, 1, 0, 0, -185116],
  [0, 0, 0, 2, -114332],
  [2, 0, -2, 0, 58793],
  [2, -1, -1, 0, 57066],
  [2, 0, 1, 0, 53322],
  [2, -1, 0, 0, 45758],
  [0, 1, -1, 0, -40923],
  [1, 0, 0, 0, -34720],
  [0, 1, 1, 0, -30383],
  [2, 0, 0, -2, 15327],
  [0, 0, 1, 2, -12528],
  [0, 0, 1, -2, 10980]
];
const BT = [
  [0, 0, 0, 1, 5128122],
  [0, 0, 1, 1, 280602],
  [0, 0, 1, -1, 277693],
  [2, 0, 0, -1, 173237],
  [2, 0, -1, 1, 55413],
  [2, 0, -1, -1, 46271],
  [2, 0, 0, 1, 32573],
  [0, 0, 2, 1, 17198]
];
const RT = [
  [0, 0, 1, 0, -20905355],
  [2, 0, -1, 0, -3699111],
  [2, 0, 0, 0, -2955968],
  [0, 0, 2, 0, -569925],
  [0, 1, 0, 0, 48888],
  [0, 0, 0, 2, -3149],
  [2, 0, -2, 0, 246158],
  [2, -1, -1, 0, -152138],
  [2, 0, 1, 0, -170733],
  [2, -1, 0, 0, -204586],
  [0, 1, -1, 0, -129620],
  [1, 0, 0, 0, 108743],
  [0, 1, 1, 0, 104755]
];
const vec = (l, b) => [cos(b) * cos(l), cos(b) * sin(l), sin(b)];

function geo(jd) {
  const T = (jd - 2451545) / 36525;
  const Lp = mod(218.3164477 + 481267.88123421 * T);
  const Dd = mod(297.8501921 + 445267.1114034 * T);
  const M = mod(357.5291092 + 35999.0502909 * T);
  const Mp = mod(134.9633964 + 477198.8675055 * T);
  const F = mod(93.272095 + 483202.0175233 * T);
  const Om = mod(125.0445479 - 1934.1362891 * T);
  const sm = (t, f) => t.reduce((a, r) => a + r[4] * f(r[0] * Dd + r[1] * M + r[2] * Mp + r[3] * F), 0);
  const lam = Lp + sm(LT, sin) / 1e6;
  const bet = sm(BT, sin) / 1e6;
  const dist = 385000.56 + sm(RT, cos) / 1e3;
  const Ms = mod(357.52911 + 35999.05029 * T);
  const C = (1.914602 - 0.004817 * T) * sin(Ms) + 0.019993 * sin(2 * Ms) + 0.000289 * sin(3 * Ms);
  const ls = 280.46646 + 36000.76983 * T + C;
  const ec = 0.016708634 - 0.000042037 * T;
  const Rs = 1.000001018 * (1 - ec * ec) / (1 + ec * cos(Ms + C)) * AU;
  const lib = (l, b) => {
    const W = l - Om;
    const I = 1.54242;
    const A = Math.atan2(sin(W) * cos(b) * cos(I) - sin(b) * sin(I), cos(W) * cos(b)) / D;
    let ll = mod(A - F);
    if (ll > 180) {
      ll -= 360;
    }
    return [ll, Math.asin(-sin(W) * cos(b) * sin(I) - sin(b) * cos(I)) / D];
  };
  const k = dist / Rs;
  const e = lib(lam, bet);
  const s = lib(ls + 180 + k * 57.2958 * cos(bet) * sin(ls - lam), k * bet);
  return {
    e: vec(e[0], e[1]), s: vec(s[0], s[1]), d: dist
  };
}

function look(la, lo, v, d) {
  const n = [cos(la) * cos(lo), cos(la) * sin(lo), sin(la)];
  const E = [-sin(lo), cos(lo), 0];
  const N = [-sin(la) * cos(lo), -sin(la) * sin(lo), cos(la)];
  let w = d ? v.map((x, i) => x * d - R * n[i]) : v;
  const m = Math.hypot(...w);
  w = w.map(x => x / m);
  const dt = (a, b) => a[0] * b[0] + a[1] * b[1] + a[2] * b[2];
  return [Math.asin(dt(w, n)) / D, mod(Math.atan2(dt(w, E), dt(w, N)) / D)];
}

let lang = "en";
const I = {
  title: ["Selene: compara sitios de aterrizaje en la Luna", "Selene: compare Moon landing sites"],
  lead: [
    "Evalúa un sitio y una fecha de aterrizaje lunar: dónde están el Sol y la Tierra respecto al horizonte, cuánto dura la luz y cuándo hay comunicación directa con la Tierra. Si quieres, añade un segundo escenario para compararlos.", "Evaluate a Moon landing site and date: where the Sun and Earth sit relative to the horizon, how long sunlight lasts and when there is a direct line to Earth. If you like, add a second scenario to compare them."
  ],
  lang: ["Idioma", "Language"],
  theme: ["Tema", "Theme"],
  th_night: ["Noche lunar", "Lunar night"],
  th_nebula: ["Nebulosa", "Nebula"],
  th_aurora: ["Aurora polar", "Polar aurora"],
  addb: ["Añadir escenario B para comparar", "Add scenario B to compare"],
  rmb: ["Quitar escenario B", "Remove scenario B"],
  chh: ["Altura a lo largo del tiempo", "Height over time"],
  axx: ["Fecha (UTC, mes-día)", "Date (UTC, month-day)"],
  axy: ["Altura (°)", "Height (°)"],
  hzl: ["Tu horizonte", "Your horizon"],
  flat: ["Horizonte plano (0°)", "Flat horizon (0°)"],
  bothl: ["Ambos", "Both"],
  lgvis: ["Zona coloreada: visible", "Shaded: visible"],
  lgcur: ["Instante elegido", "Chosen moment"],
  read: [
    "Cómo leerla: cuando una curva sube por encima de la línea punteada, ese astro es visible (Sol: energía; Tierra: comunicación). Las barras de abajo resumen esas ventanas. Arrastra sobre la gráfica para mover el tiempo.", "How to read it: when a curve rises above the dashed line, that body is visible (Sun: power; Earth: communication). The bars below summarize those windows. Drag across the chart to move time."
  ],
  dm: ["Vista 3D del cielo", "3D sky view"], dmcap: [
    "Arrastra para girar o usa los botones de vista (doble toque restablece). Las líneas son el recorrido del Sol y la Tierra desde 12 h antes hasta 60 h después; lo tenue está bajo el horizonte. La sombra del módulo se alarga cuando el Sol está bajo. Alturas ampliadas ×{x} para que se vean; los anillos marcan grados reales.", "Drag to rotate or use the view buttons (double-click resets). Lines show the Sun and Earth paths from 12 h before to 60 h after; faded parts are below the horizon. The lander’s shadow stretches when the Sun is low. Heights are magnified ×{x} so they are visible; the rings mark real degrees."
  ], vtop: ["Desde arriba", "Top view"], vside: ["De lado", "Side view"], vreset: ["Restablecer", "Reset"], lnd: ["Módulo", "Lander"],
  g9: [
    "<b>Gráfica de altura:</b> el eje vertical es la altura en grados (0° es el horizonte) y el horizontal es la fecha UTC. Las zonas coloreadas bajo cada curva son los momentos en que el astro es visible.", "<b>Height chart:</b> the vertical axis is height in degrees (0° is the horizon) and the horizontal axis is the UTC date. The shaded areas under each curve are when the body is visible."
  ],
  g10: [
    "<b>Ventanas (barras):</b> tres filas con los periodos de Sol, de Tierra y de ambos a la vez. Los huecos vacíos son noche o falta de comunicación.", "<b>Windows (bars):</b> three rows with the periods of Sun, Earth and both together. Empty gaps are night or loss of communication."
  ],
  g11: [
    "<b>Vista 3D:</b> el círculo gris es el suelo y las letras son los puntos cardinales. Gira la vista arrastrando. La Tierra sube y baja en ondas por la libración de la Luna.", "<b>3D view:</b> the grey disc is the ground and the letters are compass points. Rotate by dragging. Earth rises and falls in waves because of the Moon’s libration."
  ], share: ["Copiar enlace", "Copy link"], copied: ["¡Enlace copiado!", "Link copied!"], csv: ["Descargar datos (CSV)", "Download data (CSV)"], win: ["Ventana de tiempo", "Time window"], w15: ["15 días", "15 days"], w30: ["30 días (1 día lunar)", "30 days (1 lunar day)"], w90: ["90 días", "90 days"], w365: ["1 año", "1 year"],
  play: ["Reproducir", "Play"], pause: ["Pausar", "Pause"], why: ["¿Por qué importa?", "Why it matters"], ml: ["Duración de la misión", "Mission length"], d: ["días", "days"], time: ["Tiempo", "Time"],
  whyt: [
    "<b>Mismo sitio, misma fecha.</b> La única diferencia es un relieve de solo 2° en el horizonte del escenario B. En el borde de Shackleton el Sol nunca supera 1,5° sobre el horizonte plano, así que ese pequeño obstáculo basta para dejar el sitio sin luz solar directa. Por eso unos metros de ubicación o un día de fecha pueden cambiar toda la misión.", "<b>Same site, same date.</b> The only difference is a 2° obstruction on the horizon in scenario B. On the Shackleton rim the Sun never rises above 1.5° over a flat horizon, so that small obstacle is enough to leave the site without direct sunlight. That is why a few meters of location or one day of date can change an entire mission."
  ],
  gt: ["¿Cómo leer los datos? Toca aquí", "How to read the data: tap here"],
  g1: [
    "<b>Altura (elevación):</b> el ángulo de un astro sobre el horizonte. 0° es justo en el horizonte; si es negativa, está oculto bajo el horizonte.", "<b>Height (elevation):</b> the angle of a body above the horizon. 0° is right at the horizon; if negative, it is hidden below it."
  ],
  g2: [
    "<b>Energía solar:</b> cuando el Sol está sobre el horizonte, los paneles generan energía. Cuanto mayor el % de luz y la altura del Sol, mejor.", "<b>Solar power:</b> when the Sun is above the horizon, the panels generate power. The higher the % of light and the Sun height, the better."
  ],
  g3: [
    "<b>Noche más larga:</b> el periodo seguido más largo sin Sol. Indica cuánta batería o calor haría falta para sobrevivirla.", "<b>Longest night:</b> the longest continuous period without Sun. It shows how much battery or heat is needed to survive it."
  ],
  g4: [
    "<b>Comunicación:</b> si la Tierra está sobre el horizonte hay línea directa, sin satélite repetidor. Se muestra el % del tiempo con la Tierra visible.", "<b>Communication:</b> if Earth is above the horizon there is a direct line, with no relay satellite. The % of time with Earth in view is shown."
  ],
  g5: [
    "<b>Se pone en / sale en:</b> tiempo que falta para el próximo cambio (puesta o salida). Es la cuenta atrás operativa.", "<b>Sets in / rises in:</b> time left until the next change (set or rise). It is the operational countdown."
  ],
  g6: [
    "<b>Sol y Tierra a la vez:</b> el tiempo con energía y comunicación juntas, que es cuando la misión puede operar con normalidad.", "<b>Sun and Earth together:</b> the time with power and communication at once, when the mission can operate normally."
  ],
  g7: [
    "<b>Obstáculo del relieve:</b> consulta en Moon Trek la herramienta Calculate Elevation Profile. Para cada punto del perfil, calcula θ = atan((altura del terreno − altura del sitio) / distancia) × 180/π y usa el mayor ángulo positivo como referencia. Selene todavía requiere introducir ese ángulo manualmente.", "<b>Terrain obstruction:</b> use Calculate Elevation Profile in Moon Trek. For each profile point, calculate θ = atan((terrain height − site height) / distance) × 180/π, then use the largest positive angle as a reference. Selene still requires entering that angle manually."
  ],
  g8: [
    "<b>Veredicto:</b> semáforo orientativo. Verde si hay buena luz y comunicación, ámbar si hay limitaciones, rojo si es difícil.", "<b>Verdict:</b> a rough traffic light. Green if light and communication are good, amber if limited, red if difficult."
  ],
  sc: ["Escenario", "Scenario"], site: ["Sitio", "Site"], lat: ["Latitud (°)", "Latitude (°)"], lon: ["Longitud este (°)", "East longitude (°)"], date: ["Fecha de aterrizaje (UTC)", "Landing date (UTC)"], hz: ["Obstáculo del relieve (°)", "Terrain obstruction (°)"], shift: ["mover la fecha", "shift the date"],
  v_ok: ["Favorable", "Favorable"], v_wa: ["Con limitaciones", "Limited"], v_no: ["Difícil", "Difficult"],
  gp1: ["Energía solar", "Solar power"], gp2: ["Comunicación con la Tierra", "Communication with Earth"], gp3: ["Los dos a la vez", "Both at once"],
  sunnow: ["Altura del Sol ahora", "Sun height now"], tsun: [
    "Ángulo del Sol sobre el horizonte. Si es negativo, está oculto.", "Angle of the Sun above the horizon. If negative, it is hidden."
  ],
  sunpct: ["Tiempo con luz solar", "Time in sunlight"], tsunpct: [
    "Porcentaje de la ventana con el Sol sobre tu horizonte. Más luz, más energía.", "Share of the window with the Sun above your horizon. More light, more power."
  ],
  night: ["Noche más larga", "Longest night"], tnight: ["El periodo seguido más largo sin Sol.", "The longest continuous period without Sun."],
  sunmax: ["Altura solar máxima", "Highest Sun"], tsunmax: ["La mayor altura del Sol en la ventana.", "The greatest Sun height in the window."],
  earnow: ["Altura de la Tierra ahora", "Earth height now"], tear: [
    "Ángulo de la Tierra sobre el horizonte. Si es negativo, no se ve.", "Angle of Earth above the horizon. If negative, it is not visible."
  ],
  earpct: ["Tierra visible (permite comunicar)", "Earth in view (allows contact)"], tearpct: [
    "Porcentaje de la ventana con línea directa a la Tierra.", "Share of the window with a direct line to Earth."
  ],
  both: ["Sol y Tierra a la vez", "Sun and Earth together"], tboth: ["Tiempo con energía y comunicación al mismo tiempo.", "Time with power and communication together."],
  st1: ["Visible. Se pone en {x}", "Visible. Sets in {x}"], st2: ["Visible. No se pone en los próximos {x}", "Visible. Will not set in the next {x}"], st3: ["Oculto. Sale en {x}", "Hidden. Rises in {x}"], st4: ["Oculto. No sale en los próximos {x}", "Hidden. Will not rise in the next {x}"],
  capsky: [
    "Cielo en el instante elegido (acimut 0–360°, N a la izquierda).", "Sky at the chosen moment (azimuth 0–360°, N on the left)."
  ], sun: ["Sol", "Sun"], earth: ["Tierra", "Earth"],
  capch: [
    "Altura a lo largo de la ventana. La banda amarilla marca la luz solar; la línea punteada es tu horizonte.", "Height over the window. The yellow band marks sunlight; the dashed line is your horizon."
  ],
  capv: [
    "Veredicto orientativo: favorable si hay luz ≥ 70 % y Tierra visible ≥ 50 % del tiempo.", "Rough verdict: favorable if sunlight ≥ 70 % and Earth in view ≥ 50 % of the time."
  ],
  a1: ["Altura del Sol y de la Tierra en el tiempo", "Height of the Sun and Earth over time"], a2: [
    "Posición del Sol y la Tierra sobre el horizonte", "Position of the Sun and Earth above the horizon"
  ], a3: ["Calidad de la misión según la fecha de inicio", "Mission quality by start date"],
  fh: ["Si la fecha de aterrizaje se mueve", "If the landing date moves"],
  fcap: [
    "Calidad media (luz solar y visibilidad de la Tierra) de una misión de {L} días que empieza cada día, desde 30 días antes hasta 60 días después de tu fecha. Rojo es malo, verde es bueno; la línea marca tu fecha.", "Average quality (sunlight and Earth visibility) of a {L}-day mission starting each day, from 30 days before to 60 days after your date. Red is bad, green is good; the line marks your date."
  ],
  fbest: ["Mejores fechas alternativas (toca para usarlas):", "Best alternative dates (tap to use them):"], fbtn: ["{d}: luz {a} %, Tierra {b} %", "{d}: sunlight {a} %, Earth {b} %"]
};
const t = k => I[k][lang == "es" ? 0 : 1];
const SITES = [
  [
    ["Polo Sur de la Luna", "Lunar South Pole"],
    -90, 0
  ],
  [
    ["Cráter Shackleton (borde)", "Shackleton Crater (rim)"],
    -89.9, 129.8
  ],
  [
    ["Cresta junto a Shackleton (zona Artemis)", "Ridge near Shackleton (Artemis area)"],
    -89.45, -137.6
  ],
  [
    ["Monte Malapert", "Malapert Mountain"],
    -85.9, 0
  ],
  [
    ["Mons Mouton (misión IM-2)", "Mons Mouton (IM-2 mission)"],
    -84.8, 29.2
  ],
  [
    ["Luna-25 (Rusia)", "Luna-25 (Russia)"],
    -69.5, 43.5
  ],
  [
    ["Chandrayaan-3 (India)", "Chandrayaan-3 (India)"],
    -69.37, 32.32
  ],
  [
    ["Apolo 11 (1969)", "Apollo 11 (1969)"],
    0.67,
    23.47
  ],
  [
    ["Apolo 17 (1972)", "Apollo 17 (1972)"],
    20.19, 30.77
  ],
  [
    ["Escribir mis coordenadas", "Enter my own coordinates"],
    -85, 0
  ]
];
const $ = s => document.querySelector(s);
// Horizonte real: perfil de elevación (grados) por acimut, calculado desde el DEM LOLA. Solo escenario A.
let HZ = [null, null];
let HZID = ["", ""];
const hzAt = (az, i) => HZ[i] ? HZ[i][Math.round(((az % 360) + 360) % 360) % 360] : 0;
window.seleneSetHorizon = (arr, id, i) => { HZ[i] = arr; HZID[i] = id; cache = {}; render(); };
window.seleneScenario = i => S[i];
const nf = (x, d = 1) => {
  const s = x.toFixed(d);
  return lang == "es" ? s.replace(".", ",") : s;
};
const pc = x => x.toFixed(0) + (lang == "es" ? " %" : "%");
const S = [
  {
    si: 1, date: "2027-03-01", h: 0
  },
  {
    si: 3, date: "2027-03-01", h: 0
  }
].map(c => ({
  ...c, lat: SITES[c.si][1], lon: SITES[c.si][2]
}));
let theme = "night";
let starTint = "#ffffff";
let days = 30;
let k = 0;
let mlv = 14;
let whyOn = false;
let two = false;
let cache = {};
let timer = null;
const act = () => two ? S : [S[0]];
const step = () => days > 40 ? 6 : 2;
const NN = () => days * 24 / step();

function ui() {
  document.documentElement.lang = lang;
  document.documentElement.dataset.theme = theme;
  starTint = getComputedStyle(document.documentElement).getPropertyValue("--star").trim() || "#ffffff";
  document.title = t("title");
  $("#top").innerHTML = `<svg class="art" viewBox="0 0 1100 230" preserveAspectRatio="xMidYMax slice" aria-hidden="true"><defs><radialGradient id="eg" cx="35%" cy="30%"><stop offset="0" stop-color="#9fd2ff"/><stop offset=".55" stop-color="#2f78d0"/><stop offset="1" stop-color="#0d2c66"/></radialGradient><linearGradient id="mg" x1="0" y1="0" x2="0" y2="1"><stop offset="0" stop-color="#6f7787"/><stop offset=".25" stop-color="#2a303b"/><stop offset="1" stop-color="#0a0d13"/></linearGradient><radialGradient id="sg"><stop offset="0" stop-color="#fff3c9"/><stop offset=".25" stop-color="#ffb938" stop-opacity=".7"/><stop offset="1" stop-color="#ffb938" stop-opacity="0"/></radialGradient></defs><circle cx="170" cy="168" r="120" fill="url(#sg)"/><g class="rise"><circle cx="860" cy="92" r="46" fill="#4fa8ff" opacity=".12"/><circle cx="860" cy="92" r="30" fill="url(#eg)"/><ellipse cx="852" cy="84" rx="14" ry="3.5" fill="#fff" opacity=".35"/><ellipse cx="868" cy="100" rx="11" ry="3" fill="#fff" opacity=".3"/></g><path d="M0 196Q550 140 1100 190L1100 230L0 230Z" fill="url(#mg)"/><path d="M0 196Q550 140 1100 190" fill="none" stroke="#c9d3e6" stroke-opacity=".5"/><ellipse cx="300" cy="214" rx="46" ry="6" fill="#000" opacity=".35"/><ellipse cx="640" cy="200" rx="64" ry="7" fill="#000" opacity=".3"/><ellipse cx="930" cy="212" rx="38" ry="5" fill="#000" opacity=".35"/></svg><div class="ht"><h1>Selene</h1><p class="lead">${t("lead")}</p></div><div class="lg"><label><span class="sr">${t("theme")}</span><select id="theme">${["night", "nebula", "aurora"].map(v => `<option value="${v}"${v == theme ? " selected" : ""}>${t("th_" + v)}</option>`).join("")}</select></label><label><span class="sr">${t("lang")}</span><select id="lang"><option value="es"${lang == "es" ? " selected" : ""}>Español</option><option value="en"${lang == "en" ? " selected" : ""}>English</option></select></label></div>`;
  $("#bar").innerHTML = `<label>${t("win")} <select id="days">${[
    [15, "w15"],
    [30, "w30"],
    [90, "w90"],
    [365, "w365"]
  ].map(([v, q]) => `<option value="${v}"${v == days ? " selected" : ""}>${t(q)}</option>`).join("")}</select></label><button id="play" type="button">${t(timer ? "pause" : "play")}</button><button id="why" type="button">${t("why")}</button><button id="addb" type="button">${t(two ? "rmb" : "addb")}</button><button id="horizons" type="button">${lang == "es" ? "Abrir NASA JPL Horizons" : "Open NASA JPL Horizons"}</button><button id="share" type="button">${t("share")}</button><button id="csv" type="button">${t("csv")}</button><label>${t("ml")} <select id="ml">${[7, 14, 28].map(v => `<option value="${v}"${v == mlv ? " selected" : ""}>${v} ${t("d")}</option>`).join("")}</select></label><label class="sr" for="t">${t("time")}</label><input id="t" type="range" min="0" max="${NN()}" value="${k}"><span id="when"></span>`;
  $("#guide").innerHTML = `<details><summary>${t("gt")}</summary><ul>${[1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11].map(q => `<li>${t("g" + q)}</li>`).join("")}</ul></details>`;
  $("#whyt").innerHTML = t("whyt");
  $("#whyt").hidden = !whyOn;
}

function buildCards() {
  $("#cards").className = two ? "" : "one";
  $("#cards").innerHTML = act().map((c, i) => `<section class="card" id="c${i}"><h2>${t("sc")} ${"AB"[i]}</h2><div class="row">
<label class="w">${t("site")}<select data-i="${i}" data-f="si">${SITES.map((s, j) => `<option value="${j}"${j == c.si ? " selected" : ""}>${s[0][lang == "es" ? 0 : 1]}</option>`).join("")}</select></label>
<label>${t("lat")}<input type="number" step="0.1" min="-90" max="90" data-i="${i}" data-f="lat" value="${c.lat}"></label>
<label>${t("lon")}<input type="number" step="0.1" min="-180" max="360" data-i="${i}" data-f="lon" value="${c.lon}"></label>
<label>${t("date")}<input type="date" data-i="${i}" data-f="date" value="${c.date}"></label>
<label>${t("hz")} <output>${c.h}°</output><input type="range" min="0" max="10" step="0.5" value="${c.h}" data-i="${i}" data-f="h"></label></div>
<div class="sh">${[-7, -1, 1, 7].map(n => `<button type="button" data-i="${i}" data-sh="${n}">${n > 0 ? "+" : "−"}${Math.abs(n)} ${lang == "es" ? "d" : "d"}</button>`).join("")}<span class="cap" style="margin:0">${t("shift")}</span></div><div class="out"></div><div class="fin"></div></section>`).join("");
}

function calc(c) {
  const key = [c.lat, c.lon, c.date, days, HZID[c.id] || ""].join();
  if (cache[c.id] && cache[c.id].key == key) {
    return cache[c.id];
  }
  const j0 = Date.parse(c.date + "T00:00:00Z") / 864e5 + 2440587.5;
  const st = step();
  const o = {
    key, s: [], e: [], sa: [], ea: [], j0
  };
  for (let q = 0; q <= NN(); q++) {
    const g = geo(j0 + q * st / 24);
    const a = look(c.lat, c.lon, g.s, 0);
    const b = look(c.lat, c.lon, g.e, g.d);
    const hs = hzAt(a[1], c.id);
    const he = hzAt(b[1], c.id);
    o.s.push(a[0] - hs);
    o.sa.push(a[1]);
    o.e.push(b[0] - he);
    o.ea.push(b[1]);
  }
  return cache[c.id] = o;
}

const fmt = h => h < 48 ? Math.round(h) + " h" : nf(h / 24, 1) + " d";

function stats(a, h) {
  let up = 0;
  let dk = 0;
  let best = 0;
  for (const x of a) {
    if (x > h) {
      up++;
      dk = 0;
    }
    else {
      dk++;
      best = Math.max(best, dk);
    }
  }
  return { p: 100 * up / a.length, n: best * step() };
}

function stt(a, q, h) {
  const u = a[q] > h;
  let j = q;
  while (j < a.length && (a[j] > h) === u) {
    j++;
  }
  const e = j >= a.length;
  return t(u ? (e ? "st2" : "st1") : (e ? "st4" : "st3")).replace("{x}", fmt((j - q) * step()));
}

function bands(a, h, W, N) {
  let r = "";
  let q = 0;
  while (q <= N) {
    if (a[q] > h) {
      let j = q;
      while (j <= N && a[j] > h) {
        j++;
      }
      r += `<rect x="${q / N * W}" y="0" width="${Math.max(1, (j - q) / N * W)}" height="140" fill="var(--day)"/>`;
      q = j;
    }
    else {
      q++;
    }
  }
  return r;
}

const tile = (v, l, tp, s) => `<div class="t" title="${tp}"><b>${v}</b><span>${l}</span>${s ? `<em>${s}</em>` : ""}</div>`;
const grp = (h, a, c) => `<p class="g">${h}</p><div class="tiles ${c}">${a.join("")}</div>`;
const STARS = Array.from({ length: 46 }, (_, q) => {
  const a = Math.sin(q * 91.7) * 43758.5;
  const b = Math.sin(q * 12.9 + 4) * 23421.6;
  return `<circle cx="${((a - Math.floor(a)) * 600).toFixed(0)}" cy="${((b - Math.floor(b)) * 100).toFixed(0)}" r="${q % 5 ? 0.7 : 1.2}" fill="#fff" opacity="${(0.3 + (q % 4) * 0.15).toFixed(2)}"/>`;
}).join("");

function nextHorizonEvent(values, startIndex, horizon) {
  const isAbove = values[startIndex] > horizon;
  for (let q = startIndex; q < values.length - 1; q++) {
    const a = values[q] - horizon;
    const b = values[q + 1] - horizon;
    if ((a > 0) !== (b > 0)) {
      const fraction = Math.max(0, Math.min(1, (0 - a) / (b - a)));
      const hours = ((q - startIndex) + fraction) * step();
      const eventIndex = q + fraction;
      const label = isAbove ? (lang == "es" ? "Puesta" : "Set") : (lang == "es" ? "Salida" : "Rise");
      return { label, hours, at: eventIndex, isAbove };
    }
  }
  return null;
}
function formatCountdown(hours) {
  const total = Math.max(0, Math.round(hours * 60));
  const daysLeft = Math.floor(total / 1440);
  const hrs = Math.floor((total % 1440) / 60);
  const mins = total % 60;
  if (daysLeft) return `${daysLeft} d ${hrs} h ${mins} min`;
  if (hrs) return `${hrs} h ${mins} min`;
  return `${mins} min`;
}
function countdownCard(values, c, startIndex, horizon, bodyName) {
  const event = nextHorizonEvent(values, startIndex, horizon);
  if (!event) return `<div class="countdown-item"><b>${bodyName}: ${lang == "es" ? "sin cruce en la ventana" : "no crossing in window"}</b><span>${lang == "es" ? "Amplía el periodo para buscar la próxima salida o puesta." : "Extend the time window to find the next rise or set."}</span></div>`;
  const base = Date.parse(c.date + "T00:00:00Z");
  const at = new Date(base + event.at * step() * 3600000);
  const stamp = at.toISOString().slice(0,16).replace("T", " ") + " UTC";
  return `<div class="countdown-item"><b>${bodyName}: ${event.label.toLowerCase()} en ${formatCountdown(event.hours)}</b><span>${lang == "es" ? "Hora estimada" : "Estimated time"}: ${stamp}</span></div>`;
}
function updateTerrainCoordinates() {
  const el = document.querySelector("#terrainCoordinates");
  if (!el || !S[0]) return;
  const a = `${S[0].lat.toFixed(3)}°, ${mod(S[0].lon).toFixed(3)}° E`;
  const b = two && S[1] ? ` · B: ${S[1].lat.toFixed(3)}°, ${mod(S[1].lon).toFixed(3)}° E` : "";
  el.textContent = `${lang == "es" ? "Coordenadas seleccionadas" : "Selected coordinates"}: A ${a}${b}`;
  if (window.seleneUpdateMap) window.seleneUpdateMap(S[0].lat, S[0].lon);
}

function render() {
  updateTerrainCoordinates();
  $("#when").textContent = new Date(Date.parse(S[0].date + "T00:00:00Z") + k * step() * 36e5).toISOString().slice(0, 16).replace("T", " ") + " UTC (A)";
  act().forEach((c, i) => {
    c.id = i;
    const d = calc(c);
    const h = c.h;
    const sx = stats(d.s, h);
    const ex = stats(d.e, h);
    let both = 0;
    d.s.forEach((x, q) => {
      if (x > h && d.e[q] > h) {
        both++;
      }
    });
    both = 100 * both / d.s.length;
    const mx = Math.max(...d.s);
    const vd = sx.p >= 70 && ex.p >= 50 ? ["ok", "v_ok"] : sx.p >= 40 && ex.p >= 25 ? ["wa", "v_wa"] : ["no", "v_no"];
    const N = NN();
    const hi = Math.max(8, Math.ceil(h + 1), Math.ceil(Math.max(...d.s, ...d.e) + 1));
    const lo = Math.min(-3, Math.floor(Math.min(...d.s, ...d.e) - 1));
    const ML = 46;
    const PW = 584;
    const PT = 12;
    const PH = 148;
    const tx = q => ML + q / N * PW;
    const Yc = e => PT + PH - (e - lo) / (hi - lo) * PH;
    const st0 = [1, 2, 5, 10, 15, 30, 45, 90].find(s => (hi - lo) / s <= 6) || 90;
    const yh = Yc(h);
    const ms0 = (d.j0 - 2440587.5) * 864e5;
    let gy = "";
    let gx = "";
    for (let v = Math.ceil(lo / st0) * st0; v <= hi; v += st0) {
      gy += `<line x1="${ML}" x2="${ML + PW}" y1="${Yc(v)}" y2="${Yc(v)}" stroke="#16213a"/><text x="${ML - 6}" y="${Yc(v) + 3.5}" text-anchor="end" font-size="10.5" fill="var(--mut)">${v}°</text>`;
    }
    for (let q = 0; q <= 5; q++) {
      const qq = Math.round(q * N / 5);
      gx += `<line x1="${tx(qq)}" x2="${tx(qq)}" y1="${PT}" y2="${PT + PH}" stroke="#16213a"/><text x="${tx(qq)}" y="${PT + PH + 14}" text-anchor="${q == 0 ? "start" : q == 5 ? "end" : "middle"}" font-size="10.5" fill="var(--mut)">${new Date(ms0 + qq * step() * 36e5).toISOString().slice(5, 10)}</text>`;
    }
    const pts = a => a.map((e, q) => tx(q).toFixed(1) + "," + Yc(e).toFixed(1)).join(" ");
    const area = a => `<polygon clip-path="url(#cl${i})" points="${tx(0)},${yh} ${pts(a)} ${tx(N)},${yh}"/>`;
    const rw = (f, y, col, lab) => {
      let o = `<text x="${ML - 6}" y="${y + 10}" text-anchor="end" font-size="11" fill="var(--mut)">${lab}</text><rect x="${ML}" y="${y}" width="${PW}" height="12" rx="3" fill="#0c1422"/>`;
      let q = 0;
      while (q <= N) {
        if (f(q)) {
          let j = q;
          while (j <= N && f(j)) {
            j++;
          }
          o += `<rect x="${tx(q).toFixed(1)}" y="${y}" width="${Math.max(2, (j - q) / N * PW).toFixed(1)}" height="12" rx="3" fill="${col}"/>`;
          q = j;
        }
        else {
          q++;
        }
      }
      return o;
    };
    const cxk = tx(k);
    const an = cxk > ML + PW * 0.7 ? "end" : "start";
    const dx = an == "end" ? -8 : 8;
    const lb = (s, col, y) => `<text x="${cxk + dx}" y="${y}" text-anchor="${an}" font-size="11" font-weight="700" fill="${col}" stroke="#070b13" stroke-width="3" paint-order="stroke">${s}</text>`;
    const chart = `<svg class="ch" data-ch="${i}" viewBox="0 0 640 252" role="img" aria-label="${t("a1")}"><defs><clipPath id="cl${i}"><rect x="0" y="0" width="640" height="${yh}"/></clipPath></defs>${gy}${gx}
<g fill="var(--sun)" fill-opacity=".24">${area(d.s)}</g><g fill="var(--ea)" fill-opacity=".22">${area(d.e)}</g>
<line x1="${ML}" x2="${ML + PW}" y1="${yh}" y2="${yh}" stroke="var(--fg)" stroke-opacity=".7" stroke-dasharray="5 4"/><text x="${ML + PW - 4}" y="${yh - 5}" text-anchor="end" font-size="10.5" fill="var(--fg)" fill-opacity=".85">${h > 0 ? t("hzl") + " (" + nf(h) + "°)" : t("flat")}</text>
<polyline fill="none" stroke="var(--sun)" stroke-width="2.3" points="${pts(d.s)}"/><polyline fill="none" stroke="var(--ea)" stroke-width="2.3" points="${pts(d.e)}"/>
<text x="${ML + PW / 2}" y="${PT + PH + 28}" text-anchor="middle" font-size="10.5" fill="var(--mut)">${t("axx")}</text><text transform="rotate(-90 11 ${PT + PH / 2})" x="11" y="${PT + PH / 2}" text-anchor="middle" font-size="10.5" fill="var(--mut)">${t("axy")}</text>
${rw(q => d.s[q] > h, 198, "var(--sun)", t("sun"))}${rw(q => d.e[q] > h, 214, "var(--ea)", t("earth"))}${rw(q => d.s[q] > h && d.e[q] > h, 230, "var(--ok)", t("bothl"))}
<line x1="${cxk}" x2="${cxk}" y1="${PT}" y2="242" stroke="var(--fg)" stroke-opacity=".85"/><circle cx="${cxk}" cy="${Yc(d.s[k])}" r="4" fill="var(--sun)" stroke="#fff"/><circle cx="${cxk}" cy="${Yc(d.e[k])}" r="4" fill="var(--ea)" stroke="#fff"/>
${lb(new Date(ms0 + k * step() * 36e5).toISOString().slice(5, 16).replace("T", " "), "#e7ecf5", PT + 12)}${lb(t("sun") + " " + nf(d.s[k]) + "°", "var(--sun)", PT + 25)}${lb(t("earth") + " " + nf(d.e[k]) + "°", "var(--ea)", PT + 38)}</svg>`;
    const leg = `<div class="lgd"><span><i class="ln" style="background:var(--sun)"></i>${t("sun")}</span><span><i class="ln" style="background:var(--ea)"></i>${t("earth")}</span><span><i class="bx"></i>${t("lgvis")}</span><span><i class="ln dash"></i>${t("hzl")}</span><span><i class="cu"></i>${t("lgcur")}</span></div>`;
    const ph = Math.max(10, Math.ceil(mx + 2));
    const X = a => a / 360 * 600;
    const Yp = e => 120 - (e + 4) / (ph + 4) * 120;
    const dot = (az, el, r, col) => `<circle cx="${X(az)}" cy="${Yp(el)}" r="${r * 2.6}" fill="${col}" opacity="${el > h ? 0.2 : 0}"/><circle cx="${X(az)}" cy="${Yp(el)}" r="${r}" fill="${col}" opacity="${el > h ? 1 : 0.35}"/>`;
    const pano = `<svg viewBox="0 0 600 120" role="img" aria-label="${t("a2")}"><defs><linearGradient id="sk" x1="0" y1="0" x2="0" y2="1"><stop offset="0" stop-color="#01030a"/><stop offset="1" stop-color="#101c33"/></linearGradient><linearGradient id="gg" x1="0" y1="0" x2="0" y2="1"><stop offset="0" stop-color="#4a5262"/><stop offset="1" stop-color="#12161d"/></linearGradient></defs><rect width="600" height="120" fill="url(#sk)"/>${STARS}<rect y="${Yp(h)}" width="600" height="${120 - Yp(h)}" fill="url(#gg)"/><line x1="0" x2="600" y1="${Yp(h)}" y2="${Yp(h)}" stroke="var(--mut)"/>${[
      ["N", 0],
      ["E", 90],
      ["S", 180],
      [lang == "es" ? "O" : "W", 270]
    ].map(a => `<text x="${X(a[1]) + 4}" y="12" font-size="11" fill="var(--mut)">${a[0]}</text>`).join("")}${dot(d.ea[k], d.e[k], 9, "var(--ea)")}${dot(d.sa[k], d.s[k], 4.5, "var(--sun)")}</svg>`;
    document.querySelector("#c" + i + " .out").innerHTML = `<span class="v ${vd[0]}">${t(vd[1])}</span>
${grp('<i class="ic s"></i>' + t("gp1"), [
      tile(nf(d.s[k]) + "°", t("sunnow"), t("tsun"), stt(d.s, k, h)), tile(pc(sx.p), t("sunpct"), t("tsunpct")), tile(fmt(sx.n), t("night"), t("tnight")), tile(nf(mx) + "°", t("sunmax"), t("tsunmax"))
    ], "s")}
${grp('<i class="ic e"></i>' + t("gp2"), [tile(nf(d.e[k]) + "°", t("earnow"), t("tear"), stt(d.e, k, h)), tile(pc(ex.p), t("earpct"), t("tearpct"))], "e")}
${grp('<i class="ic b"></i>' + t("gp3"), [tile(pc(both), t("both"), t("tboth"))], "b")}
<h3 class="sec">${lang == "es" ? "Cuenta regresiva de horizonte" : "Horizon countdown"}</h3><div class="countdown-grid">${countdownCard(d.s, c, k, h, t("sun"))}${countdownCard(d.e, c, k, h, t("earth"))}</div>
<p class="cap">${t("capsky")} <span class="sw" style="background:var(--sun)"></span>${t("sun")} <span class="sw" style="background:var(--ea)"></span>${t("earth")}</p>${pano}
<h3 class="sec">${t("dm")}</h3><div class="dome" data-dome="${i}"></div><h3 class="sec">${t("chh")}</h3>${leg}${chart}<p class="cap">${t("read")}</p><p class="cap">${t("capv")}</p>`;
    finder(c, i);
    drawDome(i);
    if (i === 0 && $("#valLine")) {
      const rs = d.s[k] + hzAt(d.sa[k], 0), re_ = d.e[k] + hzAt(d.ea[k], 0);
      $("#valLine").textContent = `${c.date} +${k * step()} h UTC · lat ${c.lat}, lon ${c.lon} · Selene: Sol ${rs.toFixed(2)}° (az ${d.sa[k].toFixed(1)}°), Tierra ${re_.toFixed(2)}° (az ${d.ea[k].toFixed(1)}°)` + (HZ[0] ? ` · horizonte LOLA hacia el Sol: ${hzAt(d.sa[k], 0).toFixed(2)}°` : "");
    }
  });
}

// NASA JPL no permite incrustar su API directamente en sitios web.
// Por eso Selene mantiene sus cálculos locales y abre la interfaz oficial
// de Horizons para que los resultados puedan verificarse por separado.
function markHorizonsStale() {
  const status = document.querySelector("#horizonsStatus");
  if (status && status.textContent) {
    status.className = "";
    status.textContent = lang == "es"
      ? "Los parámetros cambiaron. Si estás verificando los resultados, vuelve a introducir las coordenadas y la fecha actualizadas en Horizons."
      : "The parameters changed. If you are checking the results, enter the updated coordinates and date in Horizons.";
  }
}

async function openHorizons(button) {
  const status = $("#horizonsStatus");
  const es = lang == "es";
  const c = S[0];
  c.id = 0;
  const d = calc(c);
  const n = Math.min(NN(), 24);
  const t0 = Date.parse(c.date + "T00:00:00Z");
  const ft = ms => new Date(ms).toISOString().slice(0, 16).replace("T", " ");
  const url = cmd => "https://ssd.jpl.nasa.gov/api/horizons.api?" + new URLSearchParams({
    format: "text", COMMAND: "'" + cmd + "'", OBJ_DATA: "'NO'", MAKE_EPHEM: "'YES'", EPHEM_TYPE: "'OBSERVER'",
    CENTER: "'coord@301'", COORD_TYPE: "'GEODETIC'", SITE_COORD: "'" + (((c.lon % 360) + 360) % 360) + "," + c.lat + ",0'",
    START_TIME: "'" + ft(t0) + "'", STOP_TIME: "'" + ft(t0 + n * step() * 36e5) + "'", STEP_SIZE: "'" + n + "'",
    QUANTITIES: "'4'", CSV_FORMAT: "'YES'", ANG_FORMAT: "'DEG'"
  });
  const parse = txt => (txt.split("$$SOE")[1] || "").split("$$EOE")[0].trim().split("\n").map(l => l.split(",").slice(1).map(x => x.trim()).filter(x => x !== "" && !isNaN(+x)).map(Number)).filter(r => r.length >= 2);
  status.className = "";
  status.textContent = es ? "Consultando JPL Horizons…" : "Querying JPL Horizons…";
  try {
    const out = [];
    for (const [cmd, nm, az, el] of [["10", t("sun"), d.sa, d.s], ["399", t("earth"), d.ea, d.e]]) {
      const rows = parse(await (await fetch(url(cmd))).text());
      if (!rows.length) throw new Error("sin datos");
      let me = 0, ma = 0;
      rows.forEach((r, q) => {
        if (q >= az.length) return;
        me = Math.max(me, Math.abs(r[1] - (el[q] + hzAt(az[q], 0))));
        ma = Math.max(ma, Math.abs(((r[0] - az[q] + 540) % 360) - 180));
      });
      out.push(`${nm}: ${es ? "diferencia máxima" : "max difference"} ${es ? "elevación" : "elevation"} ${me.toFixed(2)}°, ${es ? "acimut" : "azimuth"} ${ma.toFixed(2)}° (${rows.length} ${es ? "puntos" : "points"})`);
    }
    status.className = "ok";
    status.textContent = (es ? "Comparado con JPL Horizons (escenario A). " : "Compared with JPL Horizons (scenario A). ") + out.join(" · ");
  } catch (err) {
    status.className = "err";
    status.innerHTML = (es
      ? "El navegador no pudo leer la API de Horizons desde esta página (el servidor de JPL no envía permiso CORS, o no hay internet). No es un error de Selene. Abre estos enlaces y compara con la línea de validación: "
      : "The browser could not read the Horizons API from this page (JPL's server sends no CORS permission, or you are offline). It is not a Selene bug. Open these links and compare with the validation line: ")
      + `<a href="${url("10")}" target="_blank" rel="noopener noreferrer">${es ? "Sol" : "Sun"}</a> · <a href="${url("399")}" target="_blank" rel="noopener noreferrer">${es ? "Tierra" : "Earth"}</a>`;
  }
}

function hash() {
  return "th=" + theme + "&l=" + lang + "&d=" + days + "&m=" + mlv + act().map((c, i) => "&" + "AB"[i] + "=" + [c.si, c.lat, c.lon, c.date, c.h].join(",")).join("");
}

function load() {
  try {
    const p = new URLSearchParams(location.hash.slice(1));
    if (["night", "nebula", "aurora"].includes(p.get("th"))) {
      theme = p.get("th");
    }
    if (p.get("l")) {
      lang = p.get("l") == "en" ? "en" : "es";
    }
    if ([15, 30, 90, 365].includes(+p.get("d"))) {
      days = +p.get("d");
    }
    if ([7, 14, 28].includes(+p.get("m"))) {
      mlv = +p.get("m");
    }
    ["A", "B"].forEach((n, i) => {
      const v = (p.get(n) || "").split(",");
      if (v.length == 5 && ![v[0], v[1], v[2], v[4]].some(x => isNaN(+x)) && !isNaN(Date.parse(v[3]))) {
        const c = S[i];
        c.si = Math.max(0, Math.min(SITES.length - 1, +v[0]));
        c.lat = Math.max(-90, Math.min(90, +v[1]));
        c.lon = +v[2];
        c.date = v[3];
        c.h = Math.max(0, Math.min(10, +v[4]));
        if (i == 1) {
          two = true;
        }
      }
    });
  }
  catch (e) {
  }
}

function csv() {
  const es = lang == "es";
  const sp = es ? ";" : ",";
  const n = (x, d) => {
    const v = x.toFixed(d);
    return es ? v.replace(".", ",") : v;
  };
  const yn = b => b ? (es ? "Sí" : "Yes") : "No";
  const r = [(es ? [
      "Escenario", "Sitio", "Latitud (°)", "Longitud este (°)", "Fecha (UTC)", "Hora (UTC)", "Altura del Sol (°)", "Acimut del Sol (°)", "Altura de la Tierra (°)", "Acimut de la Tierra (°)", "Sol visible", "Tierra visible"
    ] : [
      "Scenario", "Site", "Latitude (°)", "East longitude (°)", "Date (UTC)", "Time (UTC)", "Sun height (°)", "Sun azimuth (°)", "Earth height (°)", "Earth azimuth (°)", "Sun visible", "Earth visible"
    ]).join(sp)];
  act().forEach((c, i) => {
    c.id = i;
    const d = calc(c);
    const nm = SITES[c.si][0][es ? 0 : 1].replace(/[;,]/g, " ");
    d.s.forEach((x, q) => {
      const iso = new Date((d.j0 - 2440587.5) * 864e5 + q * step() * 36e5).toISOString();
      r.push([
        "AB"[i], nm, n(c.lat, 2), n(c.lon, 2), iso.slice(0, 10), iso.slice(11, 16), n(x, 2), n(d.sa[q], 1), n(d.e[q], 2), n(d.ea[q], 1), yn(x > c.h), yn(d.e[q] > c.h)
      ].join(sp));
    });
  });
  const a = document.createElement("a");
  a.href = URL.createObjectURL(new Blob(["\ufeff" + r.join("\r\n")], { type: "text/csv;charset=utf-8" }));
  a.download = "selene.csv";
  a.click();
}

const V = [
  { y: 25, p: 52 },
  { y: 25, p: 52 }
];
let drag = null;
let scrub = null;
const DST = Array.from({ length: 70 }, (_, q) => {
  const a = Math.sin(q * 57.3 + 1) * 43758.5;
  const b = Math.sin(q * 31.1 + 7) * 23421.6;
  return `<circle cx="${((a - Math.floor(a)) * 420).toFixed(0)}" cy="${((b - Math.floor(b)) * 280).toFixed(0)}" r="${q % 6 ? 0.7 : 1.3}" fill="#fff" opacity="${(0.25 + (q % 4) * 0.15).toFixed(2)}"/>`;
}).join("");
const CRA = Array.from({ length: 16 }, (_, q) => {
  const a = Math.sin(q * 12.9 + 2) * 43758.5;
  const b = Math.sin(q * 78.2 + 5) * 23421.6;
  const e = Math.sin(q * 3.7 + 9) * 12345.6;
  const A = (a - Math.floor(a)) * 360;
  const Rd = Math.sqrt(b - Math.floor(b)) * 0.88;
  return [Rd * sin(A), Rd * cos(A), 0.03 + (e - Math.floor(e)) * 0.09];
});

function drawDome(i) {
  const el = document.querySelector(`[data-dome="${i}"]`);
  const c = S[i];
  const d = cache[i];
  if (!el || !d) {
    return;
  }
  const v = V[i];
  const h = c.h;
  const m = Math.max(...d.s.map(Math.abs), ...d.e.map(Math.abs));
  const ex = Math.max(1, Math.min(8, 45 / m));
  const cx = 210;
  const cy = 138;
  const Rr = 135;
  const Dc = 3.5;
  const P = (x, y, z) => {
    const a = x * cos(v.y) - y * sin(v.y);
    const b = x * sin(v.y) + y * cos(v.y);
    const s = Dc / (Dc - (-b * cos(v.p) + z * sin(v.p)));
    return [cx + Rr * a * s, cy - Rr * (b * sin(v.p) + z * cos(v.p)) * s, s];
  };
  const cl = E => Math.max(-85, Math.min(85, E));
  const Q = (az, E) => P(sin(az) * cos(E), cos(az) * cos(E), sin(E));
  const U = (az, e) => Q(az, cl(e * ex));
  const G = (az, e) => P(sin(az) * cos(cl(e * ex)), cos(az) * cos(cl(e * ex)), 0);
  const f = a => a.map(q => q[0].toFixed(1) + "," + q[1].toFixed(1)).join(" ");
  const ring = e => f(Array.from({ length: 73 }, (_, q) => U(q * 5, e)));
  const mer = [0, 90, 180, 270].map(a => `<polyline fill="none" stroke="#24324d" points="${f(Array.from({ length: 18 }, (_, q) => Q(a, q * 5)))}"/>`).join("");
  const cra = CRA.map(([x, y, R]) => `<polygon points="${f(Array.from({ length: 18 }, (_, q) => P(x + R * sin(q * 20), y + R * cos(q * 20), 0)))}" fill="#000" fill-opacity=".28" stroke="#6b7690" stroke-opacity=".35"/>`).join("");
  const st = [1, 2, 5, 10, 15, 30].find(s => m / s <= 4) || 30;
  let rg = "";
  for (let e = st; e <= Math.min(m, 80); e += st) {
    const p = U(45, e);
    rg += `<polyline fill="none" stroke="#2c3d5c" stroke-dasharray="2 4" points="${ring(e)}"/><text x="${p[0].toFixed(1)}" y="${p[1].toFixed(1)}" font-size="9.5" fill="#6f7e99">${e}°</text>`;
  }
  const cp = [
    ["N", 0],
    ["E", 90],
    ["S", 180],
    [lang == "es" ? "O" : "W", 270]
  ].map(([n, a]) => {
    const p = P(sin(a) * 1.14, cos(a) * 1.14, 0);
    return `<text x="${p[0].toFixed(1)}" y="${(p[1] + 4).toFixed(1)}" text-anchor="middle" font-size="12" fill="var(--mut)">${n}</text>`;
  }).join("");
  const j0 = d.j0 + k * step() / 24;
  const pt = [];
  for (let o = -12; o <= 60; o++) {
    const g = geo(j0 + o / 24);
    const rel = r => [r[0], r[1], r[0] - hzAt(r[1], c.id)];
    pt.push({ s: rel(look(c.lat, c.lon, g.s, 0)), e: rel(look(c.lat, c.lon, g.e, g.d)) });
  }
  const path = (key, col) => {
    let a = "";
    let b = "";
    for (let q = 1; q < pt.length; q++) {
      const A = U(pt[q - 1][key][1], pt[q - 1][key][0]);
      const B = U(pt[q][key][1], pt[q][key][0]);
      const s = `M${A[0].toFixed(1)} ${A[1].toFixed(1)}L${B[0].toFixed(1)} ${B[1].toFixed(1)}`;
      if (pt[q][key][2] > h) {
        a += s;
      }
      else {
        b += s;
      }
    }
    return `<path d="${b}" stroke="${col}" stroke-opacity=".4" stroke-dasharray="3 3" fill="none"/><path d="${a}" stroke="${col}" stroke-width="2.5" fill="none"/>`;
  };
  const body = (key, col, r, nm) => {
    const e = pt[12][key];
    const p = U(e[1], e[0]);
    const g = G(e[1], e[0]);
    const on = e[2] > h;
    const rr = r * p[2];
    return `<line x1="${p[0].toFixed(1)}" y1="${p[1].toFixed(1)}" x2="${g[0].toFixed(1)}" y2="${g[1].toFixed(1)}" stroke="${col}" stroke-opacity=".5" stroke-dasharray="2 3"/><circle cx="${p[0].toFixed(1)}" cy="${p[1].toFixed(1)}" r="${(rr * 2.5).toFixed(1)}" fill="${col}" opacity="${on ? 0.22 : 0}"/><circle cx="${p[0].toFixed(1)}" cy="${p[1].toFixed(1)}" r="${rr.toFixed(1)}" fill="${col}" opacity="${on ? 1 : 0.45}"/><text x="${(p[0] + rr + 5).toFixed(1)}" y="${(p[1] + 4).toFixed(1)}" font-size="11" font-weight="700" fill="${col}" stroke="#04070d" stroke-width="3" paint-order="stroke">${nm}</text>`;
  };
  const se = pt[12].s;
  const sa = se[1];
  const on = se[0] > 0.15;
  const L = on ? Math.min(0.95, 0.12 / Math.tan(se[0] * D)) : 0;
  const ox = -sin(sa);
  const oy = -cos(sa);
  const qx = cos(sa) * 0.035;
  const qy = -sin(sa) * 0.035;
  const shadow = on ? `<polygon points="${f([P(qx, qy, 0), P(ox * L, oy * L, 0), P(-qx, -qy, 0)])}" fill="#000" fill-opacity=".6"/>` : "";
  const b0 = P(0, 0, 0);
  const tp = P(0, 0, 0.12);
  const lander = `<line x1="${b0[0].toFixed(1)}" y1="${b0[1].toFixed(1)}" x2="${tp[0].toFixed(1)}" y2="${tp[1].toFixed(1)}" stroke="#cfd6e4" stroke-width="2"/><polygon points="${f([P(-0.07, -0.07, 0.12), P(0.07, -0.07, 0.12), P(0.07, 0.07, 0.12), P(-0.07, 0.07, 0.12)])}" fill="#ffd27a" fill-opacity=".85" stroke="#fff" stroke-opacity=".6"/><text x="${tp[0].toFixed(1)}" y="${(tp[1] - 9).toFixed(1)}" text-anchor="middle" font-size="10" fill="var(--fg)" stroke="#04070d" stroke-width="3" paint-order="stroke">${t("lnd")}</text>`;
  el.innerHTML = `<div class="dv">${[
    ["top", "vtop"],
    ["side", "vside"],
    ["reset", "vreset"]
  ].map(([a, b]) => `<button type="button" data-i="${i}" data-view="${a}">${t(b)}</button>`).join("")}</div><svg viewBox="0 0 420 280" role="img" aria-label="${t("dm")}"><defs><radialGradient id="dg"><stop offset="0" stop-color="#454d5f"/><stop offset="1" stop-color="#10141b"/></radialGradient></defs>${DST}
<polygon points="${ring(0)}" fill="url(#dg)" stroke="#8f9bb0" stroke-opacity=".6"/>${cra}${rg}${mer}${h > 0 ? `<polyline fill="none" stroke="var(--sun)" stroke-opacity=".6" stroke-dasharray="4 3" points="${ring(h)}"/>` : ""}${cp}${shadow}${lander}
${path("s", "var(--sun)")}${path("e", "var(--ea)")}${body("s", "var(--sun)", 5, t("sun"))}${body("e", "var(--ea)", 8, t("earth"))}</svg><p class="cap" style="padding:0 10px 8px">${t("dmcap").replace("{x}", nf(ex, 1))}</p>`;
}

function scr() {
  const s = document.querySelector(`[data-ch="${scrub}"]`);
  if (!s) {
    return;
  }
  const r = s.getBoundingClientRect();
  const fx = (scr.x - r.left) / r.width * 640;
  const N = NN();
  k = Math.max(0, Math.min(N, Math.round((fx - 46) / 584 * N)));
  $("#t").value = k;
  render();
}

document.addEventListener("pointerdown", e => {
  const m = e.target.closest("button") ? null : e.target.closest("[data-dome]");
  const c = e.target.closest("[data-ch]");
  if (m) {
    drag = {
      i: +m.dataset.dome, x: e.clientX, y: e.clientY
    };
    e.preventDefault();
  }
  else if (c) {
    scrub = +c.dataset.ch;
    scr.x = e.clientX;
    scr();
  }
});

document.addEventListener("pointermove", e => {
  if (drag) {
    const v = V[drag.i];
    v.y = (v.y + (e.clientX - drag.x) * 0.5) % 360;
    v.p = Math.max(5, Math.min(85, v.p + (e.clientY - drag.y) * 0.4));
    drag.x = e.clientX;
    drag.y = e.clientY;
    drawDome(drag.i);
  }
  else if (scrub != null) {
    scr.x = e.clientX;
    scr();
  }
});

document.addEventListener("pointerup", () => {
  drag = null;
  scrub = null;
});

document.addEventListener("dblclick", e => {
  const m = e.target.closest("[data-dome]");
  if (m) {
    V[+m.dataset.dome] = { y: 25, p: 52 };
    drawDome(+m.dataset.dome);
  }
});
const fc = {};

function finder(c, i) {
  const L = mlv;
  const key = [c.lat, c.lon, c.date, c.h, L, HZID[c.id] || ""].join();
  let f = fc[i];
  if (!f || f.key != key) {
    const n = (90 + L) * 4;
    const w = L * 4;
    const j0 = Date.parse(c.date + "T00:00:00Z") / 864e5 + 2440587.5 - 30;
    const ps = [0];
    const pe = [0];
    for (let q = 0; q < n; q++) {
      const g = geo(j0 + q / 4);
      ps.push(ps[q] + ((r => r[0] - (hzAt(r[1], c.id)) > c.h)(look(c.lat, c.lon, g.s, 0))));
      pe.push(pe[q] + ((r => r[0] - (hzAt(r[1], c.id)) > c.h)(look(c.lat, c.lon, g.e, g.d))));
    }
    const r = [];
    for (let o = 0; o <= 90; o++) {
      const a = 100 * (ps[o * 4 + w] - ps[o * 4]) / w;
      const b = 100 * (pe[o * 4 + w] - pe[o * 4]) / w;
      r.push({
        o, a, b, s: (a + b) / 2
      });
    }
    f = fc[i] = {
      key, r, j0
    };
  }
  const dt = o => new Date((f.j0 - 2440587.5 + o) * 864e5).toISOString().slice(0, 10);
  const top = [];
  const wd = 600 / 91;
  [...f.r].sort((x, y) => y.s - x.s).forEach(x => {
    if (top.length < 3 && top.every(y => Math.abs(y.o - x.o) >= 3)) {
      top.push(x);
    }
  });
  document.querySelector("#c" + i + " .fin").innerHTML = `<h3>${t("fh")}</h3><p class="cap">${t("fcap").replace("{L}", L)}</p>
<svg viewBox="0 0 600 34" role="img" aria-label="${t("a3")}">${f.r.map(x => `<rect x="${(x.o * wd).toFixed(1)}" y="0" width="${(wd + 0.5).toFixed(1)}" height="30" fill="hsl(${(x.s * 1.3).toFixed(0)},65%,42%)"/>`).join("")}<line x1="${30 * wd + wd / 2}" x2="${30 * wd + wd / 2}" y1="0" y2="34" stroke="var(--fg)" stroke-width="2"/></svg>
<p class="cap">${t("fbest")}</p><div class="sh">${top.map(x => `<button type="button" data-i="${i}" data-date="${dt(x.o)}">${t("fbtn").replace("{d}", dt(x.o)).replace("{a}", x.a.toFixed(0)).replace("{b}", x.b.toFixed(0))}</button>`).join("")}</div>`;
}

function sync() {
  const q = $("#t");
  q.max = NN();
  if (k > NN()) {
    k = NN();
  }
  q.value = k;
}

document.addEventListener("input", e => {
  const q = e.target;
  const i = q.dataset.i;
  if (q.id == "theme") {
    theme = q.value;
    ui();
    return;
  }
  if (q.id == "lang") {
    lang = q.value;
    ui();
    buildCards();
    render();
    return;
  }
  if (q.id == "days") {
    markHorizonsStale();
    days = +q.value;
    cache = {};
    k = 0;
    sync();
    render();
    return;
  }
  if (q.id == "ml") {
    mlv = +q.value;
    render();
    return;
  }
  if (q.id == "t") {
    k = +q.value;
    render();
    return;
  }
  if (i == null) {
    return;
  }
  const c = S[i];
  const f = q.dataset.f;
  const cd = $("#c" + i);
  if (f == "si") {
    markHorizonsStale();
    c.si = +q.value;
    delete cache[i];
    c.lat = SITES[c.si][1];
    c.lon = SITES[c.si][2];
    cd.querySelector("[data-f=lat]").value = c.lat;
    cd.querySelector("[data-f=lon]").value = c.lon;
  }
  else if (f == "date") {
    if (!q.value) {
      return;
    }
    markHorizonsStale();
    c.date = q.value;
    delete cache[i];
  }
  else {
    const v = parseFloat(q.value);
    if (isNaN(v)) {
      return;
    }
    c[f] = f == "lat" ? Math.max(-90, Math.min(90, v)) : v;
    if (f == "lat" || f == "lon") {
      markHorizonsStale();
      delete cache[i];
    }
    if (f == "h") {
      cd.querySelector("output").textContent = c.h + "°";
    }
    else {
      c.si = SITES.length - 1;
      cd.querySelector("[data-f=si]").value = c.si;
    }
  }
  render();
});

document.addEventListener("click", e => {
  const q = e.target;
  const dd = q.dataset;
  if (q.id == "horizons") {
    openHorizons(q);
    return;
  }
  if (q.id == "csv") {
    csv();
    return;
  }
  if (q.id == "share") {
    const u = location.href.split("#")[0] + "#" + hash();
    try {
      history.replaceState(null, "", "#" + hash());
    }
    catch (x) {
    }
    const ok = () => {
      q.textContent = t("copied");
      setTimeout(() => q.textContent = t("share"), 1500);
    };
    if (navigator.clipboard) {
      navigator.clipboard.writeText(u).then(ok, () => prompt(t("share"), u));
    }
    else {
      prompt(t("share"), u);
    }
    return;
  }
  if (q.id == "play") {
    if (timer) {
      clearInterval(timer);
      timer = null;
    }
    else {
      timer = setInterval(() => {
        k = k >= NN() ? 0 : k + 1;
        $("#t").value = k;
        render();
      }, 70);
    }
    q.textContent = t(timer ? "pause" : "play");
    return;
  }
  if (q.id == "why") {
    [0, 2].forEach((h, i) => Object.assign(S[i], {
      si: 1, lat: SITES[1][1], lon: SITES[1][2], h, date: "2027-03-01"
    }));
    days = 30;
    cache = {};
    k = 0;
    two = true;
    whyOn = true;
    ui();
    buildCards();
    render();
    return;
  }
  if (q.id == "addb") {
    markHorizonsStale();
    two = !two;
    if (!two) {
      whyOn = false;
    }
    ui();
    buildCards();
    render();
    return;
  }
  if (dd.view) {
    const n = +dd.i;
    const y = V[n].y;
    V[n] = dd.view == "top" ? { y, p: 88 } : dd.view == "side" ? { y, p: 8 } : { y: 25, p: 52 };
    drawDome(n);
    return;
  }
  if (dd.i == null) {
    return;
  }
  const c = S[dd.i];
  if (dd.sh != null) {
    markHorizonsStale();
    delete cache[dd.i];
    c.date = new Date(Date.parse(c.date + "T00:00:00Z") + dd.sh * 864e5).toISOString().slice(0, 10);
  }
  else if (dd.date) {
    markHorizonsStale();
    delete cache[dd.i];
    c.date = dd.date;
  }
  else {
    return;
  }
  $("#c" + dd.i).querySelector("[data-f=date]").value = c.date;
  render();
});

function startSky() {
  const cv = document.querySelector("#space");
  const g = cv.getContext("2d");
  const still = matchMedia("(prefers-reduced-motion: reduce)").matches;
  let w = 0;
  let h = 0;
  let stars = [];
  let shoot = null;
  let nextShoot = 4000;
  let px = 0;
  let py = 0;
  let tx = 0;
  let ty = 0;

  const fit = () => {
    const r = window.devicePixelRatio || 1;
    w = window.innerWidth;
    h = window.innerHeight;
    cv.width = w * r;
    cv.height = h * r;
    g.setTransform(r, 0, 0, r, 0, 0);
    stars = Array.from({ length: Math.round((w * h) / 8000) }, () => ({
      x: Math.random() * w,
      y: Math.random() * h,
      z: Math.random(),
      ph: Math.random() * 6.28
    }));
  };

  const drawShootingStar = (t) => {
    if (!shoot && t > nextShoot) {
      shoot = { x: Math.random() * w * 0.8, y: Math.random() * h * 0.4, t0: t };
      nextShoot = t + 6000 + Math.random() * 9000;
    }
    if (!shoot) {
      return;
    }
    const p = (t - shoot.t0) / 900;
    if (p >= 1) {
      shoot = null;
      return;
    }
    const x = shoot.x + p * 260;
    const y = shoot.y + p * 110;
    const grad = g.createLinearGradient(x, y, x - 90, y - 38);
    grad.addColorStop(0, "rgba(255,255,255,.9)");
    grad.addColorStop(1, "rgba(255,255,255,0)");
    g.globalAlpha = 1 - p * 0.6;
    g.strokeStyle = grad;
    g.lineWidth = 1.6;
    g.beginPath();
    g.moveTo(x, y);
    g.lineTo(x - 90, y - 38);
    g.stroke();
  };

  const frame = (t) => {
    px += (tx - px) * 0.05;
    py += (ty - py) * 0.05;
    g.clearRect(0, 0, w, h);
    g.fillStyle = starTint;
    for (const s of stars) {
      const shift = 4 + 14 * s.z;
      const drift = still ? 0 : t * 0.004 * (0.2 + s.z);
      const x = (((s.x - drift - px * shift) % w) + w) % w;
      const y = (((s.y - py * shift) % h) + h) % h;
      const twinkle = still ? 0.7 : 0.5 + 0.5 * Math.sin(t * 0.0015 * (1 + s.z) + s.ph);
      g.globalAlpha = (0.35 + 0.65 * twinkle) * (0.4 + 0.6 * s.z);
      g.beginPath();
      g.arc(x, y, 0.5 + s.z * 1.1, 0, 6.2832);
      g.fill();
    }
    if (!still) {
      drawShootingStar(t);
    }
    g.globalAlpha = 1;
  };

  const loop = (t) => {
    frame(t);
    requestAnimationFrame(loop);
  };

  window.addEventListener("resize", fit);
  window.addEventListener("pointermove", (e) => {
    tx = e.clientX / w - 0.5;
    ty = e.clientY / h - 0.5;
  });
  fit();
  if (still) {
    frame(0);
    window.addEventListener("resize", () => frame(0));
  } else {
    requestAnimationFrame(loop);
  }
}

load();

ui();
buildCards();
render();
startSky();
  </script>

  <script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>
  <script type="importmap">{"imports":{"three":"https://cdn.jsdelivr.net/npm/three@0.160.0/build/three.module.js"}}</script>
  <script type="module">
    import * as THREE from "https://cdn.jsdelivr.net/npm/three@0.160.0/build/three.module.js";
    import { OrbitControls } from "https://cdn.jsdelivr.net/npm/three@0.160.0/examples/jsm/controls/OrbitControls.js";
    import { fromUrl } from "https://cdn.jsdelivr.net/npm/geotiff@2.1.3/+esm";

    const map = L.map("southMap", { crs:L.CRS.EPSG4326, minZoom:1, maxZoom:7, zoomControl:true,
      maxBounds:[[-90,-180],[-60,180]], maxBoundsViscosity:1 }).setView([-86,0],2);
    L.tileLayer("https://moontrek.jpl.nasa.gov/trektiles/Moon/EQ/LRO_WAC_Mosaic_Global_303ppd_v02/1.0.0/default/default028mm/{z}/{y}/{x}.jpg", {
      tileSize:256, zoomOffset:0, minZoom:1, maxZoom:7, noWrap:true,
      attribution:'NASA/JPL Moon Trek · LRO Wide Angle Camera (WAC)'
    }).addTo(map);
    let marker = L.marker([-89.9,129.8]).addTo(map);
    window.seleneUpdateMap = (lat,lon) => {
      if (lat <= -60) { marker.setLatLng([lat,lon]); }
    };
    map.on("click", e => {
      const lat = Math.max(-90, Math.min(-60, e.latlng.lat));
      const lon = e.latlng.lng;
      marker.setLatLng([lat,lon]);
      const latInput = document.querySelector('#c0 [data-f="lat"]');
      const lonInput = document.querySelector('#c0 [data-f="lon"]');
      if (latInput && lonInput) {
        latInput.value = lat.toFixed(4); lonInput.value = lon.toFixed(4);
        latInput.dispatchEvent(new Event("change",{bubbles:true}));
        const freshLonInput = document.querySelector('#c0 [data-f="lon"]');
        if (freshLonInput) { freshLonInput.value = lon.toFixed(4); freshLonInput.dispatchEvent(new Event("change",{bubbles:true})); }
      }
    });

    const host = document.getElementById("lola3d");
    const status = document.getElementById("lolaStatus");
    let renderer, scene, camera, controls, terrainMesh;
    function init3D(){
      if(renderer) return;
      scene = new THREE.Scene(); scene.background = new THREE.Color(0x050912);
      camera = new THREE.PerspectiveCamera(45, host.clientWidth/host.clientHeight, 0.1, 5000);
      camera.position.set(0,-210,175);
      renderer = new THREE.WebGLRenderer({antialias:true}); renderer.setPixelRatio(Math.min(devicePixelRatio,2));
      renderer.setSize(host.clientWidth,host.clientHeight); host.appendChild(renderer.domElement);
      scene.add(new THREE.HemisphereLight(0xc8d9ff,0x34303a,2.0));
      const light = new THREE.DirectionalLight(0xffffff,2.2); light.position.set(-100,-80,180); scene.add(light);
      controls = new OrbitControls(camera,renderer.domElement); controls.enableDamping=true;
      const grid = new THREE.GridHelper(220,22,0x33435e,0x1b2a40); grid.position.z=-1; scene.add(grid);
      const animate=()=>{requestAnimationFrame(animate);controls.update();renderer.render(scene,camera)}; animate();
      new ResizeObserver(()=>{if(!renderer)return;camera.aspect=host.clientWidth/host.clientHeight;camera.updateProjectionMatrix();renderer.setSize(host.clientWidth,host.clientHeight)}).observe(host);
    }
    function colorFor(h,min,max){
      const v=(h-min)/Math.max(1,max-min);
      const c=new THREE.Color();
      c.setHSL(0.62-v*0.55,0.38+v*0.22,0.24+v*0.35); return c;
    }
    async function loadTerrain(){
      const site=document.getElementById("lolaSite").value;
      const url=`https://pgda.gsfc.nasa.gov/data/LOLA_5mpp/${site}/${site}_final_adj_5mpp_surf.tif`;
      status.textContent=`Descargando los datos de elevación LOLA de ${site}…`;
      try{
        const tiff=await fromUrl(url,{cacheSize:32});
        const image=await tiff.getImage();
        const w=image.getWidth(), h=image.getHeight();
        const side=Math.min(320,w,h), x0=Math.floor((w-side)/2), y0=Math.floor((h-side)/2);
        const raster=await image.readRasters({window:[x0,y0,x0+side,y0+side],width:180,height:180,interleave:true});
        const n=180, vals=Array.from(raster).filter(Number.isFinite);
        if(!vals.length) throw new Error("El DEM no contiene valores válidos en la zona central.");
        const min=Math.min(...vals), max=Math.max(...vals);
        const positions=[], colors=[], indices=[];
        for(let y=0;y<n;y++) for(let x=0;x<n;x++){
          const i=y*n+x, z=Number(raster[i]);
          positions.push((x-n/2)*1.1,(n/2-y)*1.1,(z-(min+max)/2)*0.025);
          const c=colorFor(z,min,max); colors.push(c.r,c.g,c.b);
          if(x<n-1&&y<n-1){const a=i,b=i+1,c=i+n,d=i+n+1;indices.push(a,c,b,b,c,d)}
        }
        let hzMsg="";
        try{
          const ti=+document.getElementById("lolaTarget").value, sc=window.seleneScenario(ti), Rm=1737400;
          const [ox0,oy0]=image.getOrigin(), [rx,ry]=image.getResolution();
          let cxp=w/2, cyp=h/2, rot=0, exact=false;
          if(sc.lat<-60){
            const rho=2*Rm*Math.tan(Math.PI/4+sc.lat*Math.PI/360), lam=sc.lon*Math.PI/180;
            const qx=(rho*Math.sin(lam)-ox0)/rx, qy=(rho*Math.cos(lam)-oy0)/ry;
            if(qx>=0&&qy>=0&&qx<w&&qy<h){cxp=qx;cyp=qy;rot=sc.lon;exact=true}
          }
          const HS=Math.min(2000,w,h), hx=Math.max(0,Math.min(w-HS,Math.round(cxp-HS/2))), hy=Math.max(0,Math.min(h-HS,Math.round(cyp-HS/2))), HN=400, px=Math.abs(rx||5)*HS/HN;
          const ocx=(cxp-hx)/HS*HN, ocy=(cyp-hy)/HS*HN;
          const big=await image.readRasters({window:[hx,hy,hx+HS,hy+HS],width:HN,height:HN,interleave:true});
          const at=(xx,yy)=>{const a=Math.round(xx),b=Math.round(yy);return (a<0||b<0||a>=HN||b>=HN)?NaN:Number(big[b*HN+a])};
          const z0=at(ocx,ocy)+2, prof=new Array(360).fill(0);
          for(let az=0;az<360;az++){
            let m=-90; const g=(az+rot)*Math.PI/180, s=Math.sin(g), c=Math.cos(g);
            for(let r=px;r<=4500;r+=px){
              const z=at(ocx+r*s/px,ocy-r*c/px); if(!Number.isFinite(z)) break;
              m=Math.max(m,Math.atan2(z-z0-r*r/(2*Rm),r)*180/Math.PI);
            }
            prof[az]=Math.max(-5,m);
          }
          window.seleneSetHorizon(prof,site+Date.now(),ti);
          hzMsg=exact?` Horizonte real calculado para el escenario ${"AB"[ti]} desde sus coordenadas exactas (${prof.reduce((a,b)=>Math.max(a,b)).toFixed(1)}° máx.).`:` Las coordenadas del escenario ${"AB"[ti]} caen fuera de este recorte: el horizonte se midió desde el centro del recorte. Elige un sitio LOLA que contenga tus coordenadas.`;
        }catch(e2){console.warn(e2);hzMsg=" No se pudo calcular el horizonte: "+e2.message}
        init3D();
        if(terrainMesh){scene.remove(terrainMesh);terrainMesh.geometry.dispose();terrainMesh.material.dispose()}
        const geo=new THREE.BufferGeometry();
        geo.setAttribute("position",new THREE.Float32BufferAttribute(positions,3));
        geo.setAttribute("color",new THREE.Float32BufferAttribute(colors,3)); geo.setIndex(indices); geo.computeVertexNormals();
        terrainMesh=new THREE.Mesh(geo,new THREE.MeshStandardMaterial({vertexColors:true,roughness:0.95,metalness:0.02,side:THREE.DoubleSide}));
        scene.add(terrainMesh); camera.position.set(0,-225,160); controls.target.set(0,0,0); controls.update();
        status.textContent=`${site}: relieve generado desde el GeoTIFF LOLA de 5 m/píxel. Elevación en el recorte: ${min.toFixed(1)} a ${max.toFixed(1)} m. Vista 3D con exageración vertical.`+hzMsg;
      }catch(err){
        status.textContent=`No se pudo cargar el DEM desde NASA: ${err.message}. Puede deberse a CORS o a que el servidor no permite lectura desde esta página. El modelo no se ha sustituido por terreno simulado.`;
      }
    }
    document.getElementById("loadLola").addEventListener("click",loadTerrain);
    window.addEventListener("resize",()=>{if(renderer){renderer.setSize(host.clientWidth,host.clientHeight)}});
  </script>
</body>

</html>
