# Ciclo de vida de una conexión WebSocket con FastAPI

Gestiona cada conexión en cuatro fases: aceptación, comunicación, desconexión y liberación de recursos.

## Fases del ciclo de vida

1. **Aceptación:** valida permisos y ejecuta `await websocket.accept()`.
    
2. **Recepción:** procesa mensajes mientras la conexión permanezca activa.
    
3. **Desconexión:** captura `WebSocketDisconnect` cuando el cliente cierra la conexión.
    
4. **Liberación:** elimina la conexión de los registros y libera tareas, suscripciones y recursos asociados.
    

## Implementación

```python
from fastapi import FastAPI, WebSocket, WebSocketDisconnect

app = FastAPI()

@app.websocket("/ws")
async def websocket_endpoint(ws: WebSocket):
    await ws.accept()

    try:
        while True:
            mensaje = await ws.receive_text()
            await ws.send_text(f"Recibido: {mensaje}")
    except WebSocketDisconnect:
        print("Cliente desconectado")
    finally:
        # Libera aquí los recursos asociados a esta conexión.
        print("Recursos liberados")
```

## Reglas prácticas

- Ejecuta `accept()` antes de recibir o enviar mensajes.
    
- Captura `WebSocketDisconnect` para gestionar la desconexión esperada.
    
- Utiliza `finally` para garantizar la limpieza de recursos.
    
- Elimina las conexiones desconectadas de cualquier registro de clientes.
    
- Cancela las tareas asociadas y elimina las suscripciones activas.
    
- No llames a `close()` indiscriminadamente en `finally`: la conexión puede estar cerrada. Si necesitas cerrarla explícitamente, comprueba su estado y gestiona los errores correspondientes.
    

**Referencia:** [FastAPI — WebSockets](https://fastapi.tiangolo.com/advanced/websockets/).