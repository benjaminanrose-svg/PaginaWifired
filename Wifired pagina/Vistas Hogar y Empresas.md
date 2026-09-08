# Vistas Hogar y Empresas

Parte de [[MEMORIA_ARQUITECTURA]] · ver [[Vista Empresas]] y [[Datos del negocio]]

## Cabecera de 2 pisos

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

