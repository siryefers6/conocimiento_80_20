Condicionales: ejecuta bloques según una condición.

```html
{% if edad >= 18 %}
    Mayor de edad
{% elif edad >= 13 %}
    Adolescente
{% else %}
    Niño
{% endif %}
```

- `if`: evalúa la primera condición.
    
- `elif`: evalúa otra condición si la anterior falla.
    
- `else`: ejecuta el bloque restante.
    
- `endif`: cierra el condicional.