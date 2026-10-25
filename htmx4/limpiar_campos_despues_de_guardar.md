# HTMX 4: limpiar campos después de guardar

Restablece el formulario después de guardar correctamente. Usa `hx-on:after-request` para detectar el fin de la petición y limpia los campos solo si la respuesta fue exitosa.

## Ejemplo

```html
<form
    hx-post="/pedidos"
    hx-target="#resultado"
    hx-on:after:request="if (event.detail.success) this.reset()"
>
    <input name="numero" type="number" required>
    <input name="cliente" required>

    <button type="submit">Guardar</button>
</form>

<div id="resultado"></div>
```

## Flujo

1. El usuario completa el formulario.
    
2. HTMX envía los datos a FastAPI.
    
3. El servidor responde con el resultado.
    
4. Si la petición fue exitosa, el formulario se limpia.
    
5. Si hay un error, los campos conservan sus valores.
    

## Reglas operativas

- Usa `this.reset()` para restaurar los campos a sus valores iniciales.
    
- Comprueba `event.detail.success` para evitar limpiar el formulario ante errores HTTP.
    
- Devuelve un código `2xx` cuando el guardado sea correcto.
    
- Mantén los errores de validación dentro del formulario.
    

**Principio:** limpia solo después de confirmar el éxito del servidor.