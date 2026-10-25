# Gestión de conexiones WebSocket activas con FastAPI

Mantén un registro de conexiones activas para enviar mensajes a un cliente específico o difundirlos a varios clientes conectados.

## Administrador de conexiones

```python
from fastapi import WebSocket


class ConnectionManager:
    def __init__(self):
        self.active: dict[str, WebSocket] = {}

    async def connect(self, client_id: str, ws: WebSocket):
        await ws.accept()
        self.active[client_id] = ws

    def disconnect(self, client_id: str):
        self.active.pop(client_id, None)

    async def send_to(self, client_id: str, message: str):
        ws = self.active.get(client_id)
        if ws:
            await ws.send_text(message)

    async def broadcast(self, message: str):
        for ws in list(self.active.values()):
            await ws.send_text(message)


manager = ConnectionManager()
```

## Utiliza el administrador

```python
from fastapi import FastAPI, WebSocket, WebSocketDisconnect

app = FastAPI()


@app.websocket("/ws/{client_id}")
async def websocket_endpoint(ws: WebSocket, client_id: str):
    await manager.connect(client_id, ws)

    try:
        while True:
            mensaje = await ws.receive_text()
            await manager.send_to(client_id, f"Recibido: {mensaje}")
    except WebSocketDisconnect:
        pass
    finally:
        manager.disconnect(client_id)
```

## Operaciones principales

- `connect()`: acepta y registra al cliente.
    
- `disconnect()`: elimina al cliente sin generar errores si ya no existe.
    
- `send_to()`: envía un mensaje a un destinatario específico.
    
- `broadcast()`: envía un mensaje a todas las conexiones registradas.
    

## Consideraciones técnicas

- **Identificadores únicos:** evita sobrescribir conexiones cuando un mismo cliente abre varias sesiones.
    
- **Errores de envío:** gestiona desconexiones inesperadas y elimina las conexiones inválidas.
    
- **Concurrencia:** protege el registro si necesitas operaciones coordinadas entre tareas.
    
- **Escalabilidad:** este registro solo funciona dentro de un proceso. Para varios trabajadores o servidores, utiliza un intermediario como Redis o un sistema de mensajería.
    
- **Seguridad:** autentica a los clientes y autoriza los destinatarios; no confíes en `client_id` como prueba de identidad.
    

**Referencia:** [FastAPI — WebSockets y Connection Manager](https://fastapi.tiangolo.com/advanced/websockets/).