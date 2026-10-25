`{% ... %}` — Ejecuta instrucciones de plantilla: condiciones, bucles y asignaciones.

```html
{% if edad >= 18 %}
    Mayor de edad
{% endif %}

{% for usuario in usuarios %}
    <p>{{ usuario }}</p>
{% endfor %}

{% set nombre = "Abathur" %}
```
