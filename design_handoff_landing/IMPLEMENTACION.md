# Especificación de implementación · Landing de cultay

Documento para quien implementa. Autosuficiente: no hace falta haber estado en la conversación de diseño. El *por qué* de cada decisión está en `README.md`; aquí está el *qué* y el *cuánto*.

---

# 0 · Antes de escribir código

## Qué son estos archivos

`Cultay Landing.html` + `landing.css` son una **referencia de diseño hecha en HTML**: un prototipo que muestra el aspecto y el comportamiento previstos. **No es código de producción y no se sube tal cual.** El trabajo es **recrear este diseño en el entorno del proyecto** (Astro, Next, Nuxt, Svelte, HTML estático con un builder… lo que ya use `cultay.com`) con sus patrones, su sistema de componentes y su pipeline de assets. Si no hay entorno todavía, elige el más adecuado para una landing estática — una landing es HTML, CSS y ~90 líneas de JS: no necesita framework de aplicación.

Dicho eso: **el CSS sí es aprovechable casi literalmente.** No hay dependencias, no hay preprocesador, no hay utilidades. Copiar `landing.css` y trocearlo por componentes es una vía perfectamente válida.

## Fidelidad

**Alta fidelidad.** Colores, tipografías, espaciados, radios y transiciones son definitivos: reprodúcelos al píxel. Lo único que es dato de ejemplo son los títulos de obra, las portadas y las cifras del bloque de estadísticas.

## Las tres cosas que NO deben llegar a producción

| Elemento | Qué hacer |
|---|---|
| `image-slot.js` y todos los `<image-slot>` | Herramienta de prototipado (permite arrastrar portadas encima). Sustituir por `<img>` con las portadas reales. **La página no se publica con portadas vacías.** |
| La captura de app en HTML (`.viewport > .app`) y su `fitShot()` | Es una maqueta del producto construida en HTML porque aún no hay producto. Cuando `dev.cultay.app` tenga la pantalla de Biblioteca, sustituir todo el interior de `.viewport` por un `<img>` (o `<picture>`) y borrar `fitShot()`. El contenedor `.shot` y su barra no cambian. Ver §6. |
| Fuentes desde `fonts.googleapis.com` | Autoalojar. Ver §12. |

---

# 1 · Estructura de la página

Una sola página, un solo destino. Todos los CTA apuntan al mismo formulario (`#beta`). No hay subpáginas ni enlaces salientes salvo privacidad, correo y redes.

| # | id | Fondo | Sección |
|---|---|---|---|
| — | — | oscuro translúcido | Nav sticky |
| 01 | `#top` | oscuro `--surface` | Hero + tarjeta de resultado |
| 02 | `#demo` | oscuro | El motor · demo interactiva |
| 03 | — | **claro** `.hueso` | Los tres formatos |
| 04 | `#app` | oscuro | La app · captura + 2 paneles |
| 05 | — | **claro** `.hueso` | Tu perfil · tarjetas de cifra |
| 06 | `#espanol` | oscuro | La diferencia · 2×2 |
| 07 | `#faq` | oscuro | Dudas · FAQ |
| 08 | `#beta` | **vermut** `#B22838` | Formulario de beta |
| — | — | `#1B0B17` | Footer |

La alternancia oscuro → claro → oscuro → claro → oscuro es intencionada y no se toca: da ritmo con solo dos fondos. Las dos bandas claras y la banda vermut del cierre son los únicos cambios de fondo permitidos.

**No hay conmutador de tema en la página.** La landing es oscura, punto. El único elemento con tema conmutable es la maqueta del producto de la sección 04, donde el modo claro/oscuro se *demuestra* como función del producto. No se atiende `prefers-color-scheme`: es deliberado.

---

# 2 · Tokens

Se declaran en `:root`. **Ojo al planteamiento invertido respecto a la app**: en la app los tokens claros son la base y `[data-theme=dark]` los sobreescribe; en la landing es al revés — la base es oscura y la clase `.hueso` reintroduce los claros por sección.

```css
:root{
  /* superficies */
  --surface:#241120;   /* berenjena — fondo base */
  --surface2:#381B2A;  /* bandas, tarjetas, barra del navegador falso */
  --paper:#301727;     /* campos, bloque «Por qué esta» */
  /* texto */
  --ink:#F2E7CE;
  --muted:rgba(242,231,206,.62);
  --faint:rgba(242,231,206,.4);
  /* acentos */
  --primary:#F47265;   /* coral — acento sobre oscuro */
  --primaryD:#E0574A;  /* pulsado */
  --vermut:#B22838;    /* banda del cierre */
  --pist:#BFCB5C;
  --mant:#E8BE5A;
  /* líneas */
  --line:rgba(242,231,206,.18);
  --line2:rgba(242,231,206,.09);
  /* tipografía */
  --display:'Bricolage Grotesque',sans-serif;
  --ui:Geist,sans-serif;
  --mono:'Geist Mono',monospace;
  /* forma característica del sistema */
  --shape:26px 26px 26px 5px;
}
.hueso{
  --surface:#F2E7CE; --surface2:#E5D5B5; --paper:#FBF4E4;
  --ink:#1F1612; --muted:rgba(31,22,18,.56); --faint:rgba(31,22,18,.38);
  --primary:#B22838; --primaryD:#8E1D2C;
  --line:rgba(31,22,18,.14); --line2:rgba(31,22,18,.08);
}
```

Color de fondo del footer, fuera de tokens: `#1B0B17`.

`--shape: 26px 26px 26px 5px` es la forma característica del sistema: **la esquina inferior izquierda a 5 px y las otras tres a 26**. Va en tarjetas, paneles y bloques. No redondear las cuatro por igual.

## Escala tipográfica

| Clase | Familia / peso | Tamaño | line-height | letter-spacing |
|---|---|---|---|---|
| `.xl` | Bricolage 700 | `clamp(46px,7.2vw,104px)` | .9 | −.055em |
| `.lg` | Bricolage 700 | `clamp(36px,4.6vw,64px)` | .9 | −.045em |
| `.md` | Bricolage 700 | `clamp(28px,3.2vw,42px)` | .98 | −.04em |
| `.sm` | Bricolage 700 | 23px | 1.15 | −.035em |
| `.cap` | Geist Mono 500 | 11px | — | .13em, uppercase, `--faint` |
| `.lead` | Geist 400 | `clamp(17px,1.35vw,19.5px)` | 1.5 | — · `--muted` |
| cuerpo | Geist 400 | 15–16px | 1.5–1.58 | — |
| `.data` | Geist Mono 400 | 12px | — | .04em |

**El punto vermut**: todos los titulares terminan en `<i>.</i>` — un punto en `--primary`, `font-style:normal`. Es la firma de la marca y va en cada `h1`/`h2`/`h3` de sección. En la banda vermut del cierre, ese punto va en `#E8BE5A` (mantequilla), porque el coral no contrasta contra el vermut.

## Espaciado

Rejilla base 1200 px de contenido con 40 px de padding lateral (`.wrap`). Secciones a 132 px arriba y abajo (`.sec`), 96 px en la variante `.sec.tight` (la FAQ) y 96 px en todas por debajo de 900 px.

Radios: `--shape` en tarjetas y paneles · 6 px en botones y campos · 5 px en botones `.sm` · 3 px en portadas · 12 px en el marco del navegador falso · 18–20 px en chips.

**Cero sombras en toda la página.** La profundidad viene del degradado radial interno de las tarjetas de cifra y de los solapes. Si aparece un `box-shadow`, está mal.

---

# 3 · Componentes base

## Botón `.btn`

```
alto 52 · padding 0 24 · radio 6 · Geist 15.5px/600 · gap 9 con el icono
transición: background .14s, border-color .14s, color .14s
variante .sm: alto 40 · padding 0 16 · 14px · radio 5
```

| Variante | Reposo | Hover |
|---|---|---|
| `.p` primario | fondo `--primary`, texto `#2A1420` (en `.hueso`: `#F7EFDC`) | fondo `--primaryD` |
| `.o` outline | borde 1px `--ink`, texto `--ink` | fondo `--ink`, texto `--surface` |
| `.g` ghost | texto `--muted`, padding 0 8 | texto `--ink` |
| `.inv` (solo banda vermut) | fondo `#F7EFDC`, texto `#B22838` | fondo `#fff` |

## Chip de la demo `.chip`

`alto 40 · padding 0 17 · radio 20 · borde 1px --line · 14.5px`. Hover: borde a `--ink`. **Activo** (`aria-pressed="true"`): fondo y borde `--primary`, texto `#2A1420`, peso 600.

## Etiqueta descriptiva `.tag`

`Geist Mono 11px · ls .08em · uppercase · borde 1px --line · radio 13 · padding 6 12 · color --muted · cursor:default`.

**No es pulsable y no debe parecerlo.** Es metadato, mismo lenguaje que la insignia `.stag` de la app. Si el framework le pone `cursor:pointer` por ser un `<span>` dentro de algo interactivo, córtalo.

## Marca de agua `.gm`

El logotipo `c.` sangrando por la esquina inferior derecha de tarjetas y paneles, en `currentColor` a baja opacidad, `pointer-events:none`, dentro de un contenedor con `overflow:hidden`. Tamaños y opacidades por contexto:

| Contexto | Tamaño | Posición | Opacidad |
|---|---|---|---|
| Tarjeta de resultado (hero) | 180px | `right:-48px; bottom:-54px` | .10 |
| Bloque de la demo | 300px | `right:-70px; bottom:-90px` | .07 |
| Paneles `.pane` | 190px | `right:-56px; bottom:-64px` | .08 |
| Tarjetas de cifra `.tile` | 118px | `right:-56px; bottom:-60px` | .22 |
| Banda vermut | 420px | `right:-60px; bottom:-120px` | .10 |
| Footer | 360px | `right:-40px; bottom:-140px` | .05 |

El SVG lleva `<circle fill="var(--primary)">` para el punto; en las marcas de agua se neutraliza con `--primary:currentColor` en línea, para que el punto no destaque en coral.

## Iconos

Sprite SVG inline al principio del `<body>`, `stroke:currentColor`, `fill:none`, `stroke-width:2.1`, `linecap/linejoin:round`, sobre `viewBox 0 0 24 24`. Tamaños: `.ic` 20px · `.ic.s` 15px (stroke 2.3) · `.ic.b` 28px (stroke 2) · `.ic.fill` relleno sin trazo.

Los 12 usados: `mark · film · tv · book · sparkle · search · sliders · plus · chev · upload · bookmark · starfill · star · clock`. Están en `icons.svg` (sprite completo de 28 del sistema) y también inline en el HTML. **Ningún icono nuevo**: si falta algo, se resuelve con texto, como la visibilidad de listas en la app.

---

# 4 · Nav

```
sticky top 0 · z-index 40 · alto 78
fondo rgba(36,17,32,.86) + backdrop-filter blur(14px)
border-bottom 1px transparent → --line2 cuando scrollY > 12 (transición .2s)
```

Izquierda: marca (`c.` a 30px + `cultay.` en Bricolage 700 25px, ls −.045em, el punto en `--primary`), gap 11.
Centro: enlaces a 15px/450 `--muted`, gap 32, margin-left 26. Copy: *La pregunta · La app · Por qué en español · Dudas*. **Se ocultan por debajo de 1100 px** (no hay menú hamburguesa: cuatro anclas de la misma página no lo justifican; el scroll hace el trabajo).
Derecha: insignia `.pill` *Beta privada* (mono 10.5px, ls .11em, uppercase, borde 1px `--primary`, radio 14, padding 6 12, color `--primary`) + `.btn.p.sm` *Quiero acceso* → `#beta`. Gap 20.

Comportamiento: una sola clase que se activa con el scroll.

```js
addEventListener('scroll',()=>nav.classList.toggle('stuck',scrollY>12),{passive:true});
```

`html{scroll-behavior:smooth}` para las anclas. Si el proyecto respeta `prefers-reduced-motion`, condicionarlo.

---

# 5 · Sección 01 · Hero

```
padding 104px 0 116px · overflow hidden
grid: minmax(0,1.06fr) minmax(0,.94fr) · gap 72 · align-items center
≤1100px → una columna, gap 56
```

Halo decorativo `.glow`: 900×900, `right:-320px; top:-380px`, `border-radius:50%`, `radial-gradient(circle, rgba(244,114,101,.16), rgba(244,114,101,0) 62%)`, `pointer-events:none`. Es el único elemento de la página que no es contenido; si estorba, se puede quitar sin tocar nada más.

## Columna izquierda

1. `.cap` — `LIBROS · PELÍCULAS · SERIES`
2. `h1.xl`, margin-top 26 — **«Todo lo que lees, ves y llevas semanas sin decidir.»**
3. `.lead`, margin-top 28, max-width 480 — «cultay reúne tus libros, películas y series en un solo sitio — y responde a la única pregunta que importa a las diez de la noche.»
4. `.acts`, margin-top 38 — **un solo botón**: `.btn.p` *Quiero acceso* → `#beta`.
5. `.fine`, margin-top 20, 13.5px `--faint` — «Beta privada · gratis el primer año · sin spam.»

**No añadir un segundo botón.** El trabajo de la landing es capturar altas y un secundario al lado del primario reparte la atención justo donde no debe repartirse.

## Columna derecha · tarjeta de resultado `.result`

Es el *money shot*: enseña el producto en el primer pantallazo. Réplica de la pantalla de resultado de `/recomendar`.

```
radio --shape · fondo --surface2 · borde 1px --line2 · padding 26 · overflow hidden
rejilla interna .rgrid: 172px 1fr · gap 26 · align-items start
```

- **Portada**: `aspect-ratio:2/3`, radio 3, `object-fit:cover`, fondo `--paper` mientras carga.
- `.cap` — `RECOMENDAR · PELÍCULA`
- `h2.md`, margin-top 12 — «La llegada.»
- `.rmeta`, margin-top 14, 13.5px `--muted`, gap 9: punto de tipo de 6 px + «Película · 2016 · 116 min · Ciencia ficción».
- **Bloque `.why`**: margin-top 22, radio `--shape`, fondo `--paper`, padding 20 22 22. Dentro: `.cap` con icono `sparkle` + `POR QUÉ ESTA`, y el texto en **Bricolage 500 a 17.5px**, lh 1.32, ls −.02em, margin-top 11, con los títulos citados en peso 700.
- `.ract`, margin-top 20, gap 10: `.btn.p.sm` *Añadir a mi biblioteca* (icono `plus`) + `.btn.g.sm` *Otra* (icono `sparkle`). **Son decorativos**: en la landing no llevan a ninguna parte. Si el framework insiste en semántica, usar `<span role="presentation">` o `<button type="button" tabindex="-1" aria-hidden="true">` — lo que no se debe es que entren en el orden de tabulación ni que parezcan romperse al pulsarlos.

Punto de tipo de obra `.pdot` (6px, círculo), heredado del sistema: película `--primary` · serie `--pist` · libro `--mant`.

---

# 6 · Sección 02 · El motor (demo interactiva)

La pieza de mayor valor de la página: el producto se puede **usar** sin registro. Tres preguntas, un botón, una recomendación con el motivo escrito.

## Cabecera

`.cap` `EL MOTOR` · `h2.lg` **«Tres toques y una recomendación. No una lista de cincuenta.»** · `.lead` explicando que no hace falta cuenta.

## Bloque `.demo`

```
radio --shape · fondo --surface2 · padding 44 48 48 · margin-top 52 · overflow hidden
≤900px → padding 30 24 34
```

Tres filas `.qrow`: `grid 132px 1fr · gap 22 · padding 20 0 · border-top 1px --line2`. La primera sin borde y con `padding-top 6`. Por debajo de 900 px pasan a una columna con gap 12. Izquierda el `.cap` del grupo, derecha los chips (`flex`, gap 9, wrap).

| Grupo | `data-group` | Opciones (`data-v` / `data-s` / texto) |
|---|---|---|
| Intensidad | `mood` | `ligero`/Ligero/«Algo ligero» · `denso`/Denso/«Algo denso» · `igual`/Sin filtro/«Me da igual» |
| Tiempo | `time` | `t30`/30 min/«30 min» · `t120`/1–2 h/«1–2 horas» · `tnight`/Toda la noche/«Toda la noche» |
| Formato | `kind` | `pelicula`/Película · `serie`/Serie · `libro`/Libro · `sorpresa`/Sorpresa/«Sorpréndeme» |

`data-v` es el valor que consume el motor; `data-s` la etiqueta corta que se pinta en la cabecera del resultado. Selección **exclusiva dentro de cada grupo**, marcada con `aria-pressed`. Por defecto: `ligero`, `t120`, `pelicula`.

Acción: `.btn.p` *Recomiéndame algo* (icono `sparkle`) + nota 13px `--faint` «Diez segundos. Sin registro.»

## Resultado `.dres`

Oculto (`hidden`) hasta el primer clic. `grid 132px 1fr · gap 24 · border-top 1px --line2 · padding-top 32`. Entra con `@keyframes rise` — `opacity 0→1` y `translateY(10px)→0` en **.4s `cubic-bezier(.2,.7,.3,1)`**.

Mismo contenido que la tarjeta del hero: portada, `.cap` con formato + intensidad + tiempo, título, meta, bloque `.why`, y dos acciones (`.btn.o.sm` *Guardar* con icono `bookmark`, `.btn.g.sm` *Otra recomendación*).

## Reglas del motor de la demo — **esto es lo importante**

El primer intento tenía un defecto que hay que no repetir: la respuesta ignoraba dos de las tres preguntas, así que la cabecera decía «Serie · Algo ligero» y el motivo empezaba «Densa, pero por capítulos…». **La pieza cuyo argumento es «el motor te dice el motivo» no puede contradecir lo que el usuario ha pedido.**

Reglas, en orden:

1. **El catálogo de ejemplo está indexado por `formato × intensidad`** — 12 claves, 2 fichas cada una (24 en total), en el objeto `RES` del HTML. Cópialo tal cual: la redacción está trabajada y cada motivo cita la biblioteca ficticia del usuario («puntuaste Interstellar con un 4,5», «la dejaste a medias»), que es lo que hace creíble que el motor sepa algo.
2. **El tiempo se resuelve por composición, no por índice.** Cada ficha lleva su `dur` real (minutos de película, minutos por capítulo, `0` para libros) y su `f` (formato real, que en «Sorpréndeme» no coincide con la clave). La primera frase del motivo se **genera** con la función `clause(entry, timeKey)`, que produce una afirmación verdadera para ese presupuesto:
   - encaja → «Pediste una o dos horas y son 116 min: entra justa.»
   - no encaja → «Media hora no da para 179 min, así que déjala para el finde — o pídeme algo más corto.»
   - libro → cláusula por presupuesto, sin minutos («Media hora da para el primer capítulo, y se deja en cualquier página.»)
   - serie → según duración de capítulo («Con una o dos horas caben dos capítulos de 50 min.»)
3. **Contador por clave, no global.** `seen[kind+'|'+mood]` para que *Otra recomendación* rote dentro de la combinación pedida. Un contador global hace que el resultado dependa del número de clics y no de la respuesta.
4. **Cambiar cualquier chip con el resultado ya visible lo recalcula en el sitio** y resetea el contador de esa clave. Así cabecera y texto no pueden desincronizarse nunca.

**Al enchufar el motor real, la regla se mantiene: el motivo se escribe con los filtros que el usuario ha marcado, o no se escribe.** Un motivo genérico del tipo «Porque valoras el ritmo» destruye el argumento de la sección.

Los `data-v` de los chips (`ligero/denso/igual`, `t30/t120/tnight`, `pelicula/serie/libro/sorpresa`) **hay que alinearlos con el vocabulario que espera el motor real** — sigue pendiente de confirmar por producto, igual que en el handoff de Biblioteca. Si el motor usa otros valores, se cambian los `data-v` y el copy visible se queda como está.

---

# 7 · Sección 03 · Los tres formatos (banda clara)

Envoltorio con clase `.hueso`. **Esta sección no menciona a la competencia, ni por nombre ni por paráfrasis, y no da cifras de terceros.** Ver `README.md` §3.2. Habla de lo que hace cultay, en positivo.

Cabecera: `.cap` `LOS TRES FORMATOS` · `h2.lg` **«Lo que lees y lo que ves, en la misma frase.»** · `.lead`.

## Tira `.rivals`

```
grid 3 columnas · margin-top 56 · border-top 1px --line
.rival: padding 30 32 34 · border-right 1px --line · display:flex; flex-direction:column
  primera columna: padding-left 0 · última: sin border-right
≤900px → una columna, sin border-right, con border-bottom, padding lateral 0
```

Cada columna: icono `.ic.b` en `--muted` → `h3.sm` → `<p>` 15px/1.5 `--muted` (margin 12 arriba, **22 abajo**) → línea de cierre `.lack`.

`.lack`: **`margin-top:auto`** + `padding-top 14` + `border-top 1px --line2`, 15px/600 en `--ink`, con icono `chev` de 15px en `--primary`.

> **Regla de copy, no negociable:** las tres líneas de cierre son de **una sola línea** — «Cruzan con lo que ves.» / «Cruzan con lo que lees.» / «Cruzan con todo lo demás.» El paralelismo es el argumento *y además* el hairline se ancla a ese texto: en cuanto una se va a dos líneas, las tres reglas quedan escaladas a distinta altura. Si el copy tiene que crecer, va en el `<p>`, nunca en `.lack`.

## Remate `.punch`

```
margin-top 64 · padding-top 44 · border-top 1px --line
grid: 1fr 300px · gap 56 · align-items end
≤1100px → una columna, gap 28, align-items start
```

Izquierda `h3.md` «Lees, ves series y vas al cine como parte de lo mismo — porque lo son.» · derecha `<p>` 16px/1.55 `--muted` que hace de puente con el motor.

---

# 8 · Sección 04 · La app

Cabecera: `.cap` `LA APP` · `h2.lg` **«Tu biblioteca entera en una pantalla.»** · `.lead`.

## Marco `.shot`

```
margin-top 54 · radio 12 · borde 1px --line · overflow hidden · fondo #241120
```

**Barra `.shotbar`**: alto 44, padding 0 16, fondo `--surface2`, border-bottom 1px `--line2`. Tres puntos de 11px en `--line`; barra de URL centrada (`margin:0 auto`) en mono 11.5px `--faint`, padding 5 16, radio 5, fondo `rgba(0,0,0,.14)`, texto `cultay.app/biblioteca`; y a la derecha el conmutador `.tswitch` — borde 1px `--line`, radio 14, dos botones en mono 10px, ls .09em, uppercase, padding 6 11; el activo con fondo `--ink` y texto `--surface`.

**El conmutador cambia la maqueta, no la página.** Pone/quita `data-theme="dark"` en el elemento `.app`. Es el único elemento con tema de toda la landing, y es a propósito: así el modo oscuro se presenta como función del producto en vez de como preferencia impuesta al visitante.

## La maqueta `.app` — **provisional**

Reproducción de `/biblioteca` en HTML del sistema, a 1440 px de ancho de diseño, escalada para encajar. Lleva sus propios tokens (claros por defecto, `[data-theme=dark]` los invierte) para no heredar los de la landing.

Contiene: barra de la app (76px, marca + 4 enlaces con el activo en `--primary` y subrayado de 2px), `.cap` `BIBLIOTECA`, `h1` de 76px, campo de búsqueda de 52px, barra de filtros con chips y divisor de 1×22, y rejilla de **5 columnas** (gap 32/20) con 10 portadas a 2:3, radio 3, insignia de estado y meta.

Insignias de estado — mismos tres colores en claro y en oscuro, porque van sobre la portada:

| Estado | Fondo | Texto |
|---|---|---|
| Quiero | `#E8BE5A` | `#3A2A08` |
| En curso | `#B22838` | `#F7EFDC` |
| Visto | `#BFCB5C` | `#26290C` |

**El encaje es por JS, no por escala fija** (una escala fija recortaba el 25 % de la UI en cualquier viewport menor de 1200):

```js
function fitShot(){
  const vp=document.querySelector('.viewport'), app=document.getElementById('appMock');
  const dw=parseFloat(getComputedStyle(app).width)||1440;   // 1440 · 920 en móvil
  const sc=vp.clientWidth/dw;
  app.style.transform='scale('+sc+')';                       // transform-origin: top left
  const ch=parseFloat(getComputedStyle(vp).getPropertyValue('--croph'))||770;
  vp.style.height=Math.round(ch*sc)+'px';                    // el recorte alto, escalado
}
```

Llamarlo al cargar, en `resize`, en un `ResizeObserver` sobre `.shot` y tras `document.fonts.ready`. `--croph` es la altura de recorte en unidades de diseño (770 en escritorio, 660 en móvil) y `.fade` es el degradado de 130px que justifica el corte inferior.

Por debajo de 900 px la maqueta **pasa a sangre** (márgenes laterales negativos de 28px, sin radio ni bordes laterales), el ancho de diseño baja a **920 px** y la rejilla a 4 columnas ocultando de la novena tarjeta en adelante — para que la UI siga siendo legible y no una miniatura del 23 %.

> **Cuando haya producto:** sustituir todo el interior de `.viewport` por `<img>`/`<picture>` con capturas reales en claro y en oscuro (el conmutador cambia el `src`), borrar `fitShot()`, `--croph`, `.app` y sus ~40 reglas. `.shot`, `.shotbar` y `.fade` se quedan. Dos capturas a 2× de 1440 px de ancho.

## Los dos paneles `.duo`

```
grid 2 columnas · gap 20 · margin-top 52 · (≤900px → una columna)
.pane: radio --shape · fondo --surface2 · borde 1px --line2 · padding 30 32 34 · overflow hidden
```

**Son bloques explicativos, no accesos directos.** Ese fue un problema real del primer diseño: parecían pulsables sin serlo. Las etiquetas `.tag` son metadato con `cursor:default`; el único elemento interactivo es el enlace del segundo panel.

1. **Valoración** — `.cap` con icono `starfill` · `h3.sm` «Medias estrellas, ritmo y tono. Nada de formularios.» · `<p>` · fila `.stars`: cuatro `starfill` + un `star` a 22px en `--primary`, gap 6, más «4,5» en mono 12px `--muted` con margin-left 8 · `.tags`: `Ritmo · pausado`, `Tono · melancólico`, `Nota privada`.
2. **Empezar** — `.cap` con icono `upload` · `h3.sm` «Tu historial ya existe. Tráelo.» · `<p>` · `.tags`: `Importar CSV`, `Mapeo automático`, `Sin duplicados` · y el enlace `.panelink` **«Apuntarme a la beta»** con icono `chev` → `#beta` (margin-top 22, 15px/600, `--primary`, hover a `--ink`).

---

# 9 · Sección 05 · Tu perfil (banda clara)

Envoltorio `.hueso`. Cabecera: `.cap` `TU PERFIL` · `h2.lg` **«Y a final de año, la cuenta de lo que has vivido.»** · `.lead`.

Rejilla `.tiles`: 4 columnas, gap 16, margin-top 52 (2 columnas por debajo de 900px).

**Tarjeta de cifra `.tile`** — la pieza central del sistema visual, idéntica a la de Perfil:

```
radio --shape · padding 22 22 26 · min-height 230 · flex column · overflow hidden
fondo: radial-gradient(125% 105% at 100% 100%, rgba(0,0,0,.17), rgba(0,0,0,0) 60%) sobre var(--tile)
```

Dentro: icono `.ic.b` a `opacity .55` arriba · cifra `.num` (Bricolage 700, **58px**, lh .9, ls −.055em, `margin-top:auto`; el sufijo en `<span>` a 27px) · etiqueta `.lab` (Bricolage 500, 15px, ls −.022em, `opacity .78`, margin-top 11) · marca de agua a 118px/.22.

| Clase | Fondo | Texto | Contenido de ejemplo |
|---|---|---|---|
| `.pelis` | `#B22838` | `#F7EFDC` | 47 · películas vistas este año |
| `.series` | `#BFCB5C` | `#26290C` | 9 · series terminadas |
| `.libros` | `#E8BE5A` | `#3A2A08` | 4.180 · páginas leídas |
| `.lectura` | `#381B2A` | `#F2E7CE` | 63 h · delante de una pantalla, a gusto |

Las cifras son de ejemplo. **El color plano de fondo está reservado a estas tarjetas** (y a las cuatro opciones de `/recomendar` en la app): no dar fondo de color a nada más en la landing.

---

# 10 · Secciones 06–08 y footer

## 06 · La diferencia (`#espanol`)

`.cap` `LA DIFERENCIA` · `h2.lg` **«Pensada en español. No traducida.»** · `.lead`.

Rejilla `.four`: 2 columnas, margin-top 56, border-top 1px `--line`. Cada `.pt`: padding `32 40 36 0`, border-bottom 1px `--line`; las pares con `padding-left 40` y `border-left 1px --line`; las dos últimas sin border-bottom. Dentro: número `.k` (mono 11px, ls .13em, `--primary`) · `h3.sm` margin-top 16 · `<p>` 15.5px/1.52 `--muted`, max-width 420. Una columna por debajo de 900 px.

Los cuatro: *Interfaz y comunidad en español* · *Catálogo hispanohablante de verdad* · *Recomendaciones que cruzan formatos* · *Sin vender tus datos*. Numerados, **sin marcas de verificación**: cuatro argumentos de posicionamiento no son una checklist de features.

## 07 · Dudas (`#faq`, `.sec.tight`)

`.faq` con `margin-top 48` y `border-top 1px --line`; cada `<details>` con `border-bottom 1px --line`.

`summary`: `list-style:none` (+ `::-webkit-details-marker{display:none}`), cursor pointer, padding 26 0, gap 20, Bricolage 700 21px ls −.03em. El icono `chev` va con `margin-left:auto` en `--muted` y **rota 90° en `.18s`** pasando a `--primary` cuando el `details` está abierto. Respuesta: `<p>` 16px/1.58 `--muted`, max-width 680, padding-bottom 28.

Seis preguntas, la primera abierta por defecto: precio · importar de otras apps · origen de datos y portadas · app móvil · qué se hace con los datos · cuándo abre. Usar `<details>` de verdad: funciona sin JS y Google lo indexa.

## 08 · Beta (`#beta`)

Banda a sangre en `--vermut` `#B22838`, texto `#F7EFDC`, `.wrap` con padding 110 arriba y 114 abajo, marca de agua de 420px.

`.cap` en `rgba(247,239,220,.55)` · `h2.lg` **«Únete a la beta privada.»** con el punto en `#E8BE5A`, max-width 640 · `.lead` en `rgba(247,239,220,.78)`, max-width 520.

**Formulario** `.form`: flex, gap 12, margin-top 36, max-width 560. El campo es un `<label>` con `flex:1`, alto 52, padding 0 18, radio 6, fondo `rgba(247,239,220,.1)`, borde 1px `rgba(247,239,220,.3)` que pasa a `#F7EFDC` en `:focus-within`; dentro un `<input type="email" required>` transparente a 15.5px. Botón `.btn.inv` *Apuntarme*. En columna por debajo de 620px.

Debajo, `.ctafine` a 13.5px `rgba(247,239,220,.6)`, max-width 520, con el enlace a privacidad en `#F7EFDC` subrayado con `text-underline-offset:3px`.

> **Contador de lista de espera: quitado.** El bloque `.counter`/`.avs` está en el HTML con `hidden` y el número a `—`, por si más adelante hay una cifra que valga la pena. Un contador flojo resta, y sin número es peor que no tenerlo. **No lo actives sin un número real que sume.**

**Esto es lo único que la landing tiene que hacer funcionar de verdad.** El prototipo hace `e.preventDefault()`. En producción: `POST` al proveedor de listas, estado de carga en el botón, mensaje de éxito («✓ Casi listo. Revisa tu email y confirma la suscripción para cerrar tu hueco.» — el copy ya existe en la web actual), validación de email en cliente y mensaje de error legible. Doble opt-in, como ahora.

## Footer

Fondo `#1B0B17`, padding 72 0 56, marca de agua de 360px a `opacity .05`.

`.fgrid`: `minmax(0,1fr) repeat(3,140px)`, gap 44 (una columna por debajo de 900px). Primera celda: marca + descripción de una frase, max-width 320. Luego tres columnas con `h4` en mono 11px ls .13em uppercase `--faint` (margin-bottom 18) y `<ul>` sin viñetas, flex column, gap 12, enlaces a 15px `--muted`:

- **Producto** — La pregunta · La app · Beta privada · Dudas
- **Redes** — Instagram · TikTok · X
- **cultay** — Contacto (`mailto:hello@cultay.com`) · Privacidad

`.fbot`: margin-top 64, padding-top 26, border-top 1px `--line2`, mono 11.5px ls .05em `--faint`, `space-between` — «© 2026 cultay» / «Hecho en español».

> **Pendiente:** solo el enlace de Instagram (`instagram.com/cultay.app`) está verificado. TikTok (`tiktok.com/@cultay.app`) y X (`x.com/cultayapp`) son suposición: confirmar antes de publicar. **Si una cuenta no existe o está vacía, no enlazarla** — un perfil sin publicaciones en una landing de producto sin lanzar resta.

---

# 11 · Estado e interacciones

Todo el estado es local y efímero. No hay router, no hay persistencia, no hay `localStorage`.

| Estado | Dónde | Notas |
|---|---|---|
| `navStuck` | clase en el nav | `scrollY > 12` |
| `demo.mood / time / kind` | `aria-pressed` en los chips | exclusivo por grupo; por defecto `ligero`, `t120`, `pelicula` |
| `demo.result` | oculto hasta el primer clic | recalcula al cambiar cualquier chip |
| `demo.seen[kind\|mood]` | contador por clave | rotación de *Otra recomendación* |
| `mock.theme` | `data-theme` en `.app` | claro por defecto; **no** afecta a la página |
| `shot.scale` | `transform` en `.app` | derivado del ancho, no es estado de usuario |
| `faq.open` | nativo de `<details>` | primera abierta |
| `beta.email / status` | formulario | lo único que sale a red |

Transiciones, todas las que hay:

| Qué | Valor |
|---|---|
| Botones y chips (`background`, `border-color`, `color`) | .14s (.12s en enlaces) |
| Borde del nav | .2s |
| Entrada del resultado de la demo | .4s `cubic-bezier(.2,.7,.3,1)` |
| Rotación del `chev` de la FAQ | .18s |

Nada más se mueve. No hay animaciones de entrada al hacer scroll y no deben añadirse.

## Accesibilidad

- Foco visible en todo: `:focus-visible{outline:2px solid var(--primary); outline-offset:3px}`.
- Chips y conmutador de tema: `<button>` reales con `aria-pressed`.
- Los grupos de chips deberían llevar `role="group"` con `aria-label` («Intensidad», «Tiempo disponible», «Formato») al implementarlos.
- El input de email lleva `aria-label` porque la etiqueta es visualmente un contenedor.
- Los botones decorativos de la tarjeta del hero **fuera del orden de tabulación**.
- Contraste: `--muted` sobre `--surface` cumple AA para texto de 15px+; **no bajar `--faint` de 11px de tamaño** — se usa solo para `.cap` y metadatos en mayúsculas.
- La maqueta de la app es decorativa: `aria-hidden="true"` en `.viewport` cuando pase a ser imagen, con el `alt` descriptivo en el `<img>`.

## Responsive

Tres puntos de corte: **1100** (hero a una columna, `.punch` a una columna, se ocultan los enlaces del nav) · **900** (secciones a 96px, `.wrap` a 28px, `.rivals` `.duo` `.four` `.fgrid` a una columna, `.tiles` a dos, maqueta a sangre con ancho de diseño 920) · **620** (tarjetas de resultado a una columna con portada de 180px máximo, formulario en columna, botones a ancho completo).

Se mantienen a cualquier ancho: el radio `26/26/26/5`, la marca de agua sangrando, la proporción 2:3 de las portadas y el punto vermut de los titulares.

---

# 12 · Assets, rendimiento, SEO

## Tipografías

**Bricolage Grotesque** (400–800, eje `opsz` 12–96) para display · **Geist** (300–700) para interfaz · **Geist Mono** (400, 500) para `.cap`, cifras y metadatos.

El prototipo las trae de Google Fonts por comodidad. **En producción: autoalojar** en `woff2`, subsetear a latin + latin-ext (hace falta `ñ`, acentos, `¿`, `¡`, `’`, `—`, `·`, `′`), `font-display:swap` y `<link rel="preload">` para el peso de display, que es lo primero que se ve. Sustitutos prohibidos: **Inter, Roboto, Arial**.

## Imágenes

Solo portadas. Necesarias **≈12–16**, y **están reutilizadas a propósito** entre las tres zonas (hero, demo, maqueta), así que un único juego cubre la página entera. Títulos de ejemplo usados: *La llegada · Interstellar · Aniquilación · Dune. Parte dos · Aftersun · Perfect Days · Drive My Car · Severance · Shōgun · Pachinko · Fleabag · Ted Lasso · El problema de los tres cuerpos · Los detectives salvajes · Los asquerosos · Éramos unos niños*.

Formato: 2:3, servir en AVIF/WebP con `srcset`, `loading="lazy"` salvo la del hero (que va `eager` con `fetchpriority="high"`), `width`/`height` explícitos para no provocar CLS. Confirmar derechos de uso a este tamaño antes de publicar — es una pregunta que sigue abierta desde el handoff de Biblioteca.

El logotipo `c.` y los iconos son SVG inline: no son peticiones. Mantenerlos inline, no pasarlos a `<img>`.

## Peso y métricas

Sin la maqueta HTML (cuando se sustituya por imagen) la página son ~15 KB de HTML, ~19 KB de CSS y ~5 KB de JS sin comprimir. **Objetivo: LCP < 1,5 s.** El LCP es el `h1` del hero o la portada de la tarjeta de resultado — precargar esa imagen. `backdrop-filter` en el nav es el único efecto costoso; si diera problemas en móvil, cambiarlo por un fondo opaco `#241120`.

## Meta

Ya en el `<head>` del prototipo: `title`, `description`, `viewport`, `lang="es"`. **Añadir en producción**: `og:title`, `og:description`, `og:image` (usar la imagen OG que ya está generada en `exports/logos/`), `og:type=website`, `og:url`, `twitter:card=summary_large_image`, `canonical`, y el favicon + apple-touch-icon del mismo paquete de `exports/logos/`.

`html lang="es"`. Un solo `<h1>` en la página (el del hero); las cabeceras de sección son `h2`.

---

# 13 · Archivos del paquete

| Archivo | Qué es |
|---|---|
| `Cultay Landing.html` | La página. Referencia de diseño + copy definitivo + la lógica de la demo. |
| `landing.css` | Todos los estilos. Reutilizable casi literalmente. |
| `icons.svg` | Sprite de los 28 iconos del sistema + logotipo. En el HTML va inline el subconjunto de 12 que se usa. |
| `tokens.css` | Tokens y componentes de la **app** (planteamiento claro-primero). Para cotejar que landing y producto no divergen. No lo carga la landing. |
| `image-slot.js` | Solo prototipado. **No va a producción.** |
| `README.md` | La propuesta: por qué esta estructura, qué cambia respecto a la web actual, y las decisiones cerradas (modo oscuro, competencia, contador). Léelo si algo de este documento parece arbitrario. |
| `IMPLEMENTACION.md` | Este documento. |

---

# 14 · Checklist de publicación

- [ ] Portadas reales en las tres zonas (hero, demo, maqueta). **Bloquea la publicación.**
- [ ] Formulario de beta conectado, con doble opt-in, estado de carga, éxito y error.
- [ ] Fuentes autoalojadas y subseteadas.
- [ ] Enlaces de TikTok y X confirmados, o retirados.
- [ ] `og:image`, favicon y apple-touch-icon desde `exports/logos/`.
- [ ] `image-slot.js` fuera del bundle.
- [ ] Revisado a 1440 / 1280 / 1024 / 768 / 390 px de ancho.
- [ ] Contraste comprobado en las dos bandas claras y en la banda vermut.
- [ ] Navegación completa con teclado; el resultado de la demo se anuncia al cambiar (`aria-live="polite"` en `#demoResult`).
- [ ] Confirmadas las dos afirmaciones que hoy no puedo verificar: «plan gratuito para siempre» y el origen de los datos y portadas de la FAQ.
