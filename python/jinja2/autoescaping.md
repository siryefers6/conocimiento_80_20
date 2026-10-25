Objetivo: evitar que datos de usuarios se interpreten como HTML ejecutable.

## Concepto

- Autoescaping: convierte caracteres especiales en entidades HTML.
    
- Protege contra la inyección de HTML y ciertos ataques XSS.
    
- En HTML, configura `select_autoescape` en Jinja2.
    

## Ejemplo

```python
from jinja2 import Environment, select_autoescape

env = Environment(
	autoescape=select_autoescape(["html", "xml"])
)
```

```
<p>{{ nombre }}</p>
```

Si `nombre` contiene `<script>alert(1)</script>`, se muestra como texto, no como código ejecutable.

Regla: mantén el autoescaping activado. Evita `|safe` con datos no confiables.