`{% for %}` y `{% endfor %}` — Recorre una colección y genera contenido por cada elemento.

```html
{% for usuario in usuarios %}
    <p>{{ usuario }}</p>
{% endfor %}
```

- `for`: inicia el bucle.
    
- `usuario`: variable que recibe cada elemento.
    
- `usuarios`: colección que se recorre.
    
- `endfor`: finaliza el bucle.
    

Índice y último elemento:

```html
{% for usuario in usuarios %}
    {{ loop.index }} - {{ usuario }}
    {% if loop.last %} (Último){% endif %}
{% endfor %}
```

- `loop.index`: índice desde 1.
    
- `loop.first`: indica el primer elemento.
    
- `loop.last`: indica el último elemento.

`{% else %}` — Dentro de un bucle `for`, muestra contenido si la colección está vacía.

```
{% for usuario in usuarios %}
    <p>{{ usuario }}</p>
{% else %}
    <p>No hay usuarios registrados.</p>
{% endfor %}
```

Regla: `else` se ejecuta cuando el bucle no tiene elementos.