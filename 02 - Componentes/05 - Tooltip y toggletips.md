# Tooltip

Un tooltip es una ventana con texto que aparece al momento de poner el mouse sobre un elemento, con el fin de ofrecer alguna información para ayudar al usuario.

## ¿Es necesario usarlos?

Normalmente un tooltip se usa para darle información al usuario sobre qué hace un elemento que no tiene suficiente contexto por sí solo (como un botón que sólo tiene un ícono) pero la falta de espacio viene precisamente porque no se ha considerado desde el área de diseño que dicho contenido explicativo esté desde un inicio.

![Pantallazo del libro de "Inclusive Components" de Heydon Pickering donde muestra maneras de colocar íconos y la información relacionada. Abajo está la cita de "Siempre hay espacio para texto si lo haces, pero en algunas configuraciones hay más espacio para texto que en otros"](/images/Tooltip.png)

Por eso es importante que en caso de ver un escenario donde se use un ícono sin mayor explicación, _hablar con el área de diseño para que se pueda mostrar la etiqueta del elemento_ y de ahí maquetar. Un tooltip propiamente dicho **debería usarse como último recurso** debido a sus consideraciones para hacerlo accesible.

> Está bien usar sólamente un ícono en botones siempre que sea algo universalmente aceptado (como los botones de reproducir, pausar, grabar o el botón de inicio) _siempre y cuando tengan una etiqueta que funcione para lectores de pantalla_ (como `aria-label`, `aria-labelledby` o esconder un texto visualmente)

## Markup

```html
<div class="button-with-tooltip">
  <button
    type="button"
    class="notifications"
    aria-labelledby="notifications-label"
  >
    <svg>
      <!-- Diseño del SVG  -->
    </svg>
  </button>
  <div role="tooltip" id="notifications-label">Notificación</div>
</div>
```

## Explicación

- El `button` tiene el atributo `type="button"` para evitar que el navegador confunda el botón con el que usa un `<form>` para enviar la información.
- Al final del día, un tooltip va a ser un elemento que funcione a modo de descripción del elemento interactivo (en este caso un botón), así que usar la propiedad `aria-labelledby` para tomar el contenido del tooltip para describirlo es una estrategia válida que lo hace accesible.
- El `role="tooltip"` le añade contexto a _algunos_ lectores de pantalla sobre lo que es el elemento. Sin embargo, haciendo pruebas en NVDA, el rol de tooltip no añade descripción adicional. Pero usando ChromeVox (el lector de pantalla de Google Chrome) lo leería como _Notificación, sugerencia_ (Se necesita probar en diferentes lectores de pantalla)

## CSS

El tooltip se oculta normalmente y se mostrará cuando el usuario ponga el puntero sobre el elemento o lo seleccione con el teclado. Esto se puede lograr sólamente con CSS de esta manera:

```css
.button-with-tooltip {
  position: relative;
}

[role="tooltip"] {
  position: absolute;
  display: none;
}

button:hover + [role="tooltip"],
button:focus + [role="tooltip"] {
  display: block;
}
```

El problema del tooltip recide cuando se está en una pantalla de móvil, así que si es necesario mostrar la etiqueta hay que considerar tenerla desde un principio como se mencionó anteriormente. Si no es posible, podemos mostrar directamente el texto en dispositivos touch usando la media query hover

```css
@media screen and (hover: none) {
  .button-with-tooltip,
  [role="tooltip"] {
    position: static;
    display: block;
  }
}
```

## JavaScript

La principal consideración a tener en cuenta con JavaScript es que en pantallas que no son touch (porque en pantallas touch se debería mostrar el contenido del tooltip para iniciar) se pueda quitar el tooltip presionando la tecla <kbd>Esc</kbd>. Esto con fin de asegurarse que el sitio cumpla con estándar de accesibilidad AA.

# Toggletips

Los toggletips son una versión del tooltip en la que en lugar de mostrarse el contenido al hacer hover o focus, se muestra al hacerle clic al elemento. Esta es una versión más amigable para móvil porque no depende de un puntero, pero es más complicada de configurar y _no funcionará en un elemento que ya tiene una función al hacérsele clic_ por lo que su uso está netamente para informar a los usuarios del uso de otro elemento en pantalla.

## Markup

```html
<span class="toggletip-container">
  <button
    type="button"
    data-toggletip-content="Información que clarifica lo necesario sobre el elemento"
  >
    <span aria-hidden="true">i</span>
    <span class="sr-only">Más información</span>
  </button>
  <span role="status"></span>
</span>
```

## Explicación

- El `button` tiene el atributo `type="button"` para evitar que el navegador confunda el botón con el que usa un `<form>` para enviar la información.
- El atributo `data-toggletip-content` es la información que se verá en el toggletip. No la colocamos directamente en el `span[role="alert]` porque lo que haremos será colocar la información de ese atributo en una _aria region_, y para eso se necesita JavaScript.
- Normalmente un toggletip lleva como un ícono la letra _i_ (normalmente escrita con fuente monospace) así que usaremos la letra como su ícono y para eso la escondemos de lectores de pantalla con `aria-hidden="true"`.
- El elemento que dice "Más información" está oculto de forma visual pero no de lectores de pantalla usando la clase `sr-only`.
- El `role="status"` lo vuelve lo que en aria se llama un _live region_ que es una región que al momento de recibir cualquier cambio en su contenido, lo anunciará a los lectores de pantalla. Un `role="tooltip"` no funcionaría en este caso porque este **no** funciona como un live region.

## JavaScript

Independientemente de cómo se elija renderizar el contenido, es importante tener en cuenta estas consideraciones al momento de crear un toggletip:

- El toggletip debe abrirse sólo al hacer clic, o al presionar las teclas <kbd>Enter</kbd> o <kbd>Space</kbd> (todo eso se soluciona si se usa el `button` para activar el toggletip).
- El toggletip debe desactivarse bajo estas tres condiciones.
  - Al hacer clic fuera del toggletip.
  - Al dejar de hacerle focus al botón.
  - Al presionar la tecla <kbd>Esc</kbd>.

A continuación un ejemplo de como ejecutar este tooltip teniendo en cuenta el markup sugerido arriba.

```js
const createToggletipContent = function () {
  // Selecciona todos los toggletips
  const toggletips = (document.querySelectorAll = "[data-toggletip-content]");

  // Función que procesa los toggletips

  function addLiveRegionContent(element) {
    // Selecciona el mensaje dentro de data-toggletip-content
    let message = element.getAttribute("data-toggletip-content");

    // Selecciona la live region
    let liveRegion = element.nextElementSibling;

    // Añadir el mensaje al momento de hacer clic

    element.addEventListener("click", () => {
      liveRegion.innerHTML = "";

      // El contenido se añade con setTimeout porque al cambiar el contenido de un elemento vacío a un mensaje es que anuncia el cambio
      window.setTimeout(function () {
        liveRegion.innerHTML = `<span class="toggletip-bubble">${message}</span>`;
      }, 100);
    });

    // Quitar toggletip de pantalla al hacer clic fuera de este

    document.addEventListener("click", (e) => {
      if (element !== e.target) liveRegion.innerHTML = "";
    });

    // Quitar toggletip al momento de dejar de hacerle focus al botón

    element.addEventListener("blur", (e) => {
      liveRegion.innerHTML = "";
    });

    // Quitar toggletip al presionar la tecla ESC

    element.addEventListener("keydown", (e) => {
      if ((e.keyCode || e.which) === 27) liveRegion.innerHTML = "";
    });

    // Ejecutar esta función con cada toggletip
    toggletips.forEach(element, addLiveRegionContent());
  }
};
```
