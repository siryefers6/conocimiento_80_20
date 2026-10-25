# HTMX 4: ejecutar acciones después de actualizar el HTML

Usa el evento `htmx:after:swap` para ejecutar JavaScript después de que HTMX inserte la respuesta en el DOM.

## Ejemplo: enfocar un campo actualizado

**HTML**

```html
<button hx-get="/formulario"
        hx-target="#contenedor"
        hx-swap="innerHTML">
    Cargar formulario
</button>

<div id="contenedor"></div>
```

**JavaScript**

```js
document.body.addEventListener("htmx:after:swap", (event) => {
    if (event.detail.target.id === "contenedor") {
        document.querySelector("#nombre")?.focus();
    }
});
```

**Fragmento devuelto por FastAPI**

```
<input id="nombre" name="nombre" placeholder="Nombre">
```

Al terminar el intercambio, el campo existe en el DOM y puede recibir el foco.

## Eventos relacionados

| Evento               | Momento                           |
| -------------------- | --------------------------------- |
| `htmx:before:swap`   | Antes de insertar la respuesta.   |
| `htmx:after:swap`    | Después de insertar la respuesta. |
| `htmx:after:request` | Cuando termina la petición HTTP.  |

## Regla esencial

- Usa `htmx:after:swap` para enfocar campos, inicializar componentes o ajustar la interfaz después de insertar HTML.
    
- Usa `htmx:after:request` para acciones relacionadas con el fin de la petición, independientemente de si hubo intercambio de HTML.
    
- Comprueba `event.detail.target` para limitar la acción al componente esperado.
    

**Principio:** espera a que el HTML exista antes de manipularlo.