# HTMX: atributo `hx-swap`

Define cómo insertar la respuesta HTML dentro del elemento seleccionado por `hx-target`. Controla si reemplazas contenido, agregas elementos o sustituyes el propio objetivo.

## Modos esenciales

|Valor|Comportamiento|
|---|---|
|`innerHTML`|Reemplaza el contenido interno.|
|`outerHTML`|Reemplaza el elemento completo.|
|`beforebegin`|Inserta HTML antes del elemento.|
|`afterbegin`|Inserta HTML al principio del contenido.|
|`beforeend`|Inserta HTML al final del contenido.|
|`afterend`|Inserta HTML después del elemento.|
|`delete`|Elimina el elemento objetivo.|
|`none`|No inserta la respuesta en el DOM.|

## Ejemplo con FastAPI

Backend — `main.py`

```python
from fastapi import FastAPI
from fastapi.responses import HTMLResponse

app = FastAPI()

@app.get("/pedidos", response_class=HTMLResponse)
def pedidos():
    return "<li>Pedido #103</li>"
```

HTML

```html
<button
    hx-get="/pedidos"
    hx-target="#lista"
    hx-swap="beforeend">
    Agregar pedido
</button>

<ul id="lista">
    <li>Pedido #101</li>
    <li>Pedido #102</li>
</ul>
```

Al pulsar el botón, HTMX agrega el nuevo pedido al final de la lista sin eliminar los anteriores.

## Regla operativa

- Usa `innerHTML` para actualizar una sección.
    
- Usa `outerHTML` para reemplazar un componente completo.
    
- Usa `beforeend` para agregar elementos al final.
    
- Usa `afterbegin` para agregarlos al principio.
    

Principio: `hx-target` selecciona dónde actuar; `hx-swap` define cómo modificar el DOM. Los ejemplos siguen la sintaxis habitual de HTMX 4.0.0.