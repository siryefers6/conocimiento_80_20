# FastAPI: filtrar datos desde el servidor

Filtra los datos en FastAPI antes de enviarlos a Jinja2. El navegador solicita los criterios; el servidor consulta y devuelve únicamente los resultados necesarios.

## 1. Enviar el filtro desde HTMX

```html
<input type="search"
       name="q"
       placeholder="Buscar pedido..."
       hx-get="/pedidos"
       hx-trigger="input changed delay:400ms"
       hx-target="#resultados"
       hx-swap="innerHTML">

<div id="resultados"></div>
```

## 2. Filtrar en FastAPI

```python
@app.get("/pedidos", response_class=HTMLResponse)
def listar_pedidos(request: Request, q: str = ""):
    pedidos = obtener_pedidos_filtrados(q)

    return templates.TemplateResponse(
        request=request,
        name="fragments/pedidos.html",
        context={"pedidos": pedidos},
    )
```

`obtener_pedidos_filtrados(q)` representa la función que consulta la base de datos aplicando el filtro.

## 3. Aplicar el filtro en SQLModel

```python
def obtener_pedidos_filtrados(q: str) -> list[Pedido]:
    with Session(engine) as session:
        consulta = select(Pedido)

        if q.strip():
            consulta = consulta.where(
                Pedido.numero.contains(q.strip())
            )

        return list(session.exec(consulta).all())
```

Requiere importar `select` desde `sqlmodel` y disponer de `engine` y del modelo `Pedido`.

## Reglas esenciales

- Recibe los filtros mediante parámetros de consulta, como `q`.
    
- Aplica `WHERE` en la base de datos; evita cargar todos los registros para filtrarlos en Python.
    
- Devuelve solo el fragmento HTML necesario.
    
- Valida los criterios y limita los resultados cuando el volumen de datos sea grande.
    

**Principio:** HTMX envía los criterios; FastAPI coordina la consulta; SQLModel filtra; Jinja2 presenta los resultados.