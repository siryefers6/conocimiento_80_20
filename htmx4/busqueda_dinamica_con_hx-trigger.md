# HTMX 4: búsqueda dinámica con `hx-trigger`

Ejecuta búsquedas mientras el usuario escribe, sin recargar la página. `hx-trigger` define qué evento inicia la solicitud HTTP.

## Búsqueda con retraso

```html
<input type="search"
       name="q"
       placeholder="Buscar pedido..."
       hx-get="/pedidos/buscar"
       hx-trigger="input changed delay:400ms"
       hx-target="#resultados"
       hx-swap="innerHTML">

<div id="resultados"></div>
```

- `input`: detecta cambios mientras se escribe.
    
- `changed`: evita solicitudes si el valor no cambió.
    
- `delay:400ms`: espera 400 milisegundos antes de enviar la solicitud.
    
- `hx-target`: indica dónde mostrar los resultados.
    
- `hx-swap="innerHTML"`: reemplaza el contenido del contenedor.
    

## Backend con FastAPI

```python
@app.get("/pedidos/buscar", response_class=HTMLResponse)
def buscar_pedidos(request: Request, q: str = ""):
    pedidos = buscar(q)

    return templates.TemplateResponse(
        request=request,
        name="fragments/resultados.html",
        context={"pedidos": pedidos},
    )
```

La función `buscar(q)` debe filtrar los pedidos en la base de datos y devolver los resultados.

## Variantes útiles

| `hx-trigger`                | Comportamiento                                         |
| --------------------------- | ------------------------------------------------------ |
| `input`                     | Busca al cambiar el texto.                             |
| `input changed delay:400ms` | Espera a que el usuario deje de escribir.              |
| `keyup`                     | Detecta cuando se suelta una tecla.                    |
| `search`                    | Detecta eventos de búsqueda del campo `type="search"`. |

## Regla

Usa `input changed delay:400ms` como punto de partida para búsquedas dinámicas. Mantén el filtrado en FastAPI y devuelve solo el fragmento HTML de resultados.

**Principio:** el navegador detecta cambios; el servidor filtra; HTMX actualiza la interfaz.