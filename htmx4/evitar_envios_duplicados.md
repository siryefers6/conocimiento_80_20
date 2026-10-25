# HTMX 4: evitar envíos duplicados

Deshabilita el botón mientras se procesa el formulario. Evita clics repetidos y reduce las solicitudes accidentales.

## Ejemplo

```html
<form hx-post="/pedidos" hx-target="#resultado">
    <input name="numero" required>

    <button type="submit" hx-disable>
        Guardar pedido
    </button>
</form>

<div id="resultado"></div>
```

En HTMX 4, `hx-disable` deshabilita el elemento durante la petición. No lo confundas con `hx-ignore`, que evita que HTMX procese un elemento.

## Protección en FastAPI

Deshabilitar el botón no garantiza que el servidor no reciba solicitudes duplicadas. Para operaciones importantes, implementa una restricción de unicidad en la base de datos o una clave de idempotencia.

## Reglas operativas

- Deshabilita el botón durante el envío.
    
- Valida los datos en FastAPI.
    
- Protege las operaciones críticas en la base de datos.
    
- No confíes únicamente en la interfaz para garantizar que un registro se cree una sola vez.
    

**Principio:** la interfaz reduce duplicados; el servidor garantiza la integridad.