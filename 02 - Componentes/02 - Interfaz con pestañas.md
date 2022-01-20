# Interfaz con pestañas

## Markup

```html
<ul role="tablist" aria-label="Navegación de secciones">
  <li role="presentation">
    <a role="tab" href="#section1" id="tab1" aria-selected="true">Sección 1</a>
  </li>
  <li role="presentation">
    <a role="tab" href="#section2" id="tab2">Sección 2</a>
  </li>
  <li role="presentation">
    <a role="tab" href="#section1" id="tab3">Sección 3</a>
  </li>
</ul>

<section role="tabpanel" id="section1" aria-labelledby="tab1">
  <!-- Contenido  -->
</section>

<section role="tabpanel" id="section2" aria-labelledby="tab2" aria-hidden="true">
  <!-- Contenido  -->
</section>

<section role="tabpanel" id="section3" aria-labelledby="tab3" aria-hidden="true">
  <!-- Contenido  -->
</section>
```

## Explicación

- El agregar el `role="tablist"` al `ul` le agrega semántica de lista de pestañas. Opcionalmente se puede añadir un `aria-label` para agregarle una descripción esta. En este caso, un lector de pantalla leería esta sección como *Navegación de secciones, lista de pestañas*
- Sin embargo, lo anterior *no* es suficiente para añadir el contexto necesario, por eso agregamos el `role="tab"` a los `a` para indicar que esta es la pestaña. Un lector de pantallas leería esta sección como *Navegación de secciones, lista de pestañas. Sección 1, pestaña 1 de 3*.
- `aria-selected` le añade un *Seleccionado* al final del link y sirve para indicarle a los usuarios que este elemento es el que está activo.
- El `role="tabpanel"` le agrega contexto a los contenedores donde está la información. El `aria-labelledby` hace que sean etiquetados con el `a` que tiene se ID. Dependiendo de como se estructure el contenido de la sección se puede usar el encabezado de la sección en lugar del `a` para darle contexto. En este caso el lector de pantalla lo reconoce como *Sección 1, página de propiedades*.
- El atributo `aria-hidden="true"` esconde los tabpanels que no están seleccionados de los lectores de pantalla.
- Opcionalmente, se le puede agregar el atributo `tabindex=0` a los tabpanels para que sean seleccionables por teclado.

## CSS

Con CSS podemos usar el atributo `aria-hidden="true"` para ocultar los tabpanels que no estén seleccionados de esta manera.

```css
section[role="tabpanel"][aria-hidden="true"] {
  display: none;
}
```

## JavaScript

Con JavaScript es necesario hacer las siguientes tareas para asegurarse de que las pestañas sean funcionales.

- Cambiar el `aria-selected` en los `a` dependiendo de a cuál se le haga clic y remover el `aria-selected` de los otros.
- Así mismo, dependiendo de a qué `a` se le haga clic, cambiar o quitar los atributos `aria-hidden` de los tabpanel dependiendo de a qué `a` se le haga clic.
- Es importante que al momento de presionar las teclas <kbd>Shift</kbd> + <kbd>Tab</kbd> al inicio de la sección, regrese al tab seleccionado. Para eso con JavaScript haremos que todos los `tab` menos el seleccionado tengan el atributo `tabindex="-1"`. Asumiendo que el usuario hizo clic al tab de la Sección 1, quedaría de la siguiente manera:

  ```html
  <ul role="tablist" aria-label="Navegación de secciones">
    <li role="presentation">
      <a role="tab" href="#section1" id="tab1" aria-selected="true">Sección 1</a>
    </li>
    <li role="presentation">
      <a role="tab" href="#section2" id="tab2" tabindex="-1">Sección 2</a>
    </li>
    <li role="presentation">
      <a role="tab" href="#section1" id="tab3" tabindex="-1">Sección 3</a>
    </li>
  </ul>
  ```



