# HTMX 4: confirmar acciones con `hx-confirm`

Usa `hx-confirm` para solicitar confirmación antes de enviar una petición. Si el usuario cancela, HTMX no ejecuta la acción.

## Ejemplo: eliminar un pedido

```html
<button hx-delete="/pedidos/15"
        hx-target="#pedido-15"
        hx-swap="outerHTML"
        hx-confirm="¿Confirmas eliminar este pedido?">
    Eliminar
</button>

<div id="pedido-15">Pedido 15</div>
```

## Cómo funciona

1. El usuario pulsa **Eliminar**.
    
2. El navegador muestra el diálogo de confirmación.
    
3. Si acepta, HTMX envía la petición `DELETE`.
    
4. Si cancela, no se envía la petición.
    

## FastAPI

```python
@app.delete("/pedidos/{pedido_id}", status_code=204)
def eliminar_pedido(pedido_id: int):
    borrar_pedido(pedido_id)
```

El servidor debe verificar que el pedido existe y que el usuario tiene permiso para eliminarlo.

## Reglas esenciales

- Usa `hx-confirm` para eliminaciones y acciones irreversibles.
    
- El mensaje puede ser cualquier texto.
    
- La confirmación mejora la experiencia, pero no sustituye la autorización ni la validación del servidor.
    

**Principio:** confirma en el navegador; valida y autoriza siempre en FastAPI.