Devuelve HTML desde un endpoint usando `HTMLResponse`. HTMX puede insertar esa respuesta directamente en la interfaz.

## Ejemplo mínimo funcional

`main.py`

```python
from fastapi import FastAPI
from fastapi.responses import HTMLResponse

app = FastAPI()

@app.get("/pedidos", response_class=HTMLResponse)
def obtener_pedidos():
    return """
    <ul>
        <li>Pedido #101</li>
        <li>Pedido #102</li>
    </ul>
    """
```

Ejecuta:

```
uv run uvicorn main:app --reload
```

Abre `http://127.0.0.1:8000/pedidos` para ver el HTML generado.

## Consumir el endpoint con HTMX 4

```html
<button hx-get="/pedidos" hx-target="#resultado">
    Consultar pedidos
</button>

<div id="resultado"></div>
```

Al pulsar el botón, HTMX solicita el endpoint e inserta el HTML recibido dentro de `#resultado`.

## Reglas operativas

- Importa `HTMLResponse` desde `fastapi.responses`.
    
- Usa `response_class=HTMLResponse` para declarar que el endpoint responde con HTML.
    
- Devuelve fragmentos HTML para actualizar componentes con HTMX.
    
- Devuelve páginas completas cuando la petición requiera una página nueva.
    

Principio: FastAPI genera la respuesta; HTMX decide dónde insertarla.