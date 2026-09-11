# Vistas Hogar y Empresas

Parte de [[MEMORIA_ARQUITECTURA]] · ver [[Vista Empresas]] y [[Datos del negocio]]

## Cabecera flotante (estilo DGO)

⚠️ **La barra de datos de arriba (`.wf-topbar`) se ELIMINÓ** a pedido, con su
horario, teléfono y "Desde 2017". Ahora la cabecera es **un solo piso**.

**Selector Hogar | Empresas** (`.wf-navseg` > `.wf-segs` > `button.wf-seg[data-seg-btn]`):
- Escritorio: columna **derecha** del menú, a la altura del logo (no encima).
- Celular (≤720px): fila completa **debajo** del logo, los dos botones a medias.
- El script de vistas no cambió: sigue buscando `[data-seg-btn]`.

## Menú lateral en celular (hamburguesa)

En celular (≤720px) la barra muestra solo **logo + botón hamburguesa**. El botón
abre un panel lateral derecho con: selector Hogar/Empresas · enlaces del menú ·
Contratar · contacto (Melipilla, Paine y horario).

| Pieza | Selector | Nota |
|-------|----------|------|
| Contenedor | `div.wf-menu#wf-menu` | Envuelve `.wf-navlinks` y `.wf-navseg` |
| Botón | `button.wf-burger` | 3 líneas → X; `aria-expanded` |
| Fondo oscuro | `div.wf-menu-bg` | Clic cierra el menú |
| Contacto | `div.wf-menu-extra` | Solo existe en el panel del celular |
| Estado | `.wf-header.menu-open` | Lo pone el script "MENÚ LATERAL" |

- **No se duplicó el menú:** en escritorio `.wf-menu` usa `display:contents`, así
  sus hijos siguen ocupando las columnas de la barra como antes.
- ⚠️ Con el menú abierto se quita el `backdrop-filter` de `.wf-nav`. Si no, el
  desenfoque convierte a la barra en el "marco" del panel fijo y el panel queda
  atrapado dentro de la barra (77px de alto) en vez de ocupar la pantalla.
- Con el menú abierto la página no se desplaza (`html.wf-lock`).
- Al abrir, el foco va al panel entero (`tabindex="-1"`, sin contorno), no al primer
  enlace: si no, "Servicios"/"Planes" aparecía marcado con un recuadro.
- Verificado en 375px: panel de 812px de alto (pantalla completa), cambia de vista y
  se cierra; en 1400px la hamburguesa no aparece y el menú sigue centrado.
- Se cierra con el fondo oscuro, Esc, al elegir una opción o al pasar a escritorio.
- Los enlaces cambian solos según la vista: son los mismos `[data-seg]` de siempre.

`<header class="wf-header">`. **Transparente sobre la portada, sólida al bajar.**

| Estado | Cuándo | Logo | Fondo |
|--------|--------|------|-------|
| Transparente | scroll ≤ 24px | `clamp(54px,5.6vw,78px)` | degradado oscuro `::before` |
| `.is-solid` | scroll > 24px | `clamp(44px,3.8vw,54px)` | azul 86% + desenfoque |

- La clase `.is-solid` la pone el `onScroll` que **ya existía** para la barra de
  progreso (no se creó otro detector de scroll).
- Menú en grilla de 3 columnas: `.wf-brand` (logo, izquierda) · `.wf-navlinks`
  (centrado de verdad) · tercera columna vacía para que el centro sea exacto.
- **Contratar va DENTRO del menú central** (`a.btn.wf-navbtn`), como parte de la
  barra flotante. Ya no existe `.wf-navcta`.
- Contratar **no tiene fondo celeste**: fondo transparente, borde claro y texto
  claro. Al pasar el cursor el borde y el texto se vuelven cian.
- Bajo 720px: los enlaces de texto se ocultan (`.wf-navlink`) pero el grupo sigue
  visible, así **Contratar queda a la derecha en celular**.
- El logo ya **no lleva estilo en línea**: se controla con `.wf-logo`.
- Verificado: arriba fondo `rgba(0,0,0,0)` y logo 78px; al bajar logo 53px.
- El **ojo animado se eliminó** a pedido (HTML, CSS y script `initEye`).

### Botones que llevan a una sección (anclas)

`section[id] { scroll-margin-top: var(--wf-head) }`: la sección queda **justo bajo
la cabecera**, sin franja de la sección anterior.
- `--wf-head` lo mide `medirCabecera()` (junto al `onScroll`): pone la cabecera
  sólida un instante **sin animación** (`.wf-medir`), mide su alto y la restaura.
  Se mide la versión sólida porque al llegar a la sección la cabecera ya lo está.
  Se recalcula al cambiar el tamaño de la ventana. Respaldo: 80px.
- Antes era un 112px fijo (de la cabecera de 2 pisos) y dejaba todo ~40px más arriba.
- **Se desliza, no salta** (pedido del usuario): script propio junto a
  `medirCabecera()` que atrapa los clics en `a[href^="#"]` y anima el scroll con
  una curva que acelera y frena (450–1100 ms según la distancia).
  - ⚠️ Se hizo a mano porque `scroll-behavior: smooth` del CSS se apaga cuando el
    equipo tiene "reducir animaciones" activado (común en Windows).
  - Se detiene si la persona mueve la rueda, toca la pantalla o usa el teclado.
  - **No cambia la dirección** (#red, etc.): así no se pierde el `#empresas`.
  - Durante la animación se pone `scroll-behavior:auto` en `<html>` para que cada
    paso no se "suavice" dos veces.
- Verificado: todos los botones de Hogar, Empresas y del menú lateral dan
  diferencia 0 px (celular 375px → 67px; escritorio 1400px → 74px).

## Franja de cifras bajo las portadas

`.wf-metrics` en ambas portadas: de borde a borde, 4 columnas iguales, datos
centrados. En celular pasa a 2×2. Las celdas traen relleno en línea antiguo, por
eso el CSS usa `!important` en el padding. Verificado: 4 celdas de 346px a 1385px.

## ⚠️ Cobertura OCULTA

La franja del buscador de cobertura y el enlace "Cobertura" del menú tienen
`hidden` porque **aún no se desarrolla**. Para volver a mostrarla: quitar `hidden`
de la `<section>` (tiene un comentario encima) y del enlace del menú.
Ningún botón visible apunta a `#cobertura` (el de antena va a `#contratar`).

## Cabecera de 2 pisos (HISTORIA · ya no existe)

> Todo lo de abajo describe la versión anterior. La barra de datos se eliminó;
> ver "Cabecera flotante" arriba.

`<header>` fijo arriba con `z-index:1000`. Alto total ≈ **109px**.

| Piso | Clase / etiqueta | Alto | Contenido |
|------|------------------|------|-----------|
| 1 · Datos | `.wf-topbar` | 32px | "Desde 2017 · Fibra óptica y enlaces inalámbricos en Melipilla y Paine" · horario · teléfono |
| 2 · Navegación | `<nav>` dentro del header | 77px | Logo, ojo animado, Planes / Cobertura / La red, botón Contratar |

- El `position:fixed` y el `z-index` viven ahora en el `<header>`, no en el `<nav>`.
  El `<nav>` conserva su fondo con desenfoque.
- **Si cambias el alto de la cabecera, ajusta también:**
  1. `section[id] { scroll-margin-top: 112px }` — si no, las anclas quedan tapadas
  2. `#cobertura` → `scroll-margin-top: 130px`
  3. Relleno superior del hero → `clamp(124px,11vw,148px)`
- Cortes responsivos: bajo 900px se oculta el texto de la izquierda; bajo 560px
  se oculta también el horario y queda solo el teléfono.
- El punto verde reutiliza el color `#34d399` del badge "en vivo".


## Vistas Hogar / Empresas (dos páginas en paralelo)

El sitio son **dos páginas dentro del mismo archivo**. Se cambia con el selector
de la barra superior. Script al final de `index.html`.

### Cómo funciona

| Atributo | Dónde | Qué hace |
|----------|-------|----------|
| `data-seg="hogar"` | secciones y enlaces | Solo se ve en la vista Hogar |
| `data-seg="empresas"` | secciones y enlaces | Solo se ve en la vista Empresas |
| *(sin atributo)* | — | **Se ve siempre** (cobertura, contacto, pie) |
| `data-vista` | en `#wf-root` | Guarda la vista activa |

- **Para agregar una sección nueva basta ponerle su `data-seg`.** El script la
  toma sola, no hay que tocar código.
- Las de Empresas llevan `hidden` en el HTML para que no parpadeen al cargar.
- El cambio usa el atributo `hidden` (no `display` por CSS), así cada sección
  recupera su display natural — importante porque el hero es `flex`.
- La dirección refleja la vista: `#empresas` abre directo la vista de empresas,
  así se puede compartir el enlace.

### Qué hay en cada vista

| Vista | Secciones |
|-------|-----------|
| Hogar | `#inicio`, `#planes`, `#red`, `#garantias`, `#instalacion`, `#preguntas` |
| Empresas | `#inicio-emp`, `#servicios-emp`, `#carrier-emp`, `#ixp-emp` |
| Compartidas | franja de cobertura, `#contratar`, pie de página |

- Los enlaces del menú también cambian: Hogar muestra Planes / La red;
  Empresas muestra Servicios / Red mayorista. Cobertura y Contratar siempre.

### Contenido de Empresas (sacado del Word)

- **7 servicios** (`.wf-ecard`): enlaces de fibra dedicados, enlaces microondas,
  Cloud, telefonía IP, virtualización, cableado estructurado, soporte TIC.
- **Carrier neutral** con POP de interconexión y Colocation (`.wf-elist`).
- **Punto de interconexión local** en Melipilla y Paine (`.wf-ixp`).
- ⚠️ **Las velocidades del Word (300/600/900) están obsoletas.** Mandan las de
  la página: **400 / 650 / 940 Mbps**. No copiar cifras de planes desde el Word.


## Franja de cobertura (buscador)

**El buscador se sacó del hero** y ahora vive en su propia franja oscura, entre
el hero y la sección Planes.

- Conserva el `id="cobertura"`, así el enlace **COBERTURA del menú sigue
  funcionando**. Verificado que al pulsarlo no queda tapado por la cabecera.
- Se mantuvo el fondo oscuro porque el bloque está diseñado para eso: si se
  mueve a una sección clara (como Contratar) el texto queda blanco sobre blanco.
- El JS del verificador **no se tocó**: sigue usando `.wf-checkinput`,
  `.wf-buscando`, `.wf-resultado` y el arreglo `COBERTURA`.

