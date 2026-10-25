# Validación de mensajes WebSocket con Pydantic

Valida cada mensaje recibido antes de procesarlo. Comprueba el esquema, los tipos, los campos obligatorios, las restricciones de contenido y el tamaño máximo permitido.

## Define un esquema

```python
from pydantic import BaseModel, ConfigDict, Field


class Mensaje(BaseModel):
    model_config = ConfigDict(extra="forbid", strict=True)

    tipo: str = Field(min_length=1, max_length=30)
    contenido: str = Field(min_length=1, max_length=1000)
```

- `strict=True`: evita conversiones automáticas de tipos.
    
- `extra="forbid"`: rechaza campos no definidos.
    
- `Field()`: establece límites y restricciones.
    

## Valida mensajes en FastAPI

```python
from fastapi import FastAPI, WebSocket, WebSocketDisconnect
from pydantic import ValidationError

app = FastAPI()
MAX_BYTES = 4096


@app.websocket("/ws")
async def endpoint(ws: WebSocket):
    await ws.accept()

    try:
        while True:
            mensaje = await ws.receive_text()

            if len(mensaje.encode("utf-8")) > MAX_BYTES:
                await ws.close(code=1009, reason="Mensaje demasiado grande")
                return

            try:
                datos = Mensaje.model_validate_json(mensaje)
            except ValidationError:
                await ws.send_json({
                    "tipo": "error",
                    "codigo": "MENSAJE_INVALIDO",
                })
                continue

            # Ejecuta la lógica de negocio con datos validados.
            await ws.send_json({
                "tipo": "confirmacion",
                "contenido": datos.contenido,
            })

    except WebSocketDisconnect:
        pass
```

## Reglas prácticas

- Limita el tamaño antes de procesar el contenido; configura también un límite en el servidor o proxy para evitar mensajes excesivos durante la recepción.
    
- Valida el formato JSON y rechaza tipos inesperados.
    
- Define esquemas específicos por tipo de mensaje.
    
- Comprueba rangos numéricos, cadenas, identificadores y valores permitidos.
    
- Trata todo mensaje como entrada no confiable, aunque provenga de un usuario autenticado.
    
- Devuelve errores estructurados sin exponer detalles internos.
    
- Aplica límites de frecuencia y controles de autorización además de la validación estructural.
    

**Códigos de cierre útiles:** `1003` para tipos de datos no admitidos, `1008` para violaciones de políticas y `1009` para mensajes demasiado grandes.

**Referencia:** [Pydantic — Models](https://docs.pydantic.dev/latest/concepts/models/) · [OWASP — WebSocket Security Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/WebSocket_Security_Cheat_Sheet.html).