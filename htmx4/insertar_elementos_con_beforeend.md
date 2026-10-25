# HTMX 4: insertar elementos con `beforeend`

Usa `hx-swap="beforeend"` para agregar la respuesta del servidor **al final del contenido de un elemento**, sin reemplazar los elementos existentes.

## Ejemplo: agregar pedidos a una lista

**HTML**

```html
<button hx-post="/pedidos"
        hx-target="#lista-pedidos"
        hx-swap="beforeend">
    Agregar pedido
</button>

<ul id="lista-pedidos">
    <li>Pedido 100</li>
</ul>
```

**FastAPI**

```python
@app.post("/pedidos", response_class=HTMLResponse)
def crear_pedido(request: Request):
    pedido = guardar_pedido()

    return templates.TemplateResponse(
        request=request,
        name="fragments/pedido.html",
        context={"pedido": pedido},
    )
```

`**fragments/pedido.html**`

```
<li>Pedido {{ pedido.numero }}</li>
```

Después de la respuesta, la lista conserva el pedido existente y agrega el nuevo al final.

## Diferencia entre los swaps

| `hx-swap`    | Resultado                                   |
| ------------ | ------------------------------------------- |
| `innerHTML`  | Reemplaza todo el contenido interior.       |
| `beforeend`  | Agrega al final del contenido interior.     |
| `afterbegin` | Agrega al principio del contenido interior. |
| `outerHTML`  | Reemplaza el elemento completo.             |

## Regla

Usa `beforeend` para listas, tablas o contenedores que deban acumular elementos sin perder los existentes.

**Importante:** el servidor debe devolver el fragmento del nuevo elemento, no la lista completa. Para una tabla, devuelve una fila `<tr>` dentro de un `<tbody>`.