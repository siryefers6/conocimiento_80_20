Define el elemento HTML donde HTMX mostrará la respuesta del servidor. Actualiza una sección específica sin recargar toda la página.

## Sintaxis

```html
<button hx-get="/pedidos" hx-target="#lista">
    Consultar pedidos
</button>

<div id="lista"></div>
```

- `hx-get`: solicita los pedidos.
    
- `hx-target="#lista"`: selecciona el elemento que recibirá la respuesta.
    
- `#lista`: identifica el elemento mediante su `id`.
    

## Ejemplo con FastAPI

Backend — `main.py`

```python
from fastapi import FastAPI
from fastapi.responses import HTMLResponse

app = FastAPI()

@app.get("/pedidos", response_class=HTMLResponse)
def pedidos():
    return "<p>Pedido #101</p><p>Pedido #102</p>"
```

Al pulsar el botón, HTMX inserta los pedidos devueltos dentro de `<div id="lista">`.

## Selectores frecuentes

| Selector  | Elemento seleccionado                          |
| --------- | ---------------------------------------------- |
| `#lista`  | Elemento con `id="lista"`.                     |
| `.pedido` | Elemento con clase `pedido`.                   |
| `this`    | Elemento que contiene el atributo `hx-target`. |

## Regla operativa

Usa `hx-target` para separar el elemento que inicia la petición del elemento que recibe la respuesta. Si no lo defines, HTMX utiliza el comportamiento predeterminado de su mecanismo de intercambio.