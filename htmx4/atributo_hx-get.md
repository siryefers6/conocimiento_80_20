Realiza una petición HTTP `GET` desde un elemento HTML. Obtén datos del servidor sin recargar toda la página.

## Sintaxis

```html
<button hx-get="/pedidos" hx-target="#resultado">
    Consultar pedidos
</button>

<div id="resultado"></div>
```

- `hx-get="/pedidos"`: solicita la URL `/pedidos`.
    
- `hx-target="#resultado"`: indica dónde insertar la respuesta.
    
- Por defecto, HTMX intercambia el contenido interno del elemento objetivo.
    

## Ejemplo mínimo con FastAPI

Backend — `main.py`

```python
from fastapi import FastAPI
from fastapi.responses import HTMLResponse

app = FastAPI()

@app.get("/pedidos", response_class=HTMLResponse)
def obtener_pedidos():
    return "<p>Pedido #101</p><p>Pedido #102</p>"
```

Flujo: clic → petición `GET` → FastAPI devuelve HTML → HTMX actualiza `#resultado`.

## Reglas operativas

- Usa `hx-get` para consultar información.
    
- Devuelve HTML si la respuesta se insertará directamente en la interfaz.
    
- Combínalo con `hx-target` para controlar dónde aparece la respuesta.
    
- No uses `GET` para modificar datos.
    

HTMX 4.0.0: verifica la documentación de la versión instalada antes de reutilizar ejemplos de versiones anteriores.