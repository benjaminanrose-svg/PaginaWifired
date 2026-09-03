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

## Hero · Carrusel de promociones

**DÓNDE EDITAR LAS OFERTAS:** `index.html` → último `<script>` del archivo →
arreglo **`promotionsData`**. Es el único lugar a tocar para cambiar promos.

Cada promoción tiene 4 campos:

| Campo | Qué es |
|-------|--------|
| `badge` | Texto del recuadro superior (ej: "Promo lanzamiento") |
| `titulo` | Arreglo de líneas del titular. Una línea por elemento |
| `acento` | Índice de la línea que va en azul (0 = primera). `-1` = ninguna |
| `texto` | Subtítulo descriptivo |

Agregar o quitar promociones ajusta solo los puntos y la rotación.

| Elemento | ID / Clase | Función |
|----------|-----------|---------|
| Contenedor carrusel | `#wf-hero-slider` | Envuelve las diapositivas |
| Pista | `.wf-slides` | Grid que apila todas las slides en la misma celda |
| Diapositiva | `.wf-slide` / `.is-active` | Fade 500ms; solo la activa es visible |
| Puntos | `#wf-hero-dots` / `.wf-dot` | Barras finas sobre el buscador |
| Barra progreso | `@keyframes wf-dotfill` | 6s lineal; su fin dispara el cambio |

- **Autoplay:** 6s. No usa temporizador: lo dispara el `animationend` de la barra
  del punto activo, así el indicador y la diapositiva nunca se desincronizan.
- **Pausas** (se acumulan, se reanuda cuando no queda ninguna): cursor encima,
  foco dentro, pestaña oculta, hero fuera de pantalla.
- **Teclado:** flechas ← → sobre los puntos.
- **Movimiento reducido:** sin autoplay, sin fade; los puntos siguen funcionando.
- **Sin salto de maquetación:** todas las slides comparten la celda `1 / 1` del
  grid, así la altura es la de la más alta y nada se mueve al cambiar.
- **NO SE TOCA:** `#cobertura` (buscador) ni las métricas del pie son parte del
  carrusel; quedan fijas. Verificado que el buscador sigue funcionando igual.
- El HTML trae la 1ª diapositiva escrita como **respaldo si el JS no carga**;
  al iniciar, el script reconstruye todo desde `promotionsData`.

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
