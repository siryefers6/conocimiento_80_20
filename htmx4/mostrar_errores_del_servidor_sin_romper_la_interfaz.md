# HTMX 4: mostrar errores del servidor sin romper la interfaz

Gestiona los errores HTTP sin reemplazar innecesariamente el contenido de la página. Usa un contenedor de notificaciones y los eventos de HTMX 4.

## 1. Crear la interfaz

```html
<div id="notificacion" role="alert" aria-live="polite"></div>

<button hx-post="/pedidos"
        hx-target="#resultado">
    Guardar pedido
</button>

<div id="resultado"></div>
```

## 2. Detectar errores HTTP

```js
document.body.addEventListener("htmx:response:error", (event) => {
    const notificacion = document.querySelector("#notificacion");
    const estado = event.detail.xhr.status;

    notificacion.textContent =
        estado === 422
            ? "Datos inválidos. Revisa el formulario."
            : "No se pudo completar la operación.";
});
```

El mensaje aparece fuera del destino principal, por lo que el contenido existente permanece intacto.

## 3. Devolver errores desde FastAPI

```python
from fastapi import HTTPException

@app.post("/pedidos")
def crear_pedido():
    raise HTTPException(
        status_code=422,
        detail="El número de pedido ya existe.",
    )
```

En producción, adapta el código HTTP al error real y evita exponer detalles internos o trazas del servidor.

## Reglas esenciales

- `htmx:response:error`: detecta respuestas HTTP de error.
    
- `textContent`: muestra mensajes como texto seguro.
    
- Mantén las notificaciones fuera de `hx-target` si no quieres que sean reemplazadas.
    
- Devuelve códigos HTTP adecuados desde FastAPI.
    

**Principio:** conserva la interfaz, comunica el error y permite corregir o reintentar la operación.