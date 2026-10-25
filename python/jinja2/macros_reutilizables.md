Objetivo: encapsular HTML reutilizable en funciones de plantilla.

## Definición

- `{% macro %}`: define un componente reutilizable.
    
- `{% endmacro %}`: finaliza la definición.
    
- Acepta parámetros para personalizar el HTML generado.
    

## Ejemplo

`macros.html`

```html
{% macro boton(texto, color="primary") %}
<button class="{{ color }}">
    {{ texto }}
</button>
{% endmacro %}
```

`index.html`

```html
{% from "macros.html" import boton %}

{{ boton("Guardar") }}
{{ boton("Eliminar", "danger") }}
```

Resultado: dos botones generados sin duplicar HTML.

Regla: encapsula los fragmentos repetidos; reutiliza la macro donde sea necesaria.