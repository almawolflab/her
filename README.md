# Proyecto Her — AlmaWolf

Sistema de check-in diario por voz para equipos, construido sobre agentes de IA conversacional.

| | |
|---|---|
| **Índice de versiones** | [gabriel-almawolf.github.io/her](https://gabriel-almawolf.github.io/her/) |
| **V5 · Actual** | [gabriel-almawolf.github.io/her/v5](https://gabriel-almawolf.github.io/her/v5/) |
| **V4** | [gabriel-almawolf.github.io/her/v4](https://gabriel-almawolf.github.io/her/v4/) |

## ¿Qué es Her?

Her permite que cada persona del equipo reporte su actividad diaria hablando — en su idioma, desde donde esté — y que cualquier manager consulte ese conocimiento al instante, también en voz alta. El piloto de agosto 2026 con 9 personas demostró un 96% de cumplimiento y 82% de respuestas correctas.

## Estructura del repositorio

Cada versión vive en su propia carpeta dentro de `main`. Para publicar una nueva versión se crea una carpeta nueva y se añade la entrada al índice — sin ramas adicionales.

```
her/
├── index.html          # Índice de versiones (gabriel-almawolf.github.io/her/)
├── robots.txt
├── v4/                 # Versión 4 — Piloto Agosto 2026 (revisada por JP/Edu)
│   ├── index.html
│   └── web-review-mode.js
└── v5/                 # Versión 5 — Correcciones JP, Septiembre 2026
    ├── index.html
    └── web-review-mode.js
```

## Tareas pendientes (próxima versión)

### Switch de idioma ES / EN

Implementar selector de idioma a nivel de presentación. Un objeto JS con todas las cadenas en `es` y `en`, y un botón toggle que intercambia el idioma en todos los slides a la vez.

**Condición de inicio:** contenido en español cerrado (textos finales, transcripciones, S14). Traducir antes de montar el switch para no traducir dos veces.

**Mecánica prevista:**
- Objeto `i18n = { es: {...}, en: {...} }` con ~60–80 strings del deck
- Botón `ES / EN` en la barra de navegación del deck
- Idioma persistido en `localStorage`
- Parámetro `?lang=en` en la URL para compartir directamente la versión inglesa
- Un solo archivo HTML, una sola fuente de verdad

**Issues relacionados:** AW-151 (textos en español), AW-153 (decisiones de equipo pendientes que afectan al contenido)

---

## Modo de revisión editorial

La presentación incluye una barra de revisión para dejar notas directamente sobre los elementos. Para activarla:

1. Abre la presentación en el navegador
2. Pulsa **⌥ + C** (Option + C en Mac) o haz clic en **✍️ Comentar** en la esquina inferior derecha
3. Pasa el cursor sobre cualquier elemento → haz clic → deja tu nota
4. Las notas se sincronizan automáticamente con el registro de feedback del equipo

Para volver a navegar con normalidad, pulsa **⌥ + C** de nuevo o haz clic en **🧭 Navegar**.
