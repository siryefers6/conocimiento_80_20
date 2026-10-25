# HTMX 4: deshabilitar controles con `hx-disable`

Usa `hx-disable` para impedir interacciones con un elemento mientras HTMX procesa una petición. Evita envíos repetidos y acciones duplicadas.

## Ejemplo: evitar envíos duplicados

```html
<form hx-post="/pedidos"
      hx-target="#resultado">
    <input name="numero" required>

    <button type="submit" hx-disable>
        Registrar pedido
    </button>
</form>

<div id="resultado"></div>
```

Durante la petición, HTMX deshabilita temporalmente el botón y lo habilita nuevamente al terminar.

## Cuándo utilizarlo

- Formularios de registro.
    
- Operaciones de guardado.
    
- Acciones que no deben iniciarse varias veces seguidas.
    

## Regla esencial

`hx-disable` mejora la interacción, pero no garantiza que una operación se ejecute una sola vez. Para operaciones críticas, implementa también validaciones o mecanismos de idempotencia en FastAPI.

**Principio:** previene duplicaciones en la interfaz; protege también las operaciones en el servidor.