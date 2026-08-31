# Propuesta: landing page de cultay

Actualización de `cultay.com` al sistema **Marker · Vermut v3** — el mismo de Perfil, Biblioteca, Recomendar y Mis listas. No es solo un repinte: la estructura cambia.

Archivo: `Cultay Landing.html` (+ `landing.css`, `icons.svg`, `image-slot.js`). Es una **página real, con scroll y responsive**, no artboards: una landing se juzga en movimiento y en el móvil, no en un lienzo.

---

# 1 · ¿Modo claro y oscuro en la landing? No

**Recomendación: una sola identidad, y que sea la oscura.** Sin conmutador en la cabecera.

Razones, en orden de peso:

1. **La landing tiene un solo trabajo: que te apuntes.** Cada control de la cabecera compite con el único botón que importa. Un conmutador de tema es una decisión que le regalas al visitante y que no mueve la conversión ni un punto.
2. **En la app el tema es una función; en la landing es la marca.** Dentro del producto el usuario vive horas y el tema es preferencia legítima (y ya está resuelto con `data-theme`). Una landing se ve una vez, treinta segundos: ahí el color *es* el posicionamiento. Berenjena `#241120` con vermut coral dice «cine de noche, sofá, decisión tomada». El hueso dice «papel, archivo, biblioteca». Para el argumento de venta, la primera lectura es la correcta.
3. **Duplica el QA de cada sección para siempre.** Cada banda, cada sombra sobre portada, cada degradado del mockup hay que revisarlo dos veces, en cada cambio de copy.
4. **La landing actual ya es clara y no está funcionando como escaparate del producto.** Cambiar a oscuro es, además, la diferencia visible entre «hemos hecho una web» y «hemos hecho un producto».

## Lo que sí hacemos con el modo claro — dos cosas

**a) Bandas claras como recurso editorial.** Dos secciones van en hueso `#F2E7CE` (`.band.hueso`): *El problema* y *Tu perfil / cifras*. Se consigue el respiro y el contraste del papel sin dar un interruptor a nadie, y las dos están elegidas a propósito:
- *El problema* es el mundo de antes: tres apps sueltas, luz de día, tono de informe.
- *Las cifras* son el sitio donde las tarjetas de color del sistema (vermut, pistacho, mantequilla, berenjena) revientan mejor — sobre hueso, no sobre berenjena.

**b) El conmutador vive dentro del mockup del producto.** En la sección *Un solo lugar. Por fin.* la barra del navegador falso lleva un `Claro / Oscuro` que cambia **la captura**, no la página. Así el modo oscuro deja de ser una preferencia impuesta y pasa a ser **una función que se demuestra** — que es lo que es. Es el único elemento con tema en toda la landing.

Un solo matiz técnico: **no atendemos `prefers-color-scheme`** en la landing. Es deliberado. La marca no negocia con la configuración del sistema operativo.

---

# 2 · Qué cambia en la estructura

| # | Landing actual | Propuesta | Por qué |
|---|---|---|---|
| 1 | Hero con mockup de móvil genérico | Hero con **la tarjeta de resultado real** (portada + «Por qué esta») | Es el momento en que el producto gana. Enseñarlo en el primer pantallazo vale más que cualquier titular |
| 2 | Banda de 4 cifras: `75M · 3 · 0 · 500M` | **Eliminada** | `3` y `0` no son datos, son retórica con tipografía de dato. Y disparan antes de que se sepa qué es cultay. Las dos cifras honestas (75 M, 500 M) se reubican donde argumentan |
| 3 | Selector de ánimo **estático**, a media página | **Demo funcional en la posición 2** | El diferencial es el motor. Que se pueda *usar* sin registro, tres toques y una recomendación con motivo escrito, es la mayor palanca de conversión de toda la página || 4 | Tres competidores con emoji y párrafo largo cada uno | Tres columnas por **formato** — qué hace cultay con los libros, con las películas y con las series | Ver punto 3.2: cero alusiones a la competencia. La banda pasa de describir el mundo de antes a contar el producto |
| 5 | «Un solo lugar» = tres bloques de texto numerados | **Captura real de Biblioteca** a escala, con el conmutador de tema | La app ya está diseñada. No enseñarla es el error más caro de la landing actual |
| 6 | Valoración e IA como texto | Dos paneles: estrellas con medias + importación CSV | Importar Goodreads es una función que quita fricción de entrada y estaba escondida como una etiqueta gris |
| 7 | Estadísticas: no aparecen | Banda clara con **las tarjetas de cifra** del sistema | Es lo más bonito que tiene el sistema visual y es también el gancho de retención («tu año, contado») |
| 8 | Español: 4 bullets con ✓ | Rejilla 2×2 numerada, sin marcas de verificación | Cuatro argumentos de posicionamiento no son una checklist de features |
| 9 | — | **FAQ de 6 preguntas** | Precio, importación, datos, móvil, privacidad, cuándo abre. Son las objeciones que hoy se quedan sin respuesta justo antes del formulario |
| 10 | CTA + footer mínimo | CTA en banda vermut a sangre + footer de cuatro columnas con las tres redes | El cierre necesita peso visual propio |

## Orden final

```
nav (sticky, con Beta privada + CTA)
01  Hero — oscuro                 titular + tarjeta de resultado · UN botón
02  El motor — oscuro             DEMO INTERACTIVA
03  Los tres formatos — CLARO     libros · películas · series, y cómo se cruzan
04  La app — oscuro               captura de Biblioteca (claro/oscuro) + valoración + importar
05  Tu perfil — CLARO             tarjetas de cifra
06  La diferencia — oscuro        en español, de verdad · 2×2
07  Dudas — oscuro                FAQ
08  Beta — VERMUT a sangre        formulario
    footer (4 columnas, con redes)
```

La alternancia oscuro → claro → oscuro → claro → oscuro da ritmo sin usar más de dos fondos, como manda el sistema.

---

# 3 · Correcciones de contenido

## 3.1 · El contador de lista de espera: fuera

Quitado (`.counter` queda en el HTML con `hidden` y el número a `—`, por si más adelante hay una cifra que valga la pena). Un contador flojo resta más de lo que suma, y un contador sin número es peor que no tenerlo.

## 3.2 · Cero alusiones a la competencia

La landing ya no menciona a nadie, ni por nombre ni por paráfrasis, y no da ninguna cifra de terceros. Cuatro razones:

1. **Las cifras eran el punto más frágil de la página.** Datos de terceros que envejecen solos y que alguien tiene que mantener con su fecha. Un número desactualizado justo en la sección donde argumentas te resta credibilidad donde más la necesitas.
2. **Nombrarlos te empequeñece.** En una landing de producto sin lanzar, poner tu marca al lado de tres marcas consolidadas hace que el visitante reconozca las tres y no la cuarta.
3. **La paráfrasis tampoco vale.** El primer intento quitó los nombres pero mantenía la estructura — «una app para los libros, otra para el cine, una tercera…» — que es comparación con otro traje. Fuera también.
4. **La sección rendía poco.** Gastaba una banda entera en describir el mundo de antes. Ahora esa banda habla de lo que hace cultay.

**La sección 03 cambia de trabajo.** Era *El problema*; ahora es **Los tres formatos**: qué hace cultay con los libros, con las películas y con las series, una columna cada uno. La línea de cierre de cada columna, que antes era la carencia del rival, ahora es la ventaja — y mantiene el paralelismo de una línea:

> Cruzan con lo que ves. · Cruzan con lo que lees. · **Cruzan con todo lo demás.**

Y el remate de la banda hace de puente con el motor: «cuando las tres cosas están juntas, las recomendaciones dejan de ser de libros o de películas: son de lo que te apetece». El problema queda dicho en **una sola cláusula subordinada** del `lead`, sin señalar a nadie.

Consecuencia: la sección 04 se retitula. Era *Un solo lugar. Por fin.*, que ahora se solapaba con la 03; pasa a **Tu biblioteca entera en una pantalla** y se ocupa solo de la captura del producto.

## 3.3 · Resto

1. **Contradicción a resolver.** La landing actual promete «de 3 a 5 recomendaciones»; el diseño de la app dice «una sola recomendación, no una lista infinita». **Manda la app**: el argumento entero es que no te dan una lista. Aquí ya está unificado en «una recomendación» + alternativas debajo si esa no.
2. **Fuera los emoji** (📚 🎬 📺). Iconos del set de 28: `book`, `film`, `tv`.3. **«Beta privada»** pasa a distintivo en el nav, no a texto suelto en el hero.
4. **Los motivos del motor citan la biblioteca del usuario** («puntuaste Interstellar con un 4,5», «la dejaste a medias»). Es lo que hace creíble que el motor sepa algo. La línea genérica del tipo «Porque valoras el ritmo» no lo consigue.
5. **Micro-copy bajo el CTA del hero**: `Beta privada · gratis el primer año · sin spam.` — las tres objeciones resueltas en una línea, antes de bajar.

---

# 4 · Dudas resueltas

## Un solo botón en el hero

El secundario («Probar el motor» / «Probarlo sin registro») está fuera. El trabajo de la landing es capturar altas, y un segundo botón al lado del primario reparte la atención justo en el punto donde no debe repartirse. La demo sigue ahí abajo y sigue siendo el mejor argumento de la página — se llega por scroll o por el enlace del nav, que es donde una prueba secundaria tiene su sitio.

## ¿A dónde lleva «Empezar»?

Al formulario de la beta (`#beta`), el mismo destino que todos los CTA de la página. El enlace estaba mal etiquetado («Es lo primero que harás al entrar», que no dice a dónde va); ahora dice **«Apuntarme a la beta»**. La página entera tiene **un solo destino**: nav, hero, cuadro de *Empezar* y cierre — cuatro entradas al mismo formulario, ningún otro sitio al que ir.

## Los cuadros de «Valoración» y «Empezar», ¿son accesos directos?

No, y tenías razón en dudarlo: parecían pulsables sin serlo, que es el peor de los dos mundos. Son **bloques explicativos** — las dos funciones que no caben en la captura de Biblioteca. Resuelto así:

- Las etiquetas mono (`Ritmo · pausado`, `Importar CSV`…) son **metadato descriptivo**, no chips pulsables: mismo lenguaje que la insignia `.stag` de la app, y ahora con `cursor:default` para que no inviten al clic.
- El cuadro *Empezar* sí termina en una acción de verdad: **«Apuntarme a la beta →»**. Así el par deja de ser ambiguo — uno informa, el otro informa y remata — en vez de que los dos parezcan botones a medias.

## Portadas y capturas reales

De acuerdo, y es la dependencia más importante que queda. Dos cosas distintas:

1. **Las portadas.** Hay tres sitios con portada: la tarjeta del hero, el resultado de la demo y las 10 de la captura de Biblioteca. Están como `image-slot`: puedes arrastrar imagen encima y se queda. He hecho coincidir a propósito los títulos de las tres zonas, así que **con un solo juego de ≈12 portadas** queda la página entera cubierta. Los títulos usados: *La llegada, Interstellar, Aniquilación, Dune. Parte dos, Aftersun, Perfect Days, Drive My Car, Severance, Shōgun, Pachinko, Fleabag, Ted Lasso, El problema de los tres cuerpos, Los detectives salvajes, Los asquerosos, Éramos unos niños*. Si prefieres otro reparto — más catálogo en español, por ejemplo — dímelo y cambio los datos de ejemplo; la lista también es un mensaje sobre qué tipo de app es esta.
2. **La captura.** Hoy es HTML real del sistema a escala, no una imagen. Eso es una ventaja mientras el producto no esté implementado (se actualiza sola, pesa nada, se ve nítida en cualquier pantalla), pero **en cuanto `dev.cultay.app` tenga las pantallas hay que sustituirla por captura de verdad**, con biblioteca real y portadas reales. El contenedor `.shot` no cambia: se cambia el interior por un `<img>` y el `fitShot()` deja de hacer falta.

Mientras no haya portadas reales, la página **no se publica**. Una landing cuyo argumento es visual con diez rectángulos grises vacíos dice lo contrario de lo que pretende.

## Redes en el pie

Añadida columna **Redes** con Instagram, TikTok y X, separada de la columna legal (contacto y privacidad) para que no se mezclen dos cosas distintas. El pie pasa a cuatro columnas.

**Confirma los enlaces**: solo Instagram (`instagram.com/cultay.app`) está verificado, del pie actual. TikTok y X van con `tiktok.com/@cultay.app` y `x.com/cultayapp` como suposición — dime los reales. Y si alguna de las dos cuentas aún no existe o está vacía, mejor no enlazarla: un perfil sin publicaciones en una landing de producto sin lanzar resta igual que el contador de la lista.

# 5 · Detalles de implementación

- **Tokens**: los mismos de `tokens.css`. La landing invierte el planteamiento: el bloque oscuro es `:root` y la clase `.hueso` reintroduce los tokens claros por sección.
- **Tipografía**: Bricolage Grotesque 700 para display con `clamp()` (46 → 104 px en el hero), Geist para cuerpo, Geist Mono para `.cap` y cifras. Punto vermut al final de cada titular, como en toda la app.
- **Forma**: radio `26px 26px 26px 5px` en tarjetas y paneles. `c.` sangrando al 7–22 % de opacidad. Cero sombras.
- **El mockup** es HTML real del sistema a 1440 px con `transform:scale(.7778)` dentro de una ventana de 1120, con degradado de corte abajo. Cuando haya capturas reales de `dev.cultay.app`, se sustituye por imagen sin tocar el layout.
- **Portadas**: `image-slot` para arrastrar portadas reales sobre la maqueta. **No va a producción.**
- **Accesibilidad**: `aria-pressed` en chips y conmutador, foco visible en vermut/coral, FAQ con `<details>` (funciona sin JS y es indexable por Google).
- **JS**: vanilla, sin dependencias. La demo funciona en cliente con datos de ejemplo; en producción llama al motor real.
- **Coherencia de la demo**: el catálogo de ejemplo está indexado por **formato × intensidad** (24 fichas), y el **tiempo se resuelve por composición**: cada ficha lleva su duración real y la primera frase del motivo se genera a partir del presupuesto pedido. Si lo que encaja no cabe en el tiempo, el motivo lo admite («Media hora no da para 179 min, así que déjala para el finde») en lugar de mentir. Cambiar cualquier chip recalcula el resultado en el sitio, así que el `.cap` y el texto nunca se contradicen. Es la regla que hay que mantener cuando se enchufe el motor real: **el motivo se escribe con los filtros que el usuario ha marcado, o no se escribe.**
- **El mockup se escala al ancho de su contenedor** (`fitShot()` + `ResizeObserver`), nunca a un factor fijo: encaja entero a cualquier ancho, sin recorte lateral. Por debajo de 900 px pasa a sangre, el ancho de diseño baja a 920 px y la rejilla a 4 columnas para que la UI siga siendo legible en el móvil.

## Pendiente de vosotros

1. **Portadas reales** (≈12 imágenes) y, cuando existan, **capturas reales** de Biblioteca. Bloquea la publicación.
2. **Los enlaces de TikTok y X**, y si esas cuentas ya tienen contenido.
3. **La afirmación de las portadas** en la FAQ («catálogos abiertos y bases de datos con licencia») hay que ajustarla a la fuente real — es la pregunta 2 que sigue abierta del handoff de Biblioteca.
4. **Precio del plan gratuito**: la FAQ afirma «plan gratuito para siempre». Confirmadlo o lo reformulo.
5. **Los títulos de ejemplo**, si queréis otro reparto (ver punto 4).

## Qué NO hacer

- No añadir un conmutador de tema a la cabecera (el punto 1 entero).
- No volver a poner cifras de usuarios de terceros, nombres de competidores **ni paráfrasis del tipo «otra app hace solo X»** (punto 3.2). Las ventajas se cuentan en positivo.
- No añadir un segundo botón al hero.
- No devolver el contador de la lista de espera sin un número que sume.
- **Las tres líneas de cierre de «Los tres formatos» son de una sola línea** — «Cruzan con lo que ves.» / «Cruzan con lo que lees.» / «Cruzan con todo lo demás.». El paralelismo es el argumento, y además el hairline del pie se ancla a ese texto: en cuanto una se va a dos líneas, las tres reglas se escalonan.
- No devolver la banda de cuatro cifras con `0` y `3` como métricas.
- No usar más de dos colores de fondo (berenjena y hueso) más la banda vermut del cierre.
- No poner sombras ni tarjetas con borde de acento a la izquierda.
- No prometer «de 3 a 5 recomendaciones».
- No publicar con las portadas vacías.
