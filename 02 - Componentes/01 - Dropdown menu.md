# Dropdown menu

## Markup

```html
<nav aria-label="Menú principal">
  <button type="button" aria-controls="menu-list" aria-expanded="false">
      <span class="sr-only"></span>
      <svg aria-hidden="true">
        <!-- Código del SVG -->
      </svg>
  </button>
  <ul id="menu-list" role="list">
    <li>
      <a href="#home">Home</a>
    </li>
    <li>
      <a href="#sounds">Sounds</a>
    </li>
  </ul>
<nav>
```

## Explicación

- El `nav` necesita tener un `aria-label` para que los lectores de pantalla puedan reconocer de qué trata la sección. En un lector de pantalla se leería algo como

    > Menú principal, navegación

- Por precaución, el elemento `button` debe tener el `type="button"` para evitar que en caso de que haya un formulario en la misma página, este botón recargue el sitio.

- `aria-controls` funciona solo con el lector de pantalla *JAWS* para relacionar el botón con el elemento del mismo id (en este caso, el `ul`) A pesar de funcionar en sólo un lector, es mejor dejarlo porque muchos usuarios de JAWS esperan este funcionalmento. En este lector se escucharía algo así.

    > "Presiona la tecla JAWS + Alt y M para manejar el elemento controlado."

- `aria-expanded` indica si el botón está contraido o expandido, esto da contexto al usuario sobre si el menú está abierto o no. En un lector de pantalla se escucha así.

    > Menú, botón. Contraido.

- El nombre del menú está con la clase `sr-only` para ocultarlo visualmente pero no de lectores de pantalla. Opcionalmente se puede omitir este elemento y reemplazarlo por poner un `aria-label` en el botón.

- El SVG lleva el atributo `aria-hidden="true"` para que el lector de pantalla no lo reconozca. 

- El `ul` lleva el atributo `role="list"` por con VoiceOver (el lector de pantalla de Apple) es posible que si a una lista (ordenada o no ordenada) le quitas el `list-style` con CSS, deje de reconocerlo como una lista como tal.

- Es posible usar los roles `role="menu"` en el `ul` y `role="menuitem"` en el menú, siempre y cuando *los elementos del menú no sean links*. Se puede usar para, por ejemplo, añadir un listado de botones que cumplan con funciones específicas.

## JavaScript

Con JavaScript es necesario crear un eventListener que cambie la propiedad `aria-expanded` del botón cuando se haga clic. Y es importante que cuando `aria-expanded="true"` el `ul` con las opciones de navegación se muestre y cuando sea `aria-expanded="false"` se oculte.