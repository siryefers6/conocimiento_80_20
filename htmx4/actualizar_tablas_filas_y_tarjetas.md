# HTMX 4: actualizar tablas, filas y tarjetas

Actualiza componentes específicos del DOM sin recargar la página. Devuelve fragmentos HTML desde FastAPI y selecciona el destino con `hx-target`.

## Ejemplo: actualizar una tabla

**FastAPI —** `**main.py**`

```python
from fastapi import FastAPI, Request
from fastapi.templating import Jinja2Templates

app = FastAPI()
templates = Jinja2Templates(directory="templates")

pedidos = [
    {"id": 1, "estado": "Pendiente"},
    {"id": 2, "estado": "Preparado"},
]

@app.get("/pedidos")
def listar_pedidos(request: Request):
    return templates.TemplateResponse(
        request=request,
        name="fragments/filas.html",
        context={"pedidos": pedidos},
    )
```

`**templates/fragments/filas.html**`

```html
{% for pedido in pedidos %}
<tr>
    <td>{{ pedido.id }}</td>
    <td>{{ pedido.estado }}</td>
</tr>
{% endfor %}
```

**HTML**

```html
<button hx-get="/pedidos"
        hx-target="#filas"
        hx-swap="innerHTML">
    Actualizar
</button>

<table>
    <tbody id="filas">
        <tr><td>Datos anteriores</td></tr>
    </tbody>
</table>
```

## Actualizar una fila o tarjeta

Usa `hx-target` para seleccionar el componente exacto y `hx-swap="outerHTML"` para reemplazarlo por su nueva versión.

```html
<div id="pedido-1">
    <span>Pendiente</span>
    <button hx-get="/pedidos/1"
            hx-target="#pedido-1"
            hx-swap="outerHTML">
        Actualizar estado
    </button>
</div>
```

El endpoint `/pedidos/1` debe devolver la tarjeta completa con el mismo identificador.

## Reglas operativas

- **Tabla completa:** reemplaza las filas dentro de `<tbody>`.
    
- **Fila:** reemplaza el `<tr>` completo.
    
- **Tarjeta:** reemplaza el contenedor del componente.
    
- Devuelve HTML compatible con el elemento objetivo.
    
- Mantén los identificadores estables y únicos.
    

**Principio:** actualiza la unidad mínima necesaria. Menos HTML transferido, menos DOM alterado y menor complejidad.