# HTMX 4: usar `hx-target` y `hx-swap`

Controla dónde insertar la respuesta y cómo modificar el HTML existente.

## `hx-target`: elegir el destino

Indica qué elemento recibirá la respuesta del servidor.

```html
<button hx-get="/pedidos"
        hx-target="#lista">
    Actualizar
</button>

<div id="lista"></div>
```

- `#lista`: selecciona el elemento por su ID.
    
- `.pedido`: selecciona elementos por clase.
    
- `this`: selecciona el propio elemento.
    

## `hx-swap`: elegir cómo insertar la respuesta

| Valor         | Comportamiento                   |
| ------------- | -------------------------------- |
| `innerHTML`   | Reemplaza el contenido interior. |
| `outerHTML`   | Reemplaza el elemento completo.  |
| `beforebegin` | Inserta antes del elemento.      |
| `afterend`    | Inserta después del elemento.    |
| `delete`      | Elimina el elemento destino.     |
| `none`        | No inserta la respuesta.         |

## Ejemplo: actualizar una tabla

**HTML**

```html
<button hx-get="/pedidos"
        hx-target="#filas"
        hx-swap="innerHTML">
    Actualizar
</button>

<table>
    <tbody id="filas">
        {% include "fragments/filas.html" %}
    </tbody>
</table>
```

**FastAPI**

```python
@app.get("/pedidos", response_class=HTMLResponse)
def listar_pedidos(request: Request):
    return templates.TemplateResponse(
        request=request,
        name="fragments/filas.html",
        context={"pedidos": obtener_pedidos()},
    )
```

El servidor devuelve únicamente las filas. HTMX reemplaza el contenido de `#filas`, sin reconstruir la tabla completa.

## Regla de selección

- Actualizar el contenido de un contenedor: `innerHTML`.
    
- Reemplazar una fila o tarjeta completa: `outerHTML`.
    
- Insertar un elemento junto a otro: `beforebegin` o `afterend`.
    

**Principio:** `hx-target` decide dónde. `hx-swap` decide cómo. Devuelve desde FastAPI el fragmento HTML que corresponde al destino.