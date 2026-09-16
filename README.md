# Proyecto Her — AlmaWolf

Sistema de check-in diario por voz para equipos, construido sobre agentes de IA conversacional.

| | |
|---|---|
| **Índice de versiones** | [gabriel-almawolf.github.io/her](https://gabriel-almawolf.github.io/her/) |
| **V4 · Actual** | [gabriel-almawolf.github.io/her/v4](https://gabriel-almawolf.github.io/her/v4/) |

## ¿Qué es Her?

Her permite que cada persona del equipo reporte su actividad diaria hablando — en su idioma, desde donde esté — y que cualquier manager consulte ese conocimiento al instante, también en voz alta. El piloto de agosto 2026 con 9 personas demostró un 96% de cumplimiento y 82% de respuestas correctas.

## Estructura del repositorio

Cada versión vive en su propia carpeta dentro de `main`. Para publicar una nueva versión se crea una carpeta nueva y se añade la entrada al índice — sin ramas adicionales.

```
her/
├── index.html          # Índice de versiones (gabriel-almawolf.github.io/her/)
├── robots.txt
├── v4/                 # Versión 4 — Piloto Agosto 2026
│   ├── index.html
│   └── web-review-mode.js
└── v5/                 # Próxima versión (cuando corresponda)
    └── ...
```

## Modo de revisión editorial

La presentación incluye una barra de revisión para dejar notas directamente sobre los elementos. Para activarla:

1. Abre la presentación en el navegador
2. Pulsa **⌥ + C** (Option + C en Mac) o haz clic en **✍️ Comentar** en la esquina inferior derecha
3. Pasa el cursor sobre cualquier elemento → haz clic → deja tu nota
4. Las notas se sincronizan automáticamente con el registro de feedback del equipo

Para volver a navegar con normalidad, pulsa **⌥ + C** de nuevo o haz clic en **🧭 Navegar**.
