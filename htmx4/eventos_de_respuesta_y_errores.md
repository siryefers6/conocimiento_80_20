# HTMX 4: eventos de respuesta y errores

Detecta respuestas HTTP exitosas o fallidas. Usa los eventos para informar al usuario sin recargar la página.

## Eventos principales

| Evento                | Función                                              |
| --------------------- | ---------------------------------------------------- |
| `htmx:before:swap`    | Se dispara antes de insertar la respuesta en el DOM. |
| `htmx:after:swap`     | Se dispara después de insertar la respuesta.         |
| `htmx:response:error` | Se dispara cuando la respuesta HTTP indica un error. |
| `htmx:after:request`  | Se dispara cuando termina la petición.               |

## Ejemplo: mostrar errores

**HTML**

```html
<button hx-post="/guardar"
        hx-target="#resultado">
    Guardar
</button>

<p id="mensaje"></p>
<div id="resultado"></div>
```

**JavaScript**

```javascript
document.body.addEventListener("htmx:response:error", (event) => {
    document.querySelector("#mensaje").textContent =
        `Error HTTP: ${event.detail.xhr.status}`;
});
```

Este ejemplo muestra el código HTTP cuando HTMX recibe una respuesta de error. El evento `htmx:response:error` permite gestionar estos casos centralmente.

## Distinguir éxito y error

```javascript
document.body.addEventListener("htmx:after:request", (event) => {
    const mensaje = document.querySelector("#mensaje");

    mensaje.textContent = event.detail.successful
        ? "Operación exitosa"
        : "La operación falló";
});
```

## Regla esencial

- Usa `htmx:response:error` para gestionar errores de respuesta HTTP.
    
- Usa `htmx:after:request` para finalizar indicadores de carga y evaluar el resultado.
    
- Usa `htmx:before:swap` para intervenir antes de insertar contenido.
    
- Devuelve códigos HTTP adecuados desde FastAPI: `200` para éxito, `422` para datos inválidos y `500` para errores internos.
    

**Principio:** el servidor comunica el resultado mediante HTTP; HTMX emite eventos; JavaScript actualiza los mensajes de estado.