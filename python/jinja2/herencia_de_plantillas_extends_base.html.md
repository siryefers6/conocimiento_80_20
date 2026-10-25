`{% extends "base.html" %}` — Hereda el diseño de una plantilla base y evita repetir HTML.

`base.html`

```html
<!DOCTYPE html>
<html>
<body>
    {% block contenido %}{% endblock %}
</body>
</html>
```

`index.html`

```html
{% extends "base.html" %}

{% block contenido %}
    <h1>Inicio</h1>
{% endblock %}
```

- `extends`: hereda la plantilla base.
    
- `block`: define una sección reemplazable.
    
- `endblock`: cierra la sección.
    

Regla: define la estructura común en `base.html` y el contenido específico en cada página.