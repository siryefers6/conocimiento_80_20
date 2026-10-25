# HTMX 4: gestionar notificaciones de éxito y error

Centraliza las notificaciones mediante eventos de HTMX 4. Muestra el resultado de cada petición sin recargar la página.

## 1. Crear el contenedor

```html
<div id="notificacion" role="status" aria-live="polite"></div>

<button hx-post="/pedidos"
        hx-target="#resultado">
    Guardar pedido
</button>

<div id="resultado"></div>
```

## 2. Escuchar los eventos

```js
const notificacion = document.querySelector("#notificacion");

document.body.addEventListener("htmx:after:request", (event) => {
    if (event.detail.successful) {
        notificacion.textContent = "Operación realizada correctamente";
    } else {
        notificacion.textContent = "No se pudo completar la operación";
    }
});
```

`event.detail.successful` permite distinguir una respuesta HTTP exitosa de una fallida.

## 3. Mostrar mensajes según el código HTTP

```js
document.body.addEventListener("htmx:response:error", (event) => {
    const estado = event.detail.xhr.status;

    notificacion.textContent =
        estado === 422
            ? "Revisa los datos ingresados"
            : "Error del servidor";
});
```

Usa `htmx:response:error` para tratar específicamente las respuestas HTTP de error.

## Reglas esenciales

- `htmx:after:request`: gestionar el resultado general de la petición.
    
- `htmx:response:error`: identificar respuestas HTTP fallidas.
    
- `textContent`: insertar mensajes como texto, sin interpretar HTML.
    
- FastAPI debe devolver códigos HTTP apropiados para cada resultado.
    

**Principio:** centraliza los mensajes en un único contenedor y diferencia éxito, validación fallida y error del servidor.