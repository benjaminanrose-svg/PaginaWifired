# Mapa y contacto

Parte de [[MEMORIA_ARQUITECTURA]] · ver [[Datos del negocio]]

## Contacto · Mapa oscuro de la sucursal

| Elemento | Dónde | Detalle |
|----------|-------|---------|
| Contenedor | `#wf-map` | Alto 210px, fondo `#0d1520` |
| Librería | Leaflet 1.9.4 | Desde cdnjs, con SRI verificado |
| Base | Esri Dark Gray Canvas | Sin clave de API |
| Etiquetas | Esri Dark Gray Reference | Nombres de calles |
| Pin | `.wf-pin` + `wf-pinpulse` | Punto neón con 2 anillos de pulso |
| Coordenadas | `SUCURSAL` en el script | lat -33.6882825, lon -71.2168438, zoom 16 |

### Dirección de la sucursal

**C. Libertad 701 (Esq. Silva Chávez), Melipilla**

- Coordenadas obtenidas del **nodo real donde se cruzan ambas calles** en
  OpenStreetMap (consulta Overpass), no estimadas a ojo.
- Aparece en 3 lugares, mantenerlos alineados si cambia:
  1. `SUCURSAL` en el script del mapa
  2. Texto de la tarjeta de contacto
  3. `aria-label` del `#wf-map`
- La garantía "Soporte local" dice "sucursal en el centro de Melipilla".
  Es uno de los 5 textos protegidos: **no se toca**.

### ⚠️ Zoom máximo del mapa: 16

**No subir a 17.** Esri Dark Gray no tiene cartografía sobre z16 en Melipilla:
a z17 los tiles responden HTTP 200 pero la imagen dice
**"Map data not yet available"** (gris, sin calles).

Lección: comprobar que un tile *cargue* no basta — hay que **mirarlo**.
Un 200 con 256px puede ser una imagen de relleno.

- **Por qué NO se usa CartoDB Dark:** desde hace un tiempo devuelve los tiles
  con la marca de agua **"API KEY REQUIRED"** encima si no pagas clave.
  Se probó y se descartó. Esri Dark Gray es oscuro y no pide clave.
- El mapa es **de solo lectura** (sin arrastrar ni zoom) para que no secuestre
  el scroll de la página. Para explorar está "Ver mapa completo".
- La atribución "Tiles © Esri" es **obligatoria por licencia**: no quitarla.
- `.wf-pin` necesita `display:block` porque es un `<span>` y si no, el ancho y
  alto no se aplican y el pin queda invisible (ya pasó una vez).
- Si mueves el pin, cambia `SUCURSAL` **y** el enlace "Ver mapa completo".

### Jerarquía de capas (z-index) — NO romper

| Capa | z-index | Dónde |
|------|---------|-------|
| Barra de progreso | 1001 | `#wf-progress` |
| Navegación | 1000 | `<nav>` (estilo en línea) |
| Botón WhatsApp | 55 | `#wf-fab` |
| Mapa | **0** | `#wf-map` con `isolation: isolate` |

- **El bug que se corrigió:** el `<nav>` tenía z-index 50, pero Leaflet pone sus
  controles en **z-index 1000**, así que el mapa se montaba sobre la barra.
- **La solución de fondo es `isolation: isolate` en `#wf-map`**: crea un contexto
  de apilamiento propio, así nada de adentro puede treparse afuera, sin importar
  el z-index que Leaflet use internamente.
- `#wf-map .leaflet-top, .leaflet-bottom { z-index: 10 }` baja los controles.
- **NO bajar los z-index de cada `.leaflet-*-pane` por separado.** Leaflet los usa
  para apilar sus capas (tiles 200 < marcador 600). Aplanarlos esconde el pin.
- Verificado con `elementFromPoint`: con el mapa solapando la barra, al frente
  queda un enlace de la navegación, no el mapa.

### Botón "Ver mapa completo"

- Clase `.wf-maplink`. Apunta a la búsqueda directa en Google Maps:
  `https://www.google.com/maps/search/?api=1&query=Libertad+701,+Silva+Chavez,+Melipilla`
- `target="_blank"` + `rel="noopener noreferrer"` → abre pestaña nueva sin
  recargar ni afectar la landing.
- Icono SVG de enlace externo + resplandor cian al pasar el cursor.
- Incluye texto `.wf-sr` ("se abre en una pestaña nueva") para lectores de
  pantalla. `.wf-sr` es la clase de texto invisible pero anunciado.
- **Las coordenadas del pin se cambiaron** de `-33.6889, -71.2153` a
  `-33.688519, -71.216891` (las de la ficha oficial). Estaban a unos 150 m de
  distancia, así que el pin y el botón apuntaban a puntos distintos.
- **No se usó el iframe de Google Maps** (era opcional en el pedido): con iframe
  se pierde el pin neón personalizado, porque no se puede dibujar encima de un
  iframe de terceros. Se mantuvo Leaflet + Esri Dark, que ya funcionaba.

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

