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

| Clase CSS | Nivel | Duración | Velocidad |
|-----------|-------|----------|-----------|
| `.wf-fiber` (base) | Backbone y troncal (L1/L2) | 0.8s | 17,5 u/s |
| `.wf-fiber-l3` | Distribución (OLT→Splitters) | 0.65s | 21,5 u/s |
| `.wf-fiber-l4` | Última milla (Splitters→Casas) | 0.5s | 28 u/s |
| `.wf-ring-pulse` | Splitters | 2.8s | Pulso `transform: scale(1→5)` |

- `@keyframes wf-flow`: anima `stroke-dashoffset` de 0 a **-14** = exactamente un período
  de guiones (`6 8` y `5 9` ambos suman 14) → bucle sin salto visible.
- **Por qué -14 y no -1000:** a -1000/2.5s el patrón avanzaba 47,6% del período por
  fotograma a 60Hz, casi el límite de Nyquist → efecto rueda de carreta: en pantallas
  de 60Hz se veía estático o tembloroso, en 120Hz+ se veía rápido. Ahora avanza
  2,1–3,3% por fotograma (margen 14×), fluido en cualquier tasa de refresco.
- `@keyframes wf-ring`: anima `transform: scale()` + `opacity` (GPU, Safari-safe).
  Requiere `transform-box: fill-box` para centrar el origen en el círculo.
- **Movimiento reducido:** hay 2 bloques `prefers-reduced-motion` — uno específico del
  esquema (`.wf-fiber`, `.wf-ring-pulse` → `animation: none`) y el bloque global ya
  existente de la página. No duplicar: reutilizar el global para animaciones nuevas.
