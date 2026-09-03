# Mapa de Arquitectura — WiFired Landing Page

| Módulo | Ruta/Vista | Archivo principal | Función/Propósito |
|--------|-----------|-------------------|-------------------|
| Landing page | `/` | `index.html` | Página única con toda la info: hero, planes, cobertura, contacto |
| Logo principal | — | `assets/logo.png` | Logo WiFired a color en blanco |
| Favicon | — | `assets/favicon.png` | Ícono del navegador |
| Apple touch icon | — | `assets/apple-touch-icon.png` | Ícono para iOS |
| Open Graph | — | `assets/og-image.png` | Imagen para compartir en redes |
| Fuentes | — | `assets/fonts/*.woff2` | Barlow y Barlow Condensed (400, 500, 600, 700) |
| Bóveda Obsidian | — | `Wifired pagina/` | Notas del proyecto (no va al repo) |
| Docs proyecto | — | `README.md` | Documentación general del repo |
| Dev server | — | `.claude/launch.json` | Config servidor local (npx serve :8080) para preview |

## Animaciones del esquema de red (WF-NODO-01)

| Clase CSS | Nivel | Duración | Uso |
|-----------|-------|----------|-----|
| `.wf-fiber-l1` | Backbone (Nube→Central) | 4s | Línea punteada animada |
| `.wf-fiber-l2` | Troncal (Central→OLT) | 4s | Línea punteada animada |
| `.wf-fiber-l3` | Distribución (OLT→Splitters) | 3s | Línea punteada animada |
| `.wf-fiber-l4` | Última milla (Splitters→Casas) | 2.5s | Línea punteada animada |
| `.wf-ring-pulse` | Splitters | 2.8s | Pulso expansivo con `transform: scale()` |

- `@keyframes wf-flow`: anima `stroke-dashoffset` (CSS puro)
- `@keyframes wf-ring`: anima `transform: scale()` + `opacity` (GPU-composited, Safari-safe)
- `prefers-reduced-motion: reduce` desactiva todas las animaciones del esquema
