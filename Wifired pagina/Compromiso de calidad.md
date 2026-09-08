# Compromiso de calidad

Parte de [[MEMORIA_ARQUITECTURA]] · sección solo de la vista Hogar · ver [[Vistas Hogar y Empresas]]

## Compromiso de calidad · Medidor y tarjetas

Sección `#garantias`. Se activa al entrar en pantalla con el
IntersectionObserver que **ya existía** en el archivo (no se creó otro).

### Medidor de uptime (gauge)

| Elemento | Dónde | Detalle |
|----------|-------|---------|
| Arco de relleno | `#wf-gauge` | `<circle>` r=118, perímetro 741,4 |
| Número | `<span data-count>` | Dentro del `<p>`; el `%` es un hermano aparte |

Atributos que controlan el medidor (en el `<span>` del número):

| Atributo | Valor | Qué hace |
|----------|-------|----------|
| `data-count` | `99.08` | Valor final |
| `data-decimals` | `2` | Decimales (usa coma) |
| `data-duration` | `1500` | Milisegundos de la animación |
| `data-gauge` | `#wf-gauge` | Selector del arco a mover |
| `data-gauge-arc` | `556` | Largo del arco que equivale al 100% |

- **La función `count()` fue extendida, no duplicada.** Ahora, si el elemento
  trae `data-gauge`, mueve el número Y el arco **en el mismo bucle de
  `requestAnimationFrame`**. Así nunca se desincronizan y no se agrega un
  segundo bucle al hilo principal (requisito de 60 FPS).
- `data-duration` es opcional: los demás contadores siguen usando 1100ms.
- Con **movimiento reducido** el contador no anima: pinta el valor final de una
  vez (antes no tenía esa protección).
- El HTML deja el arco en su valor final (`551 190`) como **respaldo si el JS no
  carga**; al animar, el script parte de 0. Verificado: termina en `550.9 190.5`,
  que es 556 × 99,08% — coincide con el diseño original.

### Badge "en vivo"

- `.wf-live` + `.wf-live-dot` con `@keyframes wf-ping`.
- Texto: "Sistema operativo · Monitoreo 24/7". **Reemplazó** al antiguo
  "Red monitoreada 24/7" para no repetir lo mismo dos veces.
- Es el único elemento **verde** (`#34d399`) de la página: es la convención de
  los paneles de estado. El resto de la paleta sigue siendo azul.

### Tarjetas de garantía

- `.wf-glist` / `.wf-gitem`. Los 5 textos son **exactamente los originales**.
- El `<svg>` del check usa `stroke="currentColor"` para que el color se pueda
  animar por CSS. Si lo cambias a un color fijo, el resplandor deja de funcionar.
- Efectos al pasar el cursor (verificados con valores computados):

| Efecto | Sin cursor | Con cursor |
|--------|-----------|------------|
| Elevación | `none` | `translateY(-2px)` |
| Fondo | `rgba(255,255,255,.05)` | `rgba(255,255,255,.086)` |
| Check | `rgb(181,217,253)` | `rgb(220,238,255)` + resplandor 7px |
| Etiqueta | opacidad 0 | opacidad 1 |

- `tabindex="0"` + `:focus-within` para que también funcione con teclado.
- **Etiqueta flotante `.wf-tag`:** solo aparece desde **900px de ancho Y con
  cursor real** (`@media (min-width:900px) and (hover:hover)`). En móvil y
  tablet táctil está oculta a propósito: ahí no hay hover y taparía el texto.

| Garantía | Etiqueta técnica |
|----------|------------------|
| Velocidad garantizada | Potencia óptica medida |
| Fibra propia | Tecnología GPON |
| Equipos ZTE | Wi-Fi 6 doble banda |
| Soporte local | Lun a Sáb 9:00-18:00 |
| Sin permanencia | Precio mes a mes |

