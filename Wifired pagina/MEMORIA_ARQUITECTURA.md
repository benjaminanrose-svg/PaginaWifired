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

## Contacto · Horarios y estado de la sucursal

**Horarios publicados** (texto en `index.html`, bloque de contacto):
- Sucursal: **Lunes a Sábado 9:00 - 18:00 hrs**. Domingos cerrado.
- Emergencias por WhatsApp: **8:00 - 18:00 hrs**.

**Indicador dinámico** (`.wf-horario`, script "Estado de la sucursal"):

| Función | Qué hace |
|---------|----------|
| `estaAbierto()` | `true` solo Lun-Sáb entre 09:00 y 17:59. `false` domingo o fuera de rango |
| `ahoraEnChile()` | Obtiene día y hora en `America/Santiago` vía `Intl.DateTimeFormat` |
| `pintar()` | Escribe "Sucursal abierta ahora" o "Sucursal cerrada · escríbenos" |

- Usa **hora de Chile**, no el reloj del visitante: así es correcto aunque
  entren desde otro país o tengan mal la hora del equipo.
- Se refresca sola cada 60s, por si cruzan la hora de apertura o cierre con la
  página abierta.
- Si el navegador no soporta zonas horarias, cae al reloj local (respaldo).
- **OJO:** el 24/7 de las secciones "Monitoreo del nodo" y "Red monitoreada"
  se refiere al monitoreo automático de red, NO a atención de personas.
  Esos NO se tocaron y no contradicen el horario de atención.

## Contacto · Mapa oscuro de la sucursal

| Elemento | Dónde | Detalle |
|----------|-------|---------|
| Contenedor | `#wf-map` | Alto 210px, fondo `#0d1520` |
| Librería | Leaflet 1.9.4 | Desde cdnjs, con SRI verificado |
| Base | Esri Dark Gray Canvas | Sin clave de API |
| Etiquetas | Esri Dark Gray Reference | Nombres de calles |
| Pin | `.wf-pin` + `wf-pinpulse` | Punto neón con 2 anillos de pulso |
| Coordenadas | `SUCURSAL` en el script | lat -33.6889, lon -71.2153, zoom 16 |

- **Por qué NO se usa CartoDB Dark:** desde hace un tiempo devuelve los tiles
  con la marca de agua **"API KEY REQUIRED"** encima si no pagas clave.
  Se probó y se descartó. Esri Dark Gray es oscuro y no pide clave.
- El mapa es **de solo lectura** (sin arrastrar ni zoom) para que no secuestre
  el scroll de la página. Para explorar está "Ver mapa completo".
- La atribución "Tiles © Esri" es **obligatoria por licencia**: no quitarla.
- `.wf-pin` necesita `display:block` porque es un `<span>` y si no, el ancho y
  alto no se aplican y el pin queda invisible (ya pasó una vez).
- Si mueves el pin, cambia `SUCURSAL` **y** el enlace "Ver mapa completo".

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
