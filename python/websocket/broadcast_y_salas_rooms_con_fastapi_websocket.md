# Broadcast y salas (rooms) con FastAPI WebSocket

Agrupa conexiones WebSocket por tema, usuario o recurso para enviar mensajes a destinatarios específicos sin transmitirlos a todos los clientes.

## Administrador de salas

```python
from collections import defaultdict
from fastapi import WebSocket


class ConnectionManager:
    def __init__(self):
        self.rooms: dict[str, set[WebSocket]] = defaultdict(set)

    async def connect(self, room: str, ws: WebSocket):
        await ws.accept()
        self.rooms[room].add(ws)

    def disconnect(self, room: str, ws: WebSocket):
        self.rooms[room].discard(ws)

        if not self.rooms[room]:
            del self.rooms[room]

    async def broadcast(self, room: str, message: str):
        for ws in list(self.rooms.get(room, set())):
            await ws.send_text(message)


manager = ConnectionManager()
```

## Utiliza las salas

```python
from fastapi import FastAPI, WebSocket, WebSocketDisconnect

app = FastAPI()


@app.websocket("/ws/{room}")
async def websocket_endpoint(ws: WebSocket, room: str):
    # Valida la autorización para acceder a la sala antes de aceptar.
    await manager.connect(room, ws)

    try:
        while True:
            mensaje = await ws.receive_text()
            await manager.broadcast(room, mensaje)
    except WebSocketDisconnect:
        pass
    finally:
        manager.disconnect(room, ws)
```

## Patrones de distribución

- **Broadcast global:** envía a todas las conexiones.
    
- **Sala por tema:** distribuye notificaciones de un canal específico.
    
- **Sala por recurso:** actualiza clientes que observan un pedido o proceso.
    
- **Sala por usuario:** envía notificaciones privadas a sus sesiones activas.
    

## Reglas prácticas

- Autoriza cada suscripción; no confíes en el nombre de sala enviado por el cliente.
    
- Elimina conexiones desconectadas y salas vacías.
    
- Gestiona errores de envío para que un cliente caído no interrumpa la difusión.
    
- Evita bloquear el envío a todos los clientes con una conexión lenta; usa colas acotadas cuando sea necesario.
    
- Protege las estructuras compartidas si varias tareas pueden modificarlas concurrentemente.
    
- Recuerda que los registros en memoria solo abarcan un proceso. Para varias instancias, utiliza un intermediario como Redis Pub/Sub y coordina las conexiones entre trabajadores.
    

**Referencia:** [FastAPI — WebSockets](https://fastapi.tiangolo.com/advanced/websockets/).