# Mapa de Arquitectura — WiFired

Índice del proyecto. **Empieza siempre por acá** antes de buscar en el código.

## Notas

| Nota | Qué contiene |
|------|--------------|
| [[Datos del negocio]] | Identidad, dirección, horarios, cobertura, planes vigentes |
| [[Vistas Hogar y Empresas]] | Cómo funciona el cambio entre las dos páginas + la cabecera |
| [[Vista Empresas]] | Todo lo de la página de empresas |
| [[Compromiso de calidad]] | Medidor de uptime y tarjetas de garantía (solo Hogar) |
| [[Planes]] | Pestañas (internet · TV · antena · DGO), tarjetas DGO y carrusel |
| [[Mapa y contacto]] | Mapa oscuro, pin, horarios dinámicos, enlace a Google Maps |
| [[Animaciones]] | Esquema de red, carrusel del hero y efectos |

## Archivos del proyecto

| Módulo | Ruta | Función |
|--------|------|---------|
| Página completa | `index.html` | Todo el sitio en un archivo (HTML + CSS + JS) |
| Reglas de trabajo | `CLAUDE.md` | Las 4 reglas del proyecto |
| Logo | `assets/logo.png` | Logo a color en blanco |
| Favicon | `assets/favicon.png` | Ícono del navegador |
| Ícono iOS | `assets/apple-touch-icon.png` | Para iPhone |
| Imagen al compartir | `assets/og-image.png` | WhatsApp, Facebook, LinkedIn |
| Términos y condiciones | `assets/terminos-y-condiciones.pdf` | **Provisorio**: solo dice el título. Se descarga con `download` desde el menú del celular y el pie. Para reemplazarlo, sube el PDF real con el **mismo nombre** |
| Fuentes | `assets/fonts/*.woff2` | Barlow y Barlow Condensed |
| Servidor local | `.claude/launch.json` | `npx serve` en el puerto 8080 |
| Esta bóveda | `Wifired pagina/` | Notas del proyecto (no va al repo) |

## Secciones del sitio, en orden

| id | Vista | Qué es |
|----|-------|--------|
| `inicio` | Hogar | Portada con carrusel de promos |
| `inicio-emp` | Empresas | Portada de empresas |
| *(sin id)* | Ambas | Franja del buscador de cobertura · **OCULTA** (`hidden`), aún no se desarrolla |
| `planes` | Hogar | Los planes con precios |
| `red` | Hogar | Esquema de la red (WF-NODO-01) |
| `garantias` | Hogar | Compromiso de calidad |
| `instalacion` | Hogar | Del sí a la conexión en 48 h · 3 pasos `.wf-pasos`: en fila en escritorio, **apilados en celular** |
| `preguntas` | Hogar | Preguntas frecuentes |
| `servicios-emp` | Empresas | Los 7 servicios |
| `carrier-emp` | Empresas | Carrier neutral, POP y colocation |
| `ixp-emp` | Empresas | Punto de interconexión local |
| `compromiso-emp` | Empresas | Misión, visión y compromiso |
| `contratar` | Ambas | Contacto, mapa y formulario |

## Avisos importantes

- ⚠️ **Los planes del Word están obsoletos.** Mandan los de la página: 400 / 650 /
  940 Mbps. Ver [[Datos del negocio]].
- ⚠️ **El zoom del mapa no debe pasar de 16.** Ver [[Mapa y contacto]].
- ⚠️ **Nada de planes de hogar en la vista Empresas.** Ver [[Vista Empresas]].
- ⚠️ **La imagen del hero va fuera del flujo**, si no rompe la portada.
  Ver [[Animaciones]].
