# CLAUDE.md — Corpus Method · AlmaWolf

Guía para que cualquier instancia de Claude pueda trabajar en este repositorio sin contexto previo.

---

## Qué es el Corpus Method

Presentaciones autocontenidas en un único archivo HTML, publicadas en GitHub Pages, sin frameworks ni dependencias de build. Cada versión vive en su propia carpeta dentro de `main` — no se usan ramas para versionar. El archivo HTML contiene todo: CSS inline en `<style>`, contenido en el `<body>`, navegación JS en `<script>`, y el modo de revisión editorial cargado desde `web-review-mode.js` (archivo hermano).

**Reglas fijas:**
- Un solo `index.html` por versión. Sin imports externos salvo Google Fonts e `Inter`.
- Sin frameworks (React, Vue, etc.), sin CSS frameworks (Tailwind, etc.), sin bundlers.
- `<meta name="robots" content="noindex, nofollow">` siempre presente — estas presentaciones no son públicas.
- Cada versión nueva = carpeta nueva (v5/, v6/, …) + entrada en el `index.html` raíz.

---

## Design system

### Tokens CSS (`:root`)

```css
--red:    #C13B2F   /* acento principal: CTA, labels, viñetas, speaker IA */
--navy:   #1C1C2E   /* texto principal */
--bg:     #EDECEB   /* fondo de la mayoría de slides */
--white:  #FFFFFF   /* fondo de portada, cierre y cards */
--gray:   #6B7280   /* texto secundario, subtítulos, speaker humano */
--dark:   #0F1119   /* fondo de diagramas y banners de stats */
--border: #D5D4D2   /* bordes de cards y tablas */
--f:      'Inter', system-ui, sans-serif
```

### Clases tipográficas

| Clase | Uso | Tamaño |
|-------|-----|--------|
| `.d1` | Título grande (portada) | `clamp(38px, 5.2vw, 76px)` · weight 300 |
| `.d2` | Título de slide | `clamp(28px, 3.5vw, 52px)` · weight 400 |
| `.body` | Párrafo de cuerpo | `clamp(13px, 1.15vw, 15.5px)` · color `--gray` |
| `.stat-n` | Número de estadística | `clamp(36px, 4.8vw, 68px)` · weight 800 · color `--red` |
| `.lbl` | Etiqueta de sección (top-left) | 10.5px · uppercase · color `--red` |
| `.q-sup` | Supertítulo de panel | 9px · uppercase · letter-spacing .18em · color `--red` |

### Fondos de slide

- `.bg-l` → `--bg` (warm gray, el más común)
- `.bg-w` → `--white`
- `.bg-d` → `--dark` (solo diagramas técnicos)

---

## Estructura de una slide

Toda slide es una `<section class="slide [bg-*]" id="sN">`. La primera tiene `class="slide active"`. El layout es flexbox; la dirección y el padding varían por slide.

**Chrome fijo en todas las slides (excepto portada y cierre):**
```html
<div class="lbl"><span class="d"></span>Nombre de sección</div>   <!-- top-left, rojo -->
<div class="aw"><!-- SVG logo AlmaWolf --></div>                   <!-- top-right -->
```

**SVG logo reutilizable (copiar tal cual):**
```html
<svg width="110" height="22" viewBox="0 0 110 22" fill="none">
  <polygon points="0,11 8,0 16,11 8,22" fill="#C13B2F" opacity=".9"/>
  <polygon points="8,5 14,11 8,17" fill="#7a1f16"/>
  <text x="22" y="15.5" font-family="Inter,sans-serif" font-size="13" font-weight="400" fill="#1C1C2E" letter-spacing="0.5">AlmaWolf</text>
</svg>
```

**Viñeta diamante (`.dl`):**
```html
<ul class="dl">
  <li>Texto del punto</li>
</ul>
```
El `::before` del `li` pinta un cuadrado rotado 45° en `--red`. No usar `<ul>/<li>` genéricos — siempre `.dl`.

---

## Patrones de layout frecuentes

### Dos columnas (izquierda texto · derecha contenido)
Slides S6 y S7 como referencia:
```html
<section class="slide bg-l" id="sN">
  <!-- chrome -->
  <div class="sNl" style="width:40%;padding-top:72px;padding-left:56px;padding-bottom:56px;display:flex;flex-direction:column;justify-content:center;padding-right:44px">
    <div class="d2">Título.</div>
    <p class="body" style="margin-top:16px">Párrafo explicativo.</p>
  </div>
  <div class="sNr" style="flex:1;padding:80px 56px 56px 16px;display:flex;flex-direction:column;justify-content:center">
    <!-- contenido derecha -->
  </div>
</section>
```

### Transcripciones (`.tr-*`)
Para mostrar diálogos agente/persona. Speaker IA siempre en `--red`, speaker humano siempre en `--gray`.
```html
<div class="q-sup">Transcripción real · Piloto agosto 2026</div>
<div style="margin-top:12px;display:flex;flex-direction:column;gap:5px">
  <div class="tr-line">
    <span class="tr-who ag">Agente</span>
    <span class="tr-text">Texto del agente.</span>
  </div>
  <div class="tr-line">
    <span class="tr-who us">Persona</span>
    <span class="tr-text">Respuesta.</span>
  </div>
</div>
```
Si hay muchas líneas (>10) y el espacio es justo, añadir scoped overrides:
```css
#sN .tr-text { font-size: 10px; line-height: 1.4; }
#sN .tr-line { margin-bottom: 3px; }
```

### Cards de problema/solución (`.prow`)
Grid 3 columnas: `problema → flecha → solución`.
```html
<div class="prow">
  <div class="pp">Nombre problema</div>
  <div class="pa">→</div>
  <div class="ps">
    <div class="ps-lbl">Solución</div>
    <div class="ps-txt">Texto</div>
  </div>
</div>
```
Para añadir descripción bajo el nombre de problema, envolver en `.pp-col`:
```html
<div class="pp-col">
  <div class="pp">Nombre problema</div>
  <div class="pp-desc">Descripción breve en gris</div>
</div>
```

---

## Navegación JS

Al final del `<body>`, antes del bloque de review:
```html
<script>
(function(){
  const slides = document.querySelectorAll('.slide');
  const cnt = document.getElementById('cnt');
  let cur = 0;

  function go(n){
    slides[cur].classList.remove('active');
    cur = (n + slides.length) % slides.length;
    slides[cur].classList.add('active');
    cnt.textContent = (cur+1) + ' / ' + slides.length;
  }

  document.getElementById('next').onclick = () => go(cur+1);
  document.getElementById('prev').onclick = () => go(cur-1);

  document.addEventListener('keydown', e => {
    if(e.key === 'ArrowRight' || e.key === 'ArrowDown' || e.key === ' ') go(cur+1);
    if(e.key === 'ArrowLeft'  || e.key === 'ArrowUp') go(cur-1);
  });
})();
</script>
```
Y la barra nav fija en el HTML:
```html
<nav class="nav">
  <button id="prev">‹</button>
  <span class="cnt" id="cnt">1 / N</span>
  <button id="next">›</button>
  <span class="kh">← →</span>
</nav>
```

---

## Modo de revisión editorial (web-review-mode.js)

Permite a revisores dejar comentarios directamente sobre elementos de la presentación. Los comentarios se sincronizan con un Google Sheet vía Google Apps Script.

**Configuración** — al final del `<body>`, tras el bloque de navegación:
```html
<script>
  window.WEB_REVIEW_CONFIG = {
    projectName: 'nombre-del-proyecto',   // ← CAMBIAR por proyecto nuevo
    webhookUrl:  'https://script.google.com/macros/s/…/exec',  // ← GAS webhook
    brandColor:  '#C13B2F'
  };
</script>
<script src="web-review-mode.js"></script>
```

Para un proyecto nuevo copiar `web-review-mode.js` de una versión existente y cambiar solo `projectName` y `webhookUrl`. El webhook apunta al mismo Google Sheet de feedback de AlmaWolf; si el proyecto necesita su propio sheet, crear un nuevo GAS.

**Activar:** `⌥ + C` (Mac) o botón `✍️ Comentar` (esquina inferior derecha).

---

## Convención de versiones

```
repo/
├── index.html        # Índice de versiones (lista todas las versiones publicadas)
├── robots.txt
├── vN/
│   ├── index.html    # La presentación
│   └── web-review-mode.js
└── vN+1/
    ├── index.html
    └── web-review-mode.js
```

- Para publicar una nueva versión: crear carpeta `vN+1/`, copiar `index.html` y `web-review-mode.js` de la versión anterior, hacer los cambios, y añadir la entrada al `index.html` raíz.
- La versión anterior no se toca — queda archivada con `style="opacity:.6"` en el índice.
- No se crean ramas de git para versionar. Todo va en `main`.
- GitHub Pages publica automáticamente desde `main`. URL: `gabriel-almawolf.github.io/her/vN/`.

---

## Tareas e issues pendientes

Las funcionalidades bloqueadas o pendientes de decisión de equipo se documentan en `issues/` como archivos Markdown. No se usan comentarios TODO en el código ni secciones de pendientes en el README.

---

## Tareas Jira activas (referencia)

- **AW-151** — Correcciones inmediatas JP (completado en v5)
- **AW-152** — Assets externos: logo AlmaWolf Lab, audio de ejemplo, QRs de agentes para S13
- **AW-153** — Decisiones de equipo: URL almawolf.com/lab/her, mitigación riesgo no-uso, roadmap real, rediseño S14

Label en todos: `corpus-method`. Proyecto Jira: `AW`. CloudId: `d557f1b8-c7d8-4596-8b24-f8873b3749ab`.
