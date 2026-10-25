Objetivo: controlar cuándo Jinja2 interpreta contenido como HTML.

## Concepto

- `|safe`: desactiva el escape automático para ese valor.
    
- El navegador interpreta el contenido como HTML.
    
- Con datos no confiables, puede permitir ataques XSS.
    

## Ejemplo

```
{{ contenido | safe }}
```

Si `contenido` incluye `<script>...</script>`, el navegador podría ejecutarlo.

Regla: no uses `|safe` con datos de usuarios. Si necesitas permitir HTML, sanitízalo con una biblioteca confiable antes de renderizarlo.