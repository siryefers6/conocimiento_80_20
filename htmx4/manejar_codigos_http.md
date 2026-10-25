# FastAPI y HTMX 4: manejar códigos HTTP

Interpreta el código HTTP para determinar si una petición tuvo éxito, no necesita contenido o produjo un error.

## Códigos esenciales

| Código                      | Significado         | Uso                                        |
| --------------------------- | ------------------- | ------------------------------------------ |
| `200 OK`                    | Operación correcta  | Devuelve HTML para actualizar la interfaz. |
| `204 No Content`            | Éxito sin contenido | Ejecuta una operación sin devolver HTML.   |
| `422 Unprocessable Content` | Datos inválidos     | Devuelve errores de validación.            |
| `500 Internal Server Error` | Error interno       | Indica un fallo inesperado del servidor.   |

## Ejemplo con FastAPI

```python
from fastapi import FastAPI, Form, HTTPException
from fastapi.responses import HTMLResponse, Response

app = FastAPI()


@app.post("/pedidos", response_class=HTMLResponse)
def crear_pedido(numero: str = Form()):
    if not numero.isdigit():
        raise HTTPException(422, "Número inválido")

    return f"<p>Pedido #{numero} registrado.</p>"


@app.delete("/pedidos/{numero}")
def eliminar_pedido(numero: int):
    # Ejecuta aquí la eliminación real.
    return Response(status_code=204)
```

## Reglas operativas

- Devuelve `200` cuando la operación sea correcta y necesites enviar HTML.
    
- Devuelve `204` cuando la operación termine correctamente sin contenido.
    
- Devuelve `422` cuando los datos no superen la validación.
    
- Devuelve `500` para errores inesperados; registra el detalle en el servidor y no expongas información interna.
    

**HTMX 4:** distingue las respuestas exitosas de las respuestas de error. Usa el evento `htmx:response:error` para gestionar respuestas HTTP de error cuando necesites personalizar la interfaz.

**Principio:** el código HTTP comunica el resultado; el cuerpo de la respuesta aporta el contenido o el detalle del error.