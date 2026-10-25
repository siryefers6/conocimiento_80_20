Objetivo: reutilizar fragmentos HTML sin duplicar código.

## Sintaxis

```
{% include "navbar.html" %}
```

- `include`: inserta el contenido renderizado de otra plantilla.
    
- Comparte el contexto actual, incluidas sus variables.
    
- Úsalo para navegación, alertas y fragmentos de interfaz.
    

## Ejemplo

`index.html`

```html
{% include "navbar.html" %}

<h1>{{ titulo }}</h1>
```

Regla: usa `include` para insertar fragmentos HTML; usa `macro` para componentes parametrizables.