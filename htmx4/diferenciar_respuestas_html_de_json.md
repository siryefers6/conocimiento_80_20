# HTMX: diferenciar respuestas HTML de JSON

## HTML

Devuelve HTML cuando HTMX deba actualizar directamente la interfaz. Usa `HTMLResponse` o `TemplateResponse` con Jinja2.

```python
from fastapi.responses import HTMLResponse

@app.get("/pedidos", response_class=HTMLResponse)
def pedidos():
    return "<p>Pedido #101</p>"
```

HTMX recibe el HTML y lo inserta en el elemento definido por `hx-target`.

## JSON

Devuelve JSON cuando necesites intercambiar datos estructurados con otros clientes o procesarlos mediante JavaScript.

```python
@app.get("/api/pedidos")
def obtener_pedidos():
    return {"pedidos": [101, 102, 103]}
```

HTMX no convierte automáticamente esos datos JSON en elementos HTML. Debes procesarlos explícitamente si quieres mostrarlos en la interfaz.

## Regla operativa

- **HTML:** interfaces renderizadas por FastAPI y Jinja2.
    
- **JSON:** datos estructurados para APIs y clientes programáticos.
    
- **HTMX + FastAPI:** prioriza HTML cuando el servidor controla la presentación.
    

**Principio:** devuelve el formato que necesita el consumidor de la respuesta.