# WebSocket vs. Server-Sent Events (SSE)

Elige el protocolo según la dirección de comunicación requerida.

|Característica|WebSocket|SSE|
|---|---|---|
|Dirección|Bidireccional|Servidor → cliente|
|Transporte|Conexión persistente|HTTP con `text/event-stream`|
|Formato|Texto o binario|Texto|
|Reconexión|Implementación propia en el cliente|Automática en `EventSource`|
|Uso típico|Chat, colaboración, juegos|Notificaciones, estados, progreso|

## SSE con FastAPI

```python
import asyncio
from fastapi import FastAPI
from fastapi.responses import StreamingResponse

app = FastAPI()


async def eventos():
    while True:
        yield "data: actualización\n\n"
        await asyncio.sleep(5)


@app.get("/eventos")
async def obtener_eventos():
    return StreamingResponse(
        eventos(),
        media_type="text/event-stream",
    )
```

Cliente JavaScript:

```js
const eventos = new EventSource("/eventos");

eventos.onmessage = (event) => {
    console.log(event.data);
};

eventos.onerror = () => {
    console.error("Error en el flujo SSE");
};
```

## Cuándo elegir cada uno

- **WebSocket:** necesitas enviar mensajes desde el cliente al servidor y recibir respuestas en la misma conexión.
    
- **SSE:** el cliente recibe actualizaciones del servidor y puede enviar solicitudes normales por HTTP.
    
- **SSE:** aprovecha la reconexión automática y `Last-Event-ID` para recuperar eventos, si el servidor mantiene un historial.
    
- **WebSocket:** implementa autenticación, autorización, control de recursos y reconexión según tus requisitos.
    
- **SSE:** configura correctamente los timeouts y el buffering del proxy; envía comentarios periódicos (`: ping\n\n`) si necesitas mantener activa la conexión.
    

**Regla práctica:** usa SSE para transmitir eventos en una sola dirección; usa WebSocket cuando necesites comunicación interactiva bidireccional.