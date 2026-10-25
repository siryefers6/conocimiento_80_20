# HTMX 4: gestionar los botones Atrás y Adelante

Mantén la URL sincronizada con el contenido mostrado. Usa `hx-push-url` para registrar cambios de navegación y permite que HTMX recupere el estado al volver atrás o avanzar.

## 1. Registrar cambios en el historial

```html
<a href="/pedidos?pagina=1"
   hx-get="/pedidos?pagina=1"
   hx-target="#contenido"
   hx-swap="innerHTML"
   hx-push-url="true">
    Pedidos
</a>

<main id="contenido"></main>
```

Cuando el usuario navega mediante este enlace, HTMX actualiza el contenido y añade la URL al historial.

## 2. Reconstruir la vista desde FastAPI

```python
@app.get("/pedidos", response_class=HTMLResponse)
def listar_pedidos(request: Request, pagina: int = 1):
    pedidos = obtener_pedidos(pagina)

    plantilla = (
        "fragments/pedidos.html"
        if request.headers.get("HX-Request") == "true"
        else "pedidos.html"
    )

    return templates.TemplateResponse(
        request=request,
        name=plantilla,
        context={"pedidos": pedidos, "pagina": pagina},
    )
```

Cada URL debe contener los parámetros necesarios para reconstruir su vista.

## 3. Comprender el historial en HTMX 4

- `hx-push-url="true"`: añade una entrada al historial.
    
- `hx-replace-url="true"`: reemplaza la entrada actual.
    
- Atrás y Adelante: permiten regresar a las URL registradas.
    
- HTMX 4 no usa `localStorage` para guardar automáticamente una copia de cada página; al volver atrás puede solicitar nuevamente el contenido al servidor.
    

## Reglas esenciales

- Incluye filtros y paginación en la URL cuando deban conservarse.
    
- Haz que FastAPI pueda reconstruir la vista a partir de esos parámetros.
    
- Devuelve HTML completo para navegación normal y fragmentos para navegación parcial.
    
- No dependas exclusivamente del estado temporal del navegador.
    

**Principio:** la URL representa el estado navegable; FastAPI debe poder reconstruirlo.