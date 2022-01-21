# Link de "ir a contenido principal"

Este link se usa en el header y está entre los primeros elementos del sitio. Este permite ir al contenido principal del sitio y es usado por usuarios que navegan por teclado para poder saltarse el menú principal e ir directamente al contenido del sitio.

![Link de ir a contenido principal de UNAD OVAS](/images/SkipLink.png)

## Markup

```html
<header>
  <a href="#">
    Logo
  </a>
  
  <!-- Link para ir al contenido principal -->
  <a class="skip-link" href="#main-content">
    Ir a contenido
  </a>

  <!-- Resto del contenido del header -->
</header>

<main id="main-content">
  <!-- Contenido del sitio -->
</main>
```

## CSS

El botón debe estar oculto de la pantalla si no está enfocado. Un modo de hacerlo es llevándolo a la parte superior de la pantalla usando la regla `transform` y luego trayéndolo a pantalla cuando tenga el estado de focus:

```css
.skip-link {
  position: fixed;
  inset: 0;
  transform: translateY(-100%);
  transition: transform 300ms ease-in;
}

.skip-link:focus {
  transform: translateY(0);
}
```