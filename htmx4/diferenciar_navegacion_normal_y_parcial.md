# FastAPI + HTMX 4: diferenciar navegación normal y parcial

Devuelve una página HTML completa cuando el navegador navega normalmente y un fragmento HTML cuando HTMX solicita actualizar una sección.

## 1. Detectar una petición HTMX

HTMX envía la cabecera `HX-Request: true` en sus peticiones.

```python
from fastapi import Request
from fastapi.responses import HTMLResponse

@app.get("/pedidos", response_class=HTMLResponse)
def listar_pedidos(request: Request):
    pedidos = obtener_pedidos()

    plantilla = (
        "fragments/filas_pedidos.html"
        if request.headers.get("HX-Request") == "true"
        else "pedidos.html"
    )

    return templates.TemplateResponse(
        request=request,
        name=plantilla,
        context={"pedidos": pedidos},
    )
```

## 2. Usar navegación parcial

```html
<a href="/pedidos"
   hx-get="/pedidos"
   hx-target="#contenido"
   hx-swap="innerHTML">
    Pedidos
</a>

<main id="contenido">
    Contenido actual
</main>
```

HTMX solicita `/pedidos` y reemplaza el contenido de `#contenido` con el fragmento devuelto.

## Diferencias

| Navegación normal             | Navegación parcial         |
| ----------------------------- | -------------------------- |
| No envía `HX-Request: true`.  | Envía `HX-Request: true`.  |
| Recibe la página completa.    | Recibe un fragmento HTML.  |
| El navegador carga la página. | HTMX actualiza un destino. |

## Regla esencial

- Mantén las rutas capaces de responder a ambos tipos de petición.
    
- Devuelve la plantilla completa para navegación normal.
    
- Devuelve el fragmento adecuado para peticiones HTMX.
    
- No uses `HX-Request` como mecanismo de autenticación o autorización.
    

**Principio:** una ruta, dos representaciones HTML según el tipo de navegación.