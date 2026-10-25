Objetivo: construir interfaces consistentes sin duplicar HTML.

## Ejemplo

Define macros para cada componente.

```html
{% macro tarjeta(titulo, contenido) %}
<div class="card">
    <h2>{{ titulo }}</h2>
    <p>{{ contenido }}</p>
</div>
{% endmacro %}

{% macro mensaje(texto, tipo="info") %}
<div class="alert alert-{{ tipo }}">
    {{ texto }}
</div>
{% endmacro %}
```

Uso:

```html
{% from "componentes.html" import tarjeta, mensaje %}

{{ tarjeta("Pedidos", "Total: 25") }}
{{ mensaje("Guardado correctamente", "success") }}
```

## Regla

- Macro: tarjetas, botones y mensajes parametrizables.
- Include: tablas o fragmentos HTML independientes.
- Consistencia: modifica el componente una vez; actualiza todos sus usos.