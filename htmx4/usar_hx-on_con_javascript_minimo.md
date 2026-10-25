# HTMX 4: usar `hx-on` con JavaScript mínimo

Usa `hx-on` para ejecutar JavaScript en respuesta a eventos directamente desde HTML, sin crear un listener independiente.

## Ejemplo: mostrar una notificación

```html
<button hx-post="/guardar"
        hx-on:htmx:after:request="document.querySelector('#mensaje').textContent = event.detail.successful ? 'Guardado' : 'Error'">
    Guardar
</button>

<p id="mensaje"></p>
```

Cuando termina la petición, el mensaje indica si fue exitosa.

## Ejemplo: limpiar un formulario

```html
<form hx-post="/pedidos"
      hx-on:htmx:after:request="if (event.detail.successful) this.reset()">
    <input name="numero" required>
    <button type="submit">Registrar</button>
</form>
```

El formulario se limpia si la petición termina correctamente.

## Reglas esenciales

- Usa `hx-on` para acciones pequeñas y locales.
    
- Usa `event.detail.successful` para distinguir éxito y error.
    
- Usa JavaScript externo cuando la lógica crezca o deba reutilizarse.
    
- No dupliques lógica que ya esté implementada en listeners globales.
    

**Principio:** HTML para la interacción simple; JavaScript externo para la lógica compleja.