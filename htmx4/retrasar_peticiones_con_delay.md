# HTMX 4: retrasar peticiones con `delay`

Usa `delay` en `hx-trigger` para esperar antes de enviar una petición. Si ocurre nuevamente el mismo evento durante la espera, el temporizador se reinicia.

## Ejemplo: búsqueda dinámica

```html
<input type="search"
       name="q"
       hx-get="/pedidos/buscar"
       hx-trigger="input changed delay:400ms"
       hx-target="#resultados"
       hx-swap="innerHTML">

<div id="resultados"></div>
```

**Comportamiento:**

1. El usuario escribe.
    
2. HTMX espera 400 milisegundos desde el último cambio.
    
3. Si el usuario sigue escribiendo, reinicia la espera.
    
4. Si deja de escribir, envía la petición al servidor.
    

## Sintaxis

```
hx-trigger="input delay:500ms"
```

- `input`: evento que inicia la espera.
    
- `delay:500ms`: espera medio segundo.
    
- `changed`: evita activar el disparador si el valor no cambió.
    

## Cuándo usarlo

- Búsquedas dinámicas: `delay:300ms` o `delay:400ms`.
    
- Validación de campos: `delay:500ms`.
    
- Evita solicitudes innecesarias mientras el usuario escribe.
    

**Principio:** ajusta el retraso al propósito. `delay` reduce peticiones durante la escritura, pero no reemplaza la validación del servidor.