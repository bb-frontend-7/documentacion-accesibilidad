# Generalidades.

## Tabla de contenido.

1. [Imágenes y texto alternativo](#imágenes-y-texto-alternativo)
   - [Consideraciones](#consideraciones)
   - [Excepciones](#excepciones)
   - [Accesibilidad AAA](#accesibilidad-aaa)
1. [Contraste](#contraste)
1. [Uso del color](#uso-del-color)
1. [Navegación por teclado](#navegación-por-teclado)
   - [Estado de focus](#estado-de-focus)
   - [Indicador de focus](#indicador-de-focus)
   - [Atributo `tabindex`](#atributo-tabindex)
1. [Esconder visualmente un elemento (pero no de lectores de pantalla)](#esconder-visualmente-un-elemento-pero-no-de-lectores-de-pantalla)
1. [`aria-label` y `aria-labelledby`](#aria-label-y-aria-labelledby)

   - [`aria-label`](#aria-label)
   - [`aria-labelledby`](#aria-labelledby)
   - [Cuándo usar cada caso](#cuándo-usar-cada-caso)

1. [Creación de inputs](#creación-de-inputs)
1. [Zoom en el sitio](#zoom-en-el-sitio)
1. [Visualización del texto](#visualización-de-textos)
   - [Recomendaciones sobre interlineado](#recomendaciones-sobre-interlineado)
   - [Recomendaciones sobre espacios entre párrafos](#recomendaciones-sobre-espacios-entre-párrafos)
1. [Overflow](#overflow)
1. [Animaciones](#animaciones)
   - [media query `prefers-reduced-motion`](#media-query-prefers-reduced-motion)
   - [Desactivar animaciones manualmente](#desactivar-animaciones-manualmente)
   - [Flashes en pantalla](#flashes-en-pantalla)
1. [Uso de encabezados](#uso-de-encabezados)
1. [Uso de íconos](#uso-de-íconos)
   - [Hacer accesible a un SVG como ícono](#hacer-accesible-a-un-svg-como-ícono)
1. [Uso de HTML para mejorar legibilidad](#uso-de-html-para-mejorar-legibilidad)
   - [El atributo `lang`](#el-atributo-lang)
   - [El tag `abbr`](#el-tag-abbr)
1. [Especificaciones para dispositivos touch](#especificaciones-para-dispositivos-touch)
1. [Modo oscuro](#modo-oscuro)
1. [Modo de Alto Contraste de Windows](#modo-de-alto-contraste-de-windows)
   - [Consideraciones](#consideraciones-1)
   - [media query `forced-colors`](#media-query-forced-colors)

---

## Imágenes y texto alternativo.

Las imágenes deben de llevar un texto alternativo que permita dar contexto al usuario sobre la imagen en caso de que no pueda cargar o para que los lectores de pantalla puedan decirle a los usuarios invidentes de qué trata la imagen. Esto va en el atributo `alt` del tag de imagen.

```html
<img source="/images/image.jpg" alt="Imagen de dos personas hablando" />
```

### Consideraciones.

Algunas imágenes tienen una función en específico dentro del sitio, en este caso hay que tener algunas consideraciones al momento de decidir que alt se usa. Algunas de estas son:

- La imagen contiene texto y cumple una función, por ejemplo un ícono de un botón. Ese ese caso el alt _debería comunicar la función de la imagen_.
- La imagen es usada en un link o un botón y es imposible de decir que hará este elemento si la imagen no está. En este caso, el alt _debería comunicar la página de destino (en caso de ser o link) o función (en caso de ser un botón)_
- La imagen tiene texto y este **no** está presente en otro lado del sitio. En este caso el alt _debería tener el texto que está dentro de la imagen_.
- La imagen es un gráfico que muestra información que de otro modo haría difícil de entender el contenido del sitio. En este caso el alt _debería contener una breve explicación sobre el contenido de la imagen_.

Para entender más de este tema se puede recurrir al [árbol de decisión hecho por la WCAG sobre si añadir o no un alt](https://www.w3.org/WAI/tutorials/images/decision-tree/).

### Excepciones.

Existen algunas excepciones en las que el alt se puede dejar vacío (lo que hará que la imagen no sea reconocible a través de un lector de pantalla).

- La imagen tiene texto, pero dicho texto se presenta luego en el sitio.
- La imagen tiene texto pero sólo es usado con fines decorativos.
- La imagen es con fines decorativos y no aporta nada al usuario.

### Accesibilidad AAA

Para lograr el estándar AAA de accesibilidad, es importanter que las imágenes que contienen texto _sólo se usen para decoración o cuando su uso es esencial para mostrar la información de la que se quiere hablar_

> Para ese estándar, los logotipos se consideran imágenes esenciales, así que se pueden usar como imágenes, siempre que tengan el alt adecuado.

---

## Contraste.

Al momento de crear los distintos elementos de un sitio, es importante que se distingan del fondo. Para ello hay que asegurarnos que tengan el contraste adecuado dependiendo del estándar del proyecto. El estándar de contraste debe superar los siguientes valores

| **Elemento**                              | **AA**  | **AAA** |
| ----------------------------------------- | ------- | ------- |
| Texto normal                              | _4.5:1_ | _7:1_   |
| Texto grande                              | _3:1_   | _4.5:1_ |
| Objetos gráficos y elementos interactivos | _3:1_   | _3:1_   |

Para poder medir el contraste entre el texto y el background se puede usar [este medidor de contraste](https://webaim.org/resources/contrastchecker/).

---

## Uso del color

No se puede depender sólo del color para darle feedback al usuario sobre algo que ocurre en pantalla. Hay usuarios que perciben el color diferente por distintos tipos de daltonismo. Por ello es importante tener en cuenta las siguientes consideraciones

- En el caso de qe sea un color de fondo o de texto que se use para identificar algún error o instrucción. Siempre se pueden acompañar de otros elementos como íconos o texto. Siempre manteniendo en cuenta los estándares de contraste.
- Si es un gráfico o una imagen que se usa para mostrar información específica y depende del color, es importante añadir patrones para poder mejorar su visibilidad.

  ![Un gráfico de barras en el que cada barra tiene un patrón de diseño distinto](https://www.audioeye.com/static/5a08682a9aabd0a665aa4240d42bd7c4/73e0d/3dfedb33-d236-48c7-9d25-93ff03f461e2_Designing-for-Color-Blindness-Textures-Patterns.webp)

- Ante la duda de si algo se ve bien o no para las personas con distintos grados de daltonismo, se puede usar la pestaña de **Rendering** de DevTools. Para verla, una vez abierto DevTools se hace clic en los 3 puntos a la parte derecha, luego a _Más herramientas_ y luego se selecciona _Rendering_.

  ![Pantallazo de Chrome DevTools mostrando el menú de más herramientas y luego el de Rendering](/images/DevTools.png)

  En este modo se puede ir a la opción de _Emular deficiencias de color_ y seleccionar cualquiera de los filtros en la parte de _Emular deficiencias de visión_. El siguiente screenshot muestra como se vería una página con el filtro de _Deuteranopia_

  ![Pantallazo del sitio de OVAS Unad con el filtro de Deuteranopía de la herramienta de Chrome DevToolscf](/images/DevTools2.png)

  Si la información y los colores que usamos para mostrar algún tipo de información específica siguen siendo distinguibles en etsa emulaciones de color, significa que está bien y no se requieren medidas adicionales.

---

## Navegación por teclado

### Estado de focus.

Es importante que en elementos interactivos como botones o links sean navegables a través de teclado. Eso es para tener en cuenta gente con dificultades motrices que dependen del teclado para navegar por un sitio.

Para eso es importante darle al usuario un **indicador de focus** que muestra cuál es el elemento que está seleccionado en pantalla a través de teclado. Para eso se usa la pseudo-class `:focus` en CSS para darle estilos a este elemento.

Para cumplir el nivel de accesibilidad AAA, todos los elementos _funcionales_ del sitio deben ser navegables por teclado sin requerir tiempos específicos de uso de combinaciones de teclas.

### Indicador de focus.

El indicador de focus debe cumplir con un estándar de contraste en nuestro sitio. Siendo concretos, el contraste mínimo en estándar AA es de **3:1** y en estándar AAA es de **4.5:1**.

Para estos fines, el indicador de focus es el método que usamos para indicar que el usuario tiene seleccionado dicho elemento usando el teclado. Hay varios métodos para este indicador como por ejemplo usar un `outline` o cambiar el `background-color`. Algunos ejemplos sobre el indicador de focus serían los siguientes.

![Illustration: On the left is a blue button with a white label in its default, unfocused state. In the middle is the blue button with a thick black outline around it. On the right, is a button with the same outline but with a pattern applied to it, indicating that this patterned area is the contrasting area.](https://d33wubrfki0l68.cloudfront.net/1e9a7bbfb22b70127e03aaf7e84d46b8c2d3e1b1/d4d41/images/article__focus-indicators/focus-indication-area-1.jpg)

![Illustration: On the left is a blue button with a white label in its default, unfocused state. In the middle is the blue button with a separated thick black outline. On the right, is a button with the same outline but with a pattern applied to it, indicating that this patterned area is the contrasting area.](https://d33wubrfki0l68.cloudfront.net/14f88ace50893cf159d9acd25d799b994b24e00f/a2d70/images/article__focus-indicators/contrasting-area-2.jpg)

![Illustration: On the left is a blue button with a white label in its default, unfocused state. In the middle is the blue button with an inner thick black outline. On the right, is a button with the same outline but with a pattern applied to it, indicating that this patterned area is the contrasting area.](https://d33wubrfki0l68.cloudfront.net/ebb71a73b11571ea6c38495b00f8ab36cf350b11/7cb0f/images/article__focus-indicators/contrasting-area-3.jpg)

![Illustration: On the left is a blue button with a white label in its default, unfocused state. In the middle is the button in its focused state, having a black background instead of blue. On the right, is a button with with a pattern applied to its background area, indicating that this patterned area is the contrasting area.](https://d33wubrfki0l68.cloudfront.net/46dc1be497bd385198a49974a57bfc224dcb2c12/c6f04/images/article__focus-indicators/contrasting-area-4.jpg)

En caso de necesitar más información, [Este artículo de Sara Soueidan](https://www.sarasoueidan.com/blog/focus-indicators/) explica claramente estos estándares.

### Atributo `tabindex`

Hay algunos elementos que necesitan ser navegables por teclado que por defecto no son navegables. Para eso se puede usar la propiedad de HTML llamada `tabindex` que cambia el cómo son navegados. Esta propiedad tiene tres casos de uso

- `tabindex="0"` permite que un elemento que normalmente no es navegable pueda recibir estado de focus. Esto es útil para el caso anteriormente mencioando
- `tabindex="1"` o con un número positivo cambia el orden de navegación, hace que al navegar un sitio por teclado, primero pase por los que tienen `tabindex="0"` (o sean navegables por teclado por defecto), luego los que tienen `tabindex="1"` y así sucesivamente.
- `tabindex="-1"` o usando números negativos (aunque por convención es normal solo usar `tabindex="-1"`) significa que el elemento debe ser enfocado, pero no debe de ser accesible a través de la navegación secuencial del teclado.

---

## Esconder visualmente un elemento (pero no de lectores de pantalla)

Al usar la regla `display: none` o `visibility: hidden` con CSS escondemos un elemento tanto de la pantalla como de lectores de pantalla. También podemos usar el atributo de HTML `aria-hidden="true"` para esconder un elemento de lectores de pantalla pero no del sitio.

La siguiente clase permite esconder elementos de forma visual, pero que sigan siendo visible de lectores de pantalla

```css
.sr-only {
  border: 0;
  clip: rect(0 0 0 0);
  height: auto;
  margin: 0;
  overflow: hidden;
  padding: 0;
  position: absolute;
  width: 1px;
  white-space: nowrap;
}
```

Hay algunas razones por la que querramos hacer esto:

- Para esconder elementos que son necesarios por semántica de HTML pero que no tiene sentido mostrar en pantalla (como algún encabezado o un label).
- Para esconder una región con `aria-live` que sirva para dar información de cambios en pantalla a lectores de pantalla.
- Cuando se necesite usar algo para lo que el atributo de HTML `aria-label` funcione, pero que se necesite ser traducido. Los lectores de pantalla tienen problemas traduciendo un `aria-label` a otros idiomas, cosa que no pasa con un elemento de HTML.

---

## `aria-label` y `aria-labelledby`

En ciertos casos, como en un botón donde hay sólamente un ícono, es necesario agregar alguna etiqueta para que los lectores de pantalla puedan reconocer un elemento. Un modo de hacerlo es esconder un elemento de forma visual como se hizo anteriormente, pero otro modo es usando los atributos de HTML `aria-label` y `aria-labelledby` que agregan ese contexto a lectores de pantalla.

### `aria-label`

Este atributo permite etiquetar elementos para los lectores de pantalla. Un ejemplo de como sería su uso es el siguiente:

```html
<button aria-label="Reproducir">
  <svg aria-hidden="true"></svg>
</button>
```

En este caso el botón sería anunciado como: _Reproducir, botón_

### `aria-labelledby`

Este atributo usa un elemento con un ID específico para etiquetar un elemento para lectores de pantalla. Un ejemplo de esto sería el siguiente.

```html
<section aria-labelledby="section-title">
  <h2 id="section-title">Actividades</h2>
  <ul>
    <li></li>
    <li></li>
    <li></li>
  </ul>
</section>
```

Un lector de pantalla leería esto como: _Actividades, sección_ y luego ignoraría el `h2` (por ser usado para etiquetar la sección) y proseguiría a anunciar los items de la lista.

### Cuándo usar cada caso

Ambos tienen la misma función, la mayor diferencia entre el uno y el otro es que el `aria-labelledby` permite reducir redundancias en caso de que la etiqueta de dicho elemento coincida con un elemento interno de este (como vimos en el ejemplo de `section`).

Es importante recordar que en ciertos elementos `aria-label` va a reemplazar el contenido interno, y en todos los casos `aria-labelledby` va a reemplazar `aria-label`. **No se deben usar ambas en el mismo elemento**

Algunos elementos no deberían tener `aria-label`. Para más información sobre el tema se puede consultar [la tabla dentro de este artículo](https://html5accessibility.com/stuff/2020/11/07/not-so-short-note-on-aria-label-usage-big-table-edition/).

---

## Creación de inputs.

Siempre que se cree un input es necesario agregar un label. De otro modo los lectores de pantalla no van a darle información suficiente al usuario para que sepa qué es lo que debe diligenciar. Las semánticas aceptadas para esto son las siguientes:

```html
<label for="data">Información</label>
<input id="data" type="text"></input>
```

```html
<label for="data">
  Información
  <input id="data" type="text"></input>
</label>
```

El atributo `for` permite relacionar el label con el `id` del input.

Hay casos en los que es necesario que el label no se vea. Para esto usamos la clase para esconder visualmente un elemento, como se puede ver en los siguientes ejemplos.

```html
<label class="sr-only" for="data">Información</label>
<input id="data" type="text"></input>
```

```html
<label for="data">
  <span class="sr-only">Información</span>
  <input id="data" type="text"></input>
</label>
```

---

## Zoom en el sitio.

Con excepción de los captions de un video y las imágenes, para alcanzar el estándar AA de accesibilidad es importante que nuestro sitio funcione un zoom hasta de **200%** sin que pierda contenido o funcionalidad.

Para esto es importante tener algunos temas en cuenta

- Los textos e imágenes deben ser expresados en unidades relativas como `rem` o `em` y no en `px` porque al momento de hacer zoom, lo que se exprese en `px` no va a cambiar de tamaño.
- En caso de elementos que estén de forma permanente en pantalla (ya sea con `position: fixed` o `position: sticky`) es importante darles anchos y altos máximos para evitar que ocupen demasiado espacio en pantalla y entorpezan el uso del sitio.

---

## Visualización de textos

Con el fin de facilitar la legibilidad del texto y cumplir el estándar AA de accesibilidad, es importante que nuestro texto cumpla como mínimo los siguientes requerimientos:

- Un `line-height` de al menos 1.5 (con excepción de textos grandes).
- Un espacio entre párrafos de al menos 2 veces el tamaño de la fuente.
- Un `letter-spacing` de al menos 0.12 veces el tamaño de la fuente.
- Un `word-spacing` de al menos 0.16 veces el tamaño de la fuente.

Adicionalmente, para cumplir con el nivel AAA de accesibilidad, es importante que los párrafos tengan las siguientes consideraciones.

- Los colores de fondo y del texto pueden ser escogidos por el usuario.
- El ancho de los párrafos no pueden ser ser mayores a 80 caractéres (para ello se puede usar la propiedad `max-width: 80ch`)
- El texto **no** debe estar justificado ya que esto dificulta la lectura.
- Se puede aumentar el tamaño del texto hasta en un 200% del tamaño normal sin que genere comportamientos como aumentar el scroll horizontal o que se lea sólo una línea de texto en pantalla a la vez.

### Recomendaciones sobre interlineado.

Normalmente las fuentes cumplen estas dos últimas condiciones por defecto. Algo que podemos asegurarnos para cumplir la condición del `line-height` es usar esto en nuestros estilos a modo de hoja de estilos para reset.

```css
body {
  line-height: 1.5;
}
```

Para textos más grandes -como encabezados- es posible usar un interlineado más pequeño. Algo que se puede hacer para crear un interlineado que sea más pequeño a medida que crezca el tamaño de fuente es lo siguiente.

> El siguiente método requiere pruebas para asegurarse de que no haya un comportamiento inesperado, por lo cual debe usarse con precaución. Para más información ver esta sección de [Este artículo creado por Josh Comeau](https://www.joshwcomeau.com/css/custom-css-reset/#digit-tweaking-line-height).

```css
body {
  line-height: calc(1em + 0.5rem);
}
```

### Recomendaciones sobre espacios entre párrafos

Con respecto a mantener el espacio entre párrafos, algo que se puede hacer es usar la siguiente clase:

```css
.flow > * + * {
  margin-block-start: var(--flow-space, 2em);
}
```

Al aplicar la clase `flow` a un elemento padre, todos sus elementos hijos salgo el primero ganan un `margin-top` que corresponda con la variable `--flow-space` y de no estar definida, sería de `2em`.

> Se requiere experimentación sobre el 2em en pantallas más pequeñas.

---

## Overflow

Para cumplir el estándar de accesibilidad AA, el contenido debe ser presentado sin que represente una pérdida de funcionalidad o de información sin que haya un scroll en contenedores de estos tamaños:

- No debe haber scroll horizontal si el contenedor mide _256 px_ o menos.
- No debe haber scroll vertical si el contenedor mide _320 px_ o menos.

Lo anterior no aplica a cierto contenido como:

- Mapas y diagramas.
- Presentaciones.
- Juegos.
- Interfaces en las que se necesite mantenerse visible una barra de herramientas para interactuar con el sitio.

---

## Animaciones.

Las animaciones son una parte importante de la elaboración de un sitio, pero hay usuarios que por distintas razones (vértigo, migrañas, problemas en su sistema vestibular) deciden desactivar las animaciones de su computador. Dicha configuración debemos respetarla y para ello tenemos dos acercamientos

### Media query `prefers-reduced-motion`.

CSS nos permite respetar dichas preferencias usando la media query `prefers-reduced-motion`. Esta media query tiene dos valores: `no-preference` o `reduce` y nos permite colocar reglas de CSS dentro de esta en caso de que el usuario no tenga preferencias sobre la animación o en caso de que tenga la animación desactivada, respectivamente.

Podemos usar esta media query de dos maneras:

1. **Usar la opción del valor `no-preference` para poner las animaciones dentro de esta:** así nos aseguramos que en el caso de que el usuario tenga las animaciones desactivadas no se muestren y sólo se vean en caso de que no haya preferencia algúna. Un ejemplo puede ser el siguiente:

   ```css
   @media (prefers-reduced-motion) {
     .cta {
       animation: pulse 1s linear infinite both;
     }
   }
   ```

2. **Usar un snippet de código dentro de la preferencia `reduce` para desactivar todas las animaciones:** otra opción para respetar las preferencias es usar el siguiente código para desactivar todas las animaciones si el usuario ha desactivado las animaciones.

   ```css
   @media (prefers-reduced-motion: reduce) {
     *,
     *::before,
     *::after {
       animation-duration: 0.01ms !important;
       animation-iteration-count: 1 !important;
       transition-duration: 0.01ms !important;
       scroll-behavior: auto !important;
     }
   }
   ```

El `!important` se asegura que sobreescriba todas las animaciones existentes. Esto puede ser útil pero es importante recordar que el hecho de que el usuario prefiera una experiencia reducida de animaciones **No quiere decir que no se puedan usar animaciones en absoluto**.

A veces con crear una animación mucho más sutil dentro de la media query `reduce` ayuda a generar una mejor experiencia. Un ejemplo de esto se puede ver en [este artículo de Smashing Magazine](https://www.smashingmagazine.com/2020/09/design-reduced-motion-sensitivities/#an-example-of-reducing-motion). El primer video muestra una animación bajo la preferencia `no-preference` y la segunda bajo la preferencia `reduce`.

### Desactivar animaciones manualmente

Para cumplir el estándar AAA de accesibilidad, es necesario generar una opción de forma manual para dar una experiencia de animación reducida.

### Flashes en pantalla.

Algunas animaciones pueden requerir el uso de flashes pero es importante que _ninguna animación tenga más de 3 flashes en el paso de un segundo_. De otro modo **esto puede causar convulsiones en algunos usuarios** por lo que respetar esto es **muy** importante.

> Para esta norma se entiende por flash cualquier cambio en la luminosidad de 10% o más. Es particularmente importante evadir cualquier cambio de luminosidad que implique el uso de un rojo con alta saturación.

---

## Uso de encabezados

El uso de los tags de `<h1>` a `<h6>` \*no debería hacerse de acuerdo al tamaño del texto, si no de acuerdo a la estructura que estos creen.

Los lectores de pantalla generan algo llamado un _outline_ o _esquema_ con base en los encabezados de un sitio. Una buena analogía de esto sería una tabla de contenido. Un uso correcto de los encabezados permite generar una tabla de contenido que permita a los usuarios de lectores de pantalla navegar rápidamente a las secciones que quieren visitar. A continuación se puede ver como luce un buen outline de un sitio:

![Vistazo de un outline de un sitio a través de las herramientas de desarrollo de un navegador llamado Polypane](https://i.imgur.com/ry4rsHQ.png)

Como se puede ver en el screenshot, los encabezados `<h1>`, `<h2>` y `<h3>` forman una estructura lógica de cómo es el contenido del sitio y de esto depende un usuario que navega el sitio con un lector de pantalla para encontrar el contenido que le interesa.

Por esta razón, _usar los encabezados con base en su tamaño es un error_ y por lo tanto debería tenerse en cuenta su valor semántico para generar un esquema de navegación adecuado. Para manejar el tema del tamaño **es mejor usar CSS en lugar de recurrir a los encabezados únicamente por su tamaño**.

Es posible que para crear una buena estructura tengamos que añadir encabezados que, aunque para los lectores de pantalla funcionen bien, a nivel visual se consideren ruido. Para esto podemos esconder visualmente los encabezados con la clase `sr-only`.

---

## Uso de íconos

Hay distintas maneras de agregar íconos a los sitios y algunos son más accesibles que otros. Cosas como **Material Icons** o **Font Awesome** que usan texto para renderizar íconos no son particularmente accesibles por ciertas razones:

- Los lectores de pantalla _pueden_ leer el contenido de esos textos que se usan, lo que puede generar una experiencia incómoda. Esto se puede mitigar con un `aria-hidden="true"` en el elemento donde se encuentre el ícono.
- Hay gente con dislexia que reemplaza la fuente de la página con la propia, y este reemplazo también afecta a las usadas con estos generadores de íconos, lo cual genera una experiencia frustrante.
- En el caso de que no se puedan renderizar, la opción alternativa que muestra no es muy útil, lo cual quita contexto sobre qué hace el elemento con el ícono, tal como se puede ver acá.

![Ejemplo de Microsoft de caractéres perdidos](https://cloudfour.com/wp-content/uploads/2015/11/notdef.png)

Por estas razones deberíamos hacer lo posible para _no_ usar estos generadores de íconos en los proyectos.

Las imágenes son buenos sustitutos de íconos siempre que su `alt` sea significativo (véase sección de Imágenes y texto alternativo).

La mejor opción son los SVG: son accesibles, fáciles de implementar y pueden dar pie a interacciones interesantes con CSS y JavaScript.

### Hacer accesible a un SVG como ícono

Para usar un SVG como ícono y hacerlo accesible hay que tener dos cosas en cuenta:

- **Usar el atributo `role="img"`:** Esto hará que un lector de pantalla reconozca el SVG como una imagen.

- **Usar el tag `title`:** El tag `title` será leido por lectores de pantalla para darle contexto a la gente que use lectores de pantalla para navegar por un sitio, de la misma manera que el atributo `alt` lo hace para las imágenes. También es posible usar el atributo `aria-labelledby` para hacer que el lector de pantalla use el tag `title` para describir el SVG.

  _Sin usar `aria-labelledby`_

  ```html
  <svg version="1.1" width="300" height="200" role="img">
    <title>Green rectangle</title>
    <rect
      width="75"
      height="50"
      rx="20"
      ry="20"
      fill="#90ee90"
      stroke="#228b22"
      stroke-fill="1"
    />
  </svg>
  ```

  _Usando `aria-labelledby`_

  ```html
  <svg
    version="1.1"
    width="300"
    height="200"
    aria-labelledby="title"
    role="img"
  >
    <title id="title">Green rectangle</title>
    <rect
      width="75"
      height="50"
      rx="20"
      ry="20"
      fill="#90ee90"
      stroke="#228b22"
      stroke-fill="1"
    />
  </svg>
  ```

---

## Uso de HTML para mejorar legibilidad

Uno de los requerimientos de la WCAG (Web Content Accessibility Guidelines) es que el contenido de la interfaz de usuario sea entendible. Hay un par de tags y atributos de HTML que podemos tener en cuenta para ayudar a este fin:

### El atributo `lang`

El atributo `lang` del sitio identifica el lenguaje que usa el sitio web. Esto es importante debido al contenido que maneja Books and Books. Si la mayoría del sitio está, por ejemplo, en inglés **el atributo del tag `html` del sitio debería ser `lang="en"`**.

Esto no sólo aplica al tag `html`. Para cumplir el nivel de accesibilidad AA, es importante que si un sitio en el que hay un idioma predominante (por ejemplo, español) presenta luego un contenido en otro idioma (por ejemplo, inglés), el contenido de esa parte en idioma extranjero debe tener el atributo `lang`. Por ejemplo, asumamos que el tag `html` de esta parte es `lang="es"`

```html
<p>
  El contenido del sitio está normalmente en español
  <span lang="en">but this part of the site is in English</span>
</p>
```

> Para evitar que el contenido o una parte del sitio sea traducido, se puede colocar el atributo `translate="no"` en el contenido que no querramos sea traducido (en caso de que sea todo el sitio, puede ir en el tag `html`).

### El tag `abbr`

Una de las condiciones para cumplir el nivel de accesibilidad AAA es que debe haber un mecanismo para conocer el significado de una abreviación. Para esto podemos usar el tag de HTML `abbr` usando el atributo `title` para escribir su significado. Ejemplo.

```html
<abbr title="Web Content Accessibility Guidelines">WCAG</abbr>
```

---

## Especificaciones para dispositivos touch

Por razones de usabilidad es importante tener en cuenta ciertas consideraciones al momento de diseñar para teléfonos móviles como las siguientes:

- El area de un elemento interactivo debe tener como mínimo un área de **24px x 24px** para estándar de accesibilidad AA y de **44 x 44px** para estándar de accesibilidad AAA con la excepción de que sea un texto o bloque de texto (como un link en medio de un párrafo). Algo que podemos hacer para garantizar que elementos como botones tengan el tamaño mínimo necesario es agregar esta regla dentro de la media query `any-pointer`.

  ```css
  @media screen and (any-pointer: coarse) {
    button {
      /* Estándar AA */

      min-width: 1.5rem;
      min-height: 1.5rem;

      /* Estándar AAA */

      min-width: 2.75rem;
      min-height: 2.75rem;
    }
  }
  ```

- El espacio entre elementos interactivos deben ser como mínimo de **8px**.
- No deberían haber elementos que dependan de un "hover" en elementos con pantalla touch. Esto es porque sin un puntero, el usuario tendría que presionar por un segundo la pantalla para poder ver el contenido, y eso crea una experiencia de usuario incómoda.

  Por ejemplo, veamos [esta card](https://codepen.io/crisdi384/pen/NWgOZjX). En dispositivos touch tendría problemas para verse el contenido. Para esto podemos usar las media queries `hover` y `pointer` para hacer que su animación solo se vea en equipos de escritorio/laptops

  ```css
  @media screen and (hover: hover) and (pointer: fine) {
    /* Reglas que activan la animación  */
  }
  ```

---

## Modo oscuro

El modo oscuro se originó como una opción de accesibilidad para reducir el brillo en pantalla, lo ayuda a disminuir la carga visual y es útil para usuarios que sufren de migrañas. A pesar de que la WCAG no tiene algún tipo de estándar AA o AAA para el uso de Dark Mode, _se considera un feature de accesibilidad y es importante implementarlo en la medida de lo posible_.

Un usuario puede marcar la preferencia en su navegador de si prefiere navegar usando el modo claro o el modo oscuro. Nosotros tenemos que respetar dicha preferencia. El mejor modo de hacerlo es a través de CSS usando la media query `prefers-color-scheme` que tiene tres valores: `light`, `dark` y `no-preference`.

El modo más fácil de hacer un dark mode es cambiar las custom properties del proyecto para que así se genere automáticamente el modo en caso de que el usuario tenga estas preferencia. Por ejemplo:

```css
:root {
  --clr-foreground: hsl(0 0% 0%);
  --clr-background: hsl(0 0% 100%);
}

@media (prefers-color-scheme: dark) {
  :root {
    --clr-background: hsl(0 0% 0%);
    --clr-foreground: hsl(0 0% 100%);
  }
}
```

---

## Modo de Alto Contraste de Windows

El Modo de Alto Contraste de Windows es una característica de accesibilidad que sobreescribe todos los colores en pantalla por tonos que tienen un contraste alto. Para activarlo, desde el menú de Windows seleccionamos _Configuración_, luego _Accesibilidad_ y luego vamos al modo de _Contraste alto_ y seleccionamos el que queremos usar.

Hay personas que lo usan por distintas razones: mejora la legibilidad del sitio, reduce la carga visual al eliminar varios elementos de un sitio. Algunos usuarios incluso usan este modo para escoger colores que tengan un _menor_ contraste de lo habitual, lo cual ayuda a usuarios con sensibilidad a la luz o migrañas.

### Consideraciones

Es importante tener ciertas consideraciones al momento de diseñar para un Modo de Alto Contraste.

- Este modo reemplazará todos los colores de fondo y texto que tengamos por los que se usa este modo. Así que es importante mantener un HTML semántico para asegurarse de no generar confusión al usuario al momento de usar este modo. Por ejemplo un `button` y un `div` que ejecuten la misma función al hacerles clic van a lucir diferentes en un modo de alto contraste.
- Es mala práctica usar reglas de CSS como `outline: none` a elementos en estado de focus porque en el modo de alto contraste va a ser el **único** indicador funcional que ayude a darle ese feedback visual al usuario. En su lugar se puede usar `outline-color: transparent` que tendrá el mismo efecto visual pero que en el modo de alto contraste se verá sin problemas.
- Por esto mismo, a elementos que necesitan diferenciarse del fondo como un `button` no debería usarse la regla `border: none`, ya que en este modo se mezclarán con el background y dificultará la lectura. En su lugar se debería usar `border-color: transparent` y así se distinguirá del fondo.
- Es buena práctica dejar los subrayados a los tags `a` en este modo, incluso si bajo condiciones normales no usamos el subrayado. Un buen modo de solucionar esto es usando la propiedad de CSS `text-decoration-color: transparent`.
- Las barras de desplazamiento personalizadas **no se van a ver en el Modo de Alto Contraste** por lo que es importante reconsiderar si es necesario agregarles estos estilos.

### Media query `forced-colors`

Aunque por regla general debería dejarse que el sistema decida los colores bajo este método, podemos usar la media query `forced-colors` para añadir reglas adicionales que apliquen bajo el Modo de Alto Contraste. Para agregar reglas en este modo se escribiría de la siguiente manera:

```css
@media screen and (forced-colors: active) {
  /* Reglas de CSS  */
}
```

No todas las reglas de CSS van a funcionar en este modo, o al menos no del modo esperado, por ejemplo las siguientes sólamente van tomar en cuenta los colores que se usen en este modo:

- `color`
- `background-color`
- `text-decoration-color`
- `text-emphasis-color`
- `border-color`
- `outline-color`
- `column-rule-color`
- `-webkit-tap-highlight-color`
- SVG `fill` attribute
- SVG `stroke` attribute

Y las siguientes reglas van a tener los siguientes valores:

| **Regla**          | **Valor**                                  |
| ------------------ | ------------------------------------------ |
| `box-shadow`       | `none`                                     |
| `text-shadow`      | `none`                                     |
| `background-image` | `none` excepto para valores que sean `url` |
| `color-scheme`     | `light dark`                               |
| `scrollbar-color`  | `auto`                                     |

Además de las reglas estándar de CSS tenemos una regla y unas propiedades nuevas por usar. La regla en cuestión es `forced-color-adjust: none` que hará que los colores de este elemento _no_ cambien cuando se esté en el modo de alto contraste.

> Este cambio sólo se debe hacer si **el color de este elemento es usado para transmitir información importante**.

Adicionalmente, si se desea escribir un color de un elemento, se pueden usar las siguiente palabras reservadas en propiedas que tengan que ver con color (como `border-color` o `color` para los textos):

- **ActiveText:** links activos.
- **ButtonBorder:** color de borde de los botones.
- **ButtonFace:** color de background de botones.
- **ButtonText:** color del texto de los botones.
- **Canvas:** color de fondo del sitio.
- **CanvasText:** color del texto del sitio (en general).
- **Field:** color de fondo de inputs.
- **FieldText:** texto de inputs.
- **GrayText:** texto de campos deshabilitados.
- **Highlight:** color de fondo de texto seleccionado.
- **HighlightText:** color del texto seleccionado.
- **LinkText:** texto de links no visitados que no estén activos.
- **Mark:** color de fondo de texto que esté resaltado (es posible hacer eso usando la etiqueta de HTML `mark`)
- **MarkText:** color del texto que ha sido marcado (de nuevo, con la etiqueta HTML `mark`)
- **VisitedText:** texto de links visitados

> Es importante que estos cambios de colores sólo se deberían hacer para hacer pequeños ajustes con el fin de hacer la página más consistente en estos métodos.

> Al final del día el Modo de Alto Contraste es algo que prioriza la funcionalidad sobre el diseño y debemos depende más de temas como usar las etiquetas de HTML correctas y usar bordes, outlines y subrayados transparentes para que el sitio se vea bien ahí.
