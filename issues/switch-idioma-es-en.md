# Switch de idioma ES / EN

**Estado:** Pendiente — bloqueado hasta cerrar contenido en español  
**Relacionado con:** AW-151, AW-153

## Qué es

Implementar un selector de idioma a nivel de presentación. Un botón `ES / EN` que intercambia todos los textos del deck simultáneamente, sin recargar la página ni mantener dos archivos separados.

## Condición de inicio

Contenido en español completamente cerrado: textos finales, transcripciones reales (S6/S13), S14 rediseñada, roadmap acordado (S12). Traducir antes de montar el switch para no hacerlo dos veces.

## Mecánica prevista

- Objeto `i18n = { es: { ... }, en: { ... } }` con ~60–80 strings del deck, en un bloque `<script>` inline antes de `</body>`
- Botón `ES / EN` integrado en la barra de navegación existente del deck
- Al hacer clic: recorre los elementos con atributo `data-i18n="key"` y reemplaza su `textContent`
- Idioma inicial: detectado por `navigator.language`, con override en `localStorage`
- URL: `?lang=en` para compartir directamente la versión en inglés (`history.replaceState` al cambiar)
- Un solo archivo HTML por versión — una sola fuente de verdad

## Alcance

- Todos los textos visibles de los 14 slides
- Labels del deck (lbl, q-sup, etc.)
- No afecta a `web-review-mode.js` (la barra de revisión opera en el idioma del revisor)

## Ejemplo de marcado

```html
<!-- antes -->
<div class="port-sub">Realidad percibida accionable</div>

<!-- después -->
<div class="port-sub" data-i18n="portSub">Realidad percibida accionable</div>
```

```js
const i18n = {
  es: { portSub: 'Realidad percibida accionable', ... },
  en: { portSub: 'Actionable perceived reality', ... }
}
```
