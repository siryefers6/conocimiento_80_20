Objetivo: reutilizar macros definidas en otras plantillas.

## Sintaxis

- `{% import "macros.html" as m %}`: importa el módulo completo.
    
- `{% from "macros.html" import boton %}`: importa elementos específicos.
    

## Ejemplo

```html
{% import "macros.html" as m %}
{{ m.boton("Guardar") }}

{% from "macros.html" import boton %}
{{ boton("Eliminar", "danger") }}
```

Regla: usa `import` con alias para agrupar macros; usa `from ... import` para acceder directamente a las necesarias.