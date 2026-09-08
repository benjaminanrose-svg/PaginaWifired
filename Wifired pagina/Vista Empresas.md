# Vista Empresas

Parte de [[MEMORIA_ARQUITECTURA]] · ver también [[Vistas Hogar y Empresas]]

Se activa con el botón **EMPRESAS** de la barra superior, o entrando directo a
`tudominio.cl/#empresas` (sirve para mandarle el enlace a un cliente empresa).

## Secciones (en orden)

| # | Sección | id | Fondo | Qué muestra |
|---|---------|-----|-------|-------------|
| — | Portada | `inicio-emp` | Oscuro | Carrusel de 3 imágenes de oficina + 4 cifras |
| — | Cobertura | *(compartida)* | Oscuro | Buscador. Texto propio: "Factibilidad para tu oficina" |
| 01 | Servicios | `servicios-emp` | Claro | 7 tarjetas de servicio |
| 02 | Red mayorista | `carrier-emp` | Oscuro | Carrier neutral: POP + Colocation |
| 03 | Interconexión local | `ixp-emp` | Claro | Punto de interconexión Melipilla y Paine |
| 04 | Quiénes somos | `compromiso-emp` | Oscuro | Misión, visión y compromiso |
| 04 | Cotizar | `contratar` *(compartida)* | Claro | Formulario de empresa + mapa |

## Los 7 servicios (`.wf-ecard`)

Sacados textualmente del Word:

1. **Enlaces de fibra dedicados** — conectividad exclusiva y simétrica
2. **Enlaces microondas** — para sitios remotos, implementación rápida
3. **Servicios Cloud** — almacenamiento, aplicaciones y procesamiento
4. **Telefonía IP** — centralitas virtuales, llamadas internacionales
5. **Virtualización** — entornos virtuales de servidores y almacenamiento
6. **Cableado estructurado** — voz, datos y video
7. **Soporte y mantención TIC** — monitoreo preventivo y correctivo

## Portada de Empresas (carrusel)

Misma estructura que la portada de Hogar: imagen de lado a lado, texto
sobrepuesto abajo y puntos. Ver [[Animaciones]].

**Para editar:** arreglo **`promotionsEmpresas`** en el último `<script>`.
Mismos campos que `promotionsList` (badge, title, accent, description,
imageUrl, alt).

| # | Mensaje | Foto |
|---|---------|------|
| 1 | Conectividad crítica para tu empresa | Oficina abierta con equipo trabajando |
| 2 | Toda tu operación sobre una sola red | Equipo con notebooks en una mesa |
| 3 | Un operador que te contesta el teléfono | Dos personas en oficina |

Las 3 fotos son de Unsplash y están **verificadas mirándolas**, no solo
comprobando que la URL responda. Ver el aviso en [[Animaciones]].

## Formulario de empresa

En la vista Empresas el formulario **cambia por completo**:

| | Hogar | Empresas |
|---|-------|----------|
| Campos | Nombre, Teléfono, Dirección | Nombre, Teléfono, **Empresa**, Dirección de sucursal, **Puestos o sedes** |
| Elección | Plan (Básico/Medio/Full/Duo) | **Servicio** (7 opciones + carrier para ISP) |
| Botón | "Solicitar instalación" | "Solicitar cotización" |

## ⚠️ Regla: nada de hogar en la vista Empresas

Verificado que **no aparece ningún plan ni precio de hogar**. Si agregas algo
nuevo a una sección compartida, revisa que no hable de planes hogar sin su
`data-seg`. Los sitios compartidos donde hay que tener cuidado:

- El **formulario** de `#contratar`
- Los **enlaces del pie de página**
- La **franja de cobertura**

## Estilo

- Mismas piezas que el resto: `blueprint` con esquinas, epígrafes numerados,
  títulos en mayúscula, alternancia de fondo claro/oscuro.
- Tarjetas de servicio con ícono en recuadro y elevación al pasar el cursor.
- Ver [[Animaciones]] para el detalle de los efectos.
