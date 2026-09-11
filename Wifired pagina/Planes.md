# Planes (vista Hogar)

Parte de [[MEMORIA_ARQUITECTURA]] · ver [[Datos del negocio]] para precios vigentes

Sección `#planes` con `data-seg="hogar"`.

## Estructura

| Pieza | Selector | Notas |
|-------|----------|-------|
| Selector de pestañas | `.seg` > `.seg-opt[data-tab]` | "Solo internet" / "Internet + TV" |
| Grilla de planes | `.wf-plans[data-plan="internet"]` y `[data-plan="tv"]` | La de TV parte con `hidden` |
| Tarjeta de plan | `.blueprint.wf-lift` | Esquinas `i.corner`, relleno 30px |
| Plan destacado | mismo + `border-color:var(--color-accent)` | El Full, con cinta "Más elegido" |
| ~~Aviso empresas~~ | **ELIMINADO** a pedido | Hogar ya no ofrece empresas; eso vive en [[Vista Empresas]] (bloque Cotizar) |

- `.wf-lift:hover` sube la tarjeta 6px con sombra.
- El script que cambia de pestaña busca `.wf-plans[data-plan]`.

## Anatomía de una tarjeta de plan

1. Epígrafe gris ("Banda ancha") · 11px, mayúsculas
2. Nombre del plan · Barlow Condensed 26px
3. Velocidad grande · `clamp(44px,5vw,58px)` en `--color-accent-800`
4. Barra de velocidad · 5px de alto
5. Precio · `clamp(32px,3.4vw,42px)`
6. Lista con guion "—" en azul
7. Botón `btn btn-secondary btn-block` (el destacado usa `btn-primary`)

## Pestañas del selector (4)

**Solo internet · Internet + TV · Antena · DGO**. Cada `input[name="wf-tab"][data-tab="X"]`
muestra el panel `.wf-plans[data-plan="X"]` y oculta los demás. El script de
pestañas es genérico: **para agregar una pestaña nueva basta un `label.seg-opt`
más y un panel `.wf-plans` con el mismo nombre** (con `hidden`). No tocar el script.

- En celular (≤640px) el selector se desliza de lado para que quepan las 4.
- Antena y DGO **no van apiladas bajo los planes**: van en su pestaña (pedido del usuario).

## Formulario de contratación (vista Hogar)

"Plan que te interesa" en `#contratar` muestra **todos los planes** en un acordeón
por categoría: **Solo internet (3) · Internet + TV (3) · Antena (1) · DGO (4)**.
- Reutiliza el acordeón `[data-acordeon]` de Empresas con el modificador
  `.wf-acc--form` (versión compacta). No hay script nuevo.
- Todas las opciones comparten `name="wf3-plan"` → **se elige un solo plan**. Cada
  una lleva `value` con el nombre del plan (para cuando el formulario envíe).
- Por defecto: Solo internet abierto, con Medio 650 marcado.
- La categoría que tiene el plan elegido muestra "· elegido" aunque esté cerrada
  (`:has(input:checked)`).
- Las opciones de categorías cerradas quedan con `visibility:hidden` para que no
  se pueda llegar a ellas con Tab.
- ⚠️ Si cambias un precio en las tarjetas de planes, **cámbialo también aquí**:
  el formulario repite los precios.

| Categoría | Planes y precios |
|-----------|------------------|
| Solo internet | Básico 400 $13.990 · Medio 650 $19.990 · Full 940 $29.990 |
| Internet + TV | Duo Básico 400 $21.990 · Duo Medio 650 $27.990 · Duo Full 940 $35.990 |
| Antena | Según factibilidad |
| DGO | Lite + DSports $7.200 · Streaming $22.900 · Fútbol $18.900 · Fútbol Total $21.900 |

## Planes de internet y TV en celular: deslizables

En escritorio las grillas `.wf-plangrid` son 3 columnas. **En celular (≤720px) se
deslizan de lado** (tarjeta al 86% para que se asome la siguiente) en vez de
apilarse, así no alargan la página.
- No hay código nuevo: cada panel `.wf-plans` (internet y tv) lleva `data-rail` y
  sus flechas; la grilla lleva `data-rail-track`. Usa el mismo script del carrusel DGO.
- Relleno 16px arriba/abajo y 10px a los lados (con `scroll-padding-inline: 10px`)
  para no cortar la cinta "Más elegido" (sobresale ~12px) ni las esquinas `+`.
- La grilla ya no tiene estilo en línea: se controla con la clase `.wf-plangrid`.
- ⚠️ Las flechas solo aparecen si la fila **se desliza de verdad** (`overflow-x`
  auto). En escritorio las esquinas `+` sobresalen ~6px y antes hacían aparecer una
  flecha sin motivo.
- El carrusel se recalcula también al cambiar de pestaña de planes.
- Verificado en 375px: una fila, tarjeta al ~81%, cinta sin cortar, sección de
  ~1.600px a 520px de alto.

## Carrusel horizontal (`[data-rail]`)

La fila DGO es un carrusel: 4 tarjetas visibles en escritorio; si no caben, se
desliza con flechas o con el dedo. **Nunca pasa a doble fila.**
- Marcas: `div.wf-rail[data-rail]` > `button[data-rail-prev|next]` + `.wf-dgo-grid[data-rail-track]`.
- Las flechas se ocultan solas si todo cabe. Script "CARRUSEL HORIZONTAL" al final
  del archivo, reutilizable para cualquier otra fila de tarjetas.
- ⚠️ La pista lleva `scroll-padding-inline` **igual a su relleno lateral (2px)**. Sin
  eso, el ajuste magnético deja el scroll en 2px y la flecha "atrás" aparece al
  inicio. Si cambias el relleno, cambia también el scroll-padding.
- Verificado: 4 pestañas funcionan, DGO en 1 sola fila, sin errores en consola.

## Contenido de las pestañas nuevas

### Internet por antena (`.blueprint.wf-antena`)

Tarjeta horizontal en 3 columnas (texto · lista · precio y botón); en celular se apila.
Texto del Word: "internet sin cables, ideal para zonas sin acceso a fibra".
- **Sin precio**: dice "Precio según factibilidad". Botón "Consultar" → `#contratar`.
- ⚠️ No afirmar "equipos en comodato" para antena: el Word solo lo dice de fibra.

### Suma DGO (`.wf-dgo-grid` > `article.wf-dgo`)

Imitan las tarjetas de directvgo.com/cl pero con nuestro estilo (esquinas rectas,
fondo claro, Barlow, azul WiFired). Anatomía de cada tarjeta:

1. `.wf-dgo-media` — foto 150px con degradado y texto encima "DGO · PLAN / NOMBRE"
2. `h4` nombre · `.wf-dgo-pre` ("Solo" o precio tachado + `.wf-dgo-badge`)
3. `.wf-dgo-price` precio grande "/ mes"
4. Botón Contratar → `#contratar`
5. `.wf-dgo-inc` "Incluido" con `.wf-chip` (texto, **no logos**)
6. `.wf-dgo-list` 3 puntos · `.wf-dgo-more` "Detalles del plan ↗" → directvgo.com/cl

| Plan | Precio | Antes | Incluye |
|------|--------|-------|---------|
| Lite + DSports | $7.200 | — | DSports |
| Streaming | $22.900 | — | DSports, Amazon Prime, Disney+, HBO Max, +2 |
| Fútbol | $18.900 | $21.300 | DSports, Amazon Prime, TNT Sports, NFL Game Pass |
| Fútbol Total | $21.900 | $23.900 | DSports, Amazon Prime, TNT Sports, NFL Game Pass |

- ⚠️ **Precios referenciales** copiados de DGO (sept. 2026). La tarjeta lo avisa.
  Confirmar con el usuario si WiFired los vende a ese precio.
- ⚠️ **No usar logos de DGO ni de los canales** (marcas de terceros): van como texto.
- Fotos (verificadas mirándolas): balón `photo-1579952363873…`, smart TV
  `photo-1593784991095…`, estadio `photo-1522778119026…`, cancha nocturna
  `photo-1431324155629…`. Todas `loading="lazy"`.
