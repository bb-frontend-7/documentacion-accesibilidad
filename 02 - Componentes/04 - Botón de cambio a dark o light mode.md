# Botón para cambiar entre modo claro y oscuro

## Markup

```html
<button id="theme-toggle" type="button" aria-labelledby="theme-toggle-text">
  <span id="theme-toggle-text">Cambiar a modo oscuro</span>
  <svg>
    <!-- Diseño del SVG -->
  </svg>
</button>
```

## Explicación

- El `type="button"` está por precaución. Si el sitio tiene un formulario, es posible que el botón active el envío de dicho formulario si no usamos este atributo.
- `aria-labelledby` Está para usar el `span` como etiqueta del botón. Podría dejarse sin `aria-label` y esconder visualmente el `span`, pero no se hace por dos razones:

  - Con JavaScript vamos a estar cambiando el contenido del `span` dependiendo de si se ha activado o no el modo oscuro. El cambiar el contenido de este y relacionarlo como el `aria-labelledby` del botón hará que al momento de hacerle clic el lector de pantalla anuncie el cambio, dándole el contexto necesario a un usuario de estas herramientas.
  - Usando `aria-labelledby` en el botón, podemos ponerle la propiedad `display: none` en el `span` e igual funcionará en lectores de pantallas. Eso con el fin de que si necesitamos usar este mismo span como un tooltip al momento de que el botón tenga el estado hover o focus, podamos sobreescribir fácilmente esta regla en lugar de sobreescribir **todas** las reglas de la clase de utilidad `sr-only`.

## CSS

Es importante respetar las preferencias del usuario sobre si el navegador o el sistema operativo funcionan en modo oscuro. Para eso usamos la media query `prefers-color-scheme` para que al momento de cargar la página se activen las preferencias de si el usuario usa un modo claro o modo oscuro por defecto.

Un buen modo de hacer un modo oscuro es determinar bien las custom properties de CSS y luego cambiarlas dentro de esta media query, así:

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

Adicionalmente, es necesario agregar estos mismos cambios de las custom properties a un par de clases, esto con el fin de que cuando empecemos a usar JavaScript para activar los cambios podamos usar estas clases en el tag `body` para cambiar los colores de la misma manera que hacemos con la media query.

```css
.light-theme {
  --clr-foreground: hsl(0 0% 0%);
  --clr-background: hsl(0 0% 100%);
}

.dark-theme {
  --clr-background: hsl(0 0% 0%);
  --clr-foreground: hsl(0 0% 100%);
}
```

## Javascript

- Con JavaScript es necesario cambiar el contenido del `span` al momento de hacerle clic para darle el contexto suficiente al usuario.
- También es necesario agregarle al body la clase `.light-theme` o `.dark-theme` que agregamos anteriormente para poder cambiar las custom properties del sitio.
- Adicionalmente es necesario que al momento de cargar la página, esta revise si la preferencia del modo oscuro está activida para traer los estilos del tema oscuro al sitio.

A continuación se puede ver como sería un script en JavaScript Vanilla que tenga en cuenta estas consideraciones. Todo teniendo en cuenta el markup del inicio de esta secció.

```js
const themeToggle = document.querySelector("#theme-toggle");
const themeToggleLabel = document.querySelector("#theme-toggle-text");

// Event listener para activar o desactivar dark mode

themeToggle.addEventListener("click", () => {
  document.body.classList.contains("light-theme")
    ? enableDarkMode()
    : enableLightMode();
});

// Activa el modo oscuro

function enableDarkMode() {
  document.body.classList.remove("light-theme");
  document.body.classList.add("dark-theme");
  themeToggleLabel.textContent = "Cambiar a modo claro";
}

// Desactiva el modo oscuro

function enableLightMode() {
  document.body.classList.remove("dark-theme");
  document.body.classList.add("light-theme");
  themeToggleLabel.textContent = "Cambiar a modo oscuro";
}

// Revisa si el navegador o sistema operativo tiene preferencia del modo oscuro, y de ser el caso, lo activa

function setThemePreference() {
  if (window.matchMedia("(prefers-color-scheme: dark)").matches) {
    enableDarkMode();
    return;
  }
  enableLightMode();
}

// Activa la función setThemePreference al momento de cargar el sitio

document.onload = setThemePreference();
```
