# Animaciones

Parte de [[MEMORIA_ARQUITECTURA]] · efectos de todo el sitio

## Esquema de red WF-NODO-01 (coordenadas)

SVG `viewBox="0 0 520 260"` en la sección `#red`. Recorrido:
**Nube PIT → Central WiFired → OLT → Splitter principal 1:2 → Splitters 1 y 2 (1:8) → 6 casas**.

| Pieza | Posición | Nota |
|-------|----------|------|
| Nube PIT | `translate(12,96) scale(2)` · centro x 36 | |
| Central WiFired | grupo con `translate(-24,0)` · centro x 116 | Antes decía "Central Melipilla" |
| OLT | rect x 172–218 · texto x 195 | |
| Splitter principal | círculo (261,120) · etiqueta "SPLITTER 1:2" | Agregado a pedido |
| Splitter 1 / 2 | (340,62) y (340,192) · "1:8" | |
| Casas | x 448, en y 22/54/86 y 152/184/216 | |

Tramos animados: L1 `M60→96`, L2 `M136→170`, L3 OLT→principal `M220→255` y
principal→splitters (curvas desde x 267). La relación 1:2 del principal la puse yo
porque reparte a exactamente dos splitters; confirmar si es otra.

## Fondo de partículas de la portada (`#wf-net`, `initNet`)

- ⚠️ **Se redibuja con `ResizeObserver` sobre el lienzo**, no solo con el evento
  `resize` de la ventana. Antes, si la portada cambiaba de alto por otra causa, el
  dibujo quedaba estirado y los brillos se veían como **manchas alargadas**.
- `sincronizar()` solo reconstruye si el tamaño cambió más de 1px.
- Con la portada oculta (vista Empresas) mide 0: `frame()` no dibuja y espera.
- Con movimiento reducido pinta una vez en estático (`pintarEstatico`).

## El carrusel se usa DOS veces

La función **`montarCarrusel(idSeccion, lista)`** monta un carrusel dentro de la
sección que se le pase. Se llama dos veces al final del script:

```
montarCarrusel('inicio', promotionsList);         // portada Hogar
montarCarrusel('inicio-emp', promotionsEmpresas); // portada Empresas
```

Cada portada marca sus piezas con **atributos, no con id**, para que la función
las encuentre dentro de su propia sección:

| Atributo | Va en |
|----------|-------|
| `data-carrusel-textos` | El div que contiene `.wf-slides` |
| `data-carrusel-puntos` | El div de los puntos `.wf-dots` |
| *(clase)* `.wf-promo-media` | El contenedor de las imágenes |

⚠️ **Si agregas una portada nueva, no dupliques el script:** ponle esos
atributos y agrega una llamada más a `montarCarrusel`.

⚠️ **Al elegir fotos de Unsplash, MÍRALAS antes de usarlas.** Que la URL
responda 200 no basta: la foto puede no existir (404) o ser una imagen de
relleno. Ya pasó dos veces. Hay que descargarla y abrirla.

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

## Hero · Banner a pantalla completa

La imagen ocupa **de lado a lado** (borde a borde) y todo el alto libre bajo la
cabecera. El texto de la promoción va **sobrepuesto en la parte de abajo**.

| Elemento | Clase | Función |
|----------|-------|---------|
| Escenario | `.wf-hero-stage` | `flex:1`, ocupa el alto libre |
| Tarjeta | `.wf-promo-card` | Sin bordes ni esquinas redondeadas dentro del hero |
| Imagen | `.wf-promo-media` | `flex:1`, alto mínimo `clamp(420px,62svh,680px)` |
| Texto | `.wf-hero-caption` | Sobrepuesto abajo, contenido a 1240px centrado |
| Puntos | `.wf-dots` | Abajo al centro; a la derecha desde 980px |

- El velo `.wf-promo-card::after` se **oscureció abajo** (94% al pie) para que el
  texto se lea sobre cualquier foto.
- ⚠️ **La imagen va `position:absolute; inset:0`, fuera del flujo.** Si se deja en
  el flujo, impone su proporción natural (1000×667) y en pantallas anchas estira
  el hero: a 1585px de ancho pedía 1058px de alto y empujaba el título fuera de
  la pantalla (solo se veía la foto). El alto lo manda el `flex` de la sección.
- El alto mínimo va en `.wf-hero-stage`, **no** en `.wf-promo-media`.
- La misma clase `.wf-promo-card` se usa en el hero y (antes) en tarjeta chica.
  Dentro del hero se anula el radio y los bordes con `.wf-hero-stage .wf-promo-card`.
- Las clases `.wf-hero-grid` / `.wf-hero-left` / `.wf-hero-right` **ya no se usan**.

## Franja de cobertura (buscador)

**El buscador se sacó del hero** y ahora vive en su propia franja oscura, entre
el hero y la sección Planes.

- Conserva el `id="cobertura"`, así el enlace **COBERTURA del menú sigue
  funcionando**. Verificado que al pulsarlo no queda tapado por la cabecera.
- Se mantuvo el fondo oscuro porque el bloque está diseñado para eso: si se
  mueve a una sección clara (como Contratar) el texto queda blanco sobre blanco.
- El JS del verificador **no se tocó**: sigue usando `.wf-checkinput`,
  `.wf-buscando`, `.wf-resultado` y el arreglo `COBERTURA`.

