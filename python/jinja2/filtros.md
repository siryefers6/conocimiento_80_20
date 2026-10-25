Filtros (`|`) — Transforman valores directamente en Jinja2.

```html
{{ nombre | upper }}             {# MAYÚSCULAS #}
{{ precio | round(2) }}          {# Redondea a 2 decimales #}
{{ fecha | default("Sin fecha") }} {# Valor por defecto si no existe #}
{{ texto | length }}             {# Longitud de texto o colección #}
```

Filtros 80/20 que debes dominar:

- `upper` / `lower`: cambia mayúsculas y minúsculas.
- `default("...")`: establece un valor predeterminado.
- `length`: cuenta elementos o caracteres.
- `round(2)`: redondea números.
- `trim`: elimina espacios al inicio y al final.
- `replace("a", "b")`: reemplaza texto.
- `join(", ")`: une elementos de una lista.

Regla: aplica filtros con `|` sin modificar la variable original.