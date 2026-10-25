# HTMX 4: reemplazar componentes con `outerHTML`

Usa `hx-swap="outerHTML"` para reemplazar el elemento destino completo, incluida su etiqueta HTML.

## Ejemplo: actualizar una tarjeta

**HTML inicial**

```html
<div id="pedido-1">
    <p>Estado: pendiente</p>

    <button hx-post="/pedidos/1/confirmar"
            hx-target="#pedido-1"
            hx-swap="outerHTML">
        Confirmar
    </button>
</div>
```

**FastAPI**

```python
@app.post("/pedidos/{pedido_id}/confirmar", response_class=HTMLResponse)
def confirmar_pedido(request: Request, pedido_id: int):
    pedido = confirmar(pedido_id)

    return templates.TemplateResponse(
        request=request,
        name="fragments/tarjeta_pedido.html",
        context={"pedido": pedido},
    )
```

`**fragments/tarjeta_pedido.html**`

```html
<div id="pedido-{{ pedido.id }}">
    <p>Estado: {{ pedido.estado }}</p>
</div>
```

HTMX reemplaza la tarjeta antigua por la respuesta del servidor. El estado actualizado aparece sin recargar la página.

## Reglas esenciales

- `hx-target`: identifica el componente que se reemplazará.
    
- `hx-swap="outerHTML"`: sustituye el componente completo.
    
- La respuesta debe contener el elemento HTML completo, con su etiqueta exterior.
    
- Conserva el mismo `id` si el componente debe seguir siendo seleccionable.
    

**Principio:** devuelve el componente actualizado, no la página completa.