# HTMX 4: eventos `htmx:before:request` y `htmx:after:request`

Controla el comportamiento de la interfaz antes y después de una petición HTTP. Escucha los eventos del ciclo de petición con JavaScript.

## `htmx:before:request`

Se dispara antes de enviar la petición. Úsalo para mostrar indicadores de carga o preparar la interfaz.

## `htmx:after:request`

Se dispara después de que termina la petición, incluso si la respuesta indica un error HTTP. Úsalo para ocultar indicadores o ejecutar tareas posteriores.

## Ejemplo funcional

**HTML**

```html
<button id="guardar"
        hx-post="/guardar"
        hx-target="#resultado">
    Guardar
</button>

<p id="estado"></p>
<div id="resultado"></div>
```

**JavaScript**

```javascript
document.body.addEventListener("htmx:before:request", (event) => {
    if (event.detail.elt.id === "guardar") {
        document.querySelector("#estado").textContent = "Guardando...";
    }
});

document.body.addEventListener("htmx:after:request", (event) => {
    if (event.detail.elt.id === "guardar") {
        document.querySelector("#estado").textContent = "Petición finalizada";
    }
});
```

## Diferencia esencial

| Evento                | Momento                 | Uso                                 |
| --------------------- | ----------------------- | ----------------------------------- |
| `htmx:before:request` | Antes de enviar         | Mostrar carga, preparar controles.  |
| `htmx:after:request`  | Al terminar la petición | Ocultar carga, limpiar indicadores. |

**Principio:** utiliza `before:request` para iniciar el estado de carga y `after:request` para finalizarlo. No confundas el fin de la petición con el éxito de la operación: comprueba `event.detail.successful` si necesitas distinguir respuestas exitosas de fallidas.