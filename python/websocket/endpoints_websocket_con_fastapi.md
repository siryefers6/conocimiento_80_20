# Endpoints WebSocket con FastAPI

Implementa rutas WebSocket con `@app.websocket()` y administra el ciclo de vida de cada conexión mediante `accept()`, `receive_text()` y `send_text()`.

## Implementación mínima

```python
from fastapi import FastAPI, WebSocket, WebSocketDisconnect

app = FastAPI()

@app.websocket("/ws")
async def websocket_endpoint(websocket: WebSocket):
    await websocket.accept()

    try:
        while True:
            mensaje = await websocket.receive_text()
            await websocket.send_text(f"Recibido: {mensaje}")
    except WebSocketDisconnect:
        print("Cliente desconectado")
```

## Métodos esenciales

| Método                             | Función                                      |
| ---------------------------------- | -------------------------------------------- |
| `@app.websocket("/ws")`            | Registra una ruta WebSocket.                 |
| `await websocket.accept()`         | Acepta el handshake y establece la conexión. |
| `await websocket.receive_text()`   | Espera y recibe un mensaje de texto.         |
| `await websocket.send_text(texto)` | Envía un mensaje de texto al cliente.        |
| `await websocket.close()`          | Inicia el cierre de la conexión.             |

## Ciclo de vida

1. Acepta la conexión con `accept()`.
    
2. Recibe mensajes dentro de un bucle asíncrono.
    
3. Procesa cada mensaje y responde cuando corresponda.
    
4. Captura `WebSocketDisconnect` cuando el cliente se desconecta.
    

## Cliente JavaScript

```js
const ws = new WebSocket("ws://localhost:8000/ws");

ws.onopen = () => ws.send("Hola FastAPI");
ws.onmessage = (event) => console.log(event.data);
```

## Reglas prácticas

- Utiliza `async def` y `await` para las operaciones WebSocket asíncronas.
    
- Captura `WebSocketDisconnect` para gestionar desconexiones normales.
    
- Valida los mensajes recibidos antes de procesarlos.
    
- Acepta la conexión después de comprobar los permisos necesarios.
    
- No confundas `receive_text()` con `receive_json()` ni con `receive_bytes()`: cada método espera un tipo de mensaje diferente.
    

**Referencia:** [FastAPI — WebSockets](https://fastapi.tiangolo.com/advanced/websockets/).