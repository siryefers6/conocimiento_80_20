# HTMX 4: reemplazar contenido con la respuesta del servidor

Usa `hx-target` para seleccionar el elemento y `hx-swap="innerHTML"` para reemplazar su contenido interno con el HTML recibido.

## Ejemplo mínimo

HTML

```html
<button hx-get="/pedidos" hx-target="#lista" hx-swap="innerHTML">
    Actualizar pedidos
</button>

<div id="lista">
    <p>Contenido anterior</p>
</div>
```

FastAPI — `main.py`

```python
from fastapi import FastAPI
from fastapi.responses import HTMLResponse

app = FastAPI()

@app.get("/pedidos", response_class=HTMLResponse)
def pedidos():
    return "<p>Pedido #101</p><p>Pedido #102</p>"
```

## Flujo de ejecución

1. El usuario pulsa el botón.
    
2. HTMX solicita `GET /pedidos`.
    
3. FastAPI devuelve HTML.
    
4. HTMX elimina el contenido anterior de `#lista` e inserta la respuesta.
    

## Regla operativa

- `hx-target`: selecciona dónde actualizar.
    
- `hx-swap="innerHTML"`: reemplaza el contenido interno.
    
- `hx-swap="outerHTML"`: reemplaza el elemento completo.
    

Principio: el servidor determina el HTML; HTMX actualiza el DOM sin recargar la página.