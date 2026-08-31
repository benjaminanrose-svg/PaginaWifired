# WiFired Telecomunicaciones — sitio web

Landing page de WiFired (internet y TV de fibra en Melipilla).

Originalmente diseñada en **Claude Design** y **desempaquetada a HTML/CSS/JS
estándar** para poder editarla y mantenerla directamente en el repositorio,
sin runtime de React ni del editor de diseño.

## Estructura

```
index.html            La página completa: HTML + CSS (en <style>) + JS (en <script>)
assets/
  logo.png            Logo de WiFired
  fonts/*.woff2       Tipografías Barlow / Barlow Condensed (auto-alojadas, sin CDN)
```

Todo es autocontenido: no depende de ninguna CDN ni de conexión a internet
para renderizar (salvo el mapa de OpenStreetMap y los enlaces de WhatsApp,
que sí requieren red).

## Secciones

Hero con verificador de cobertura · Planes (tabs Solo internet / Internet+TV) ·
La red (esquema animado) · Garantías · Instalación · Preguntas (FAQ acordeón) ·
Contratar (formulario + contacto) · Footer.

## Interactividad (JavaScript plano, al final de `index.html`)

- **Verificador de cobertura** — busca la dirección contra la lista `COBERTURA`.
- **Tabs de planes**, **acordeón de preguntas**, **contadores** y animaciones
  reveal-on-scroll, barra de progreso, red de partículas del hero y el "ojo"
  que sigue el cursor.
- **Formulario de contratación** — hoy solo muestra confirmación en el cliente.
  El envío al backend está marcado con `TODO (back)` dentro del `<script>`.

## Cómo editar

- **Textos, colores, tipografías, layout:** editar `index.html` directamente.
  Los colores del sistema están en las variables `:root` (`--color-*`).
- **Teléfono / WhatsApp:** el número está como `56989798503` (enlaces `wa.me`)
  y `+56 9 8979 8503` (texto visible). Buscar y reemplazar en `index.html`.
- **Planes y precios:** en la sección `#planes`.
- **Preguntas frecuentes:** array `faqs` en el `<script>` — ojo, el HTML del
  acordeón también está en `#preguntas`; mantener ambos en sync si se agregan.

## Flujo de diseño

El aspecto visual se puede seguir retocando en Claude Design; al exportar una
versión nueva, se reconcilian los cambios sobre este `index.html`. La última
exportación original de Claude Design queda en el historial de git
(primer commit de la rama).
