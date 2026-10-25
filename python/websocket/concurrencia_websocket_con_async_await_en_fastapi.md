# Concurrencia WebSocket con async/await en FastAPI

Utiliza `async def` y `await` para atender múltiples conexiones sin bloquear el bucle de eventos (_event loop_). Cada conexión puede esperar mensajes mientras otras conexiones continúan ejecutándose.

## Operaciones bloqueantes y no bloqueantes

- **No bloqueante:** `await websocket.receive_text()`, `await asyncio.sleep(1)` y operaciones de E/S asíncronas.
    
- **Bloqueante:** `time.sleep(1)`, cálculos intensivos de CPU y funciones síncronas de E/S ejecutadas directamente dentro de una función asíncrona.
    

## Ejemplo: conexiones concurrentes

```python
import asyncio

from fastapi import FastAPI, WebSocket, WebSocketDisconnect

app = FastAPI()

@app.websocket("/ws")
async def websocket_endpoint(ws: WebSocket):
    await ws.accept()

    try:
        while True:
            mensaje = await ws.receive_text()
            await ws.send_text(f"Recibido: {mensaje}")
            await asyncio.sleep(0)  # Cede el control al event loop.
    except WebSocketDisconnect:
        pass
```

Uvicorn puede atender múltiples conexiones mientras cada tarea espera operaciones asíncronas. No necesitas crear un hilo por conexión.

## Ejecuta tareas concurrentes

Utiliza `asyncio.create_task()` para iniciar tareas independientes, como enviar actualizaciones periódicas:

```python
async def notificar(ws: WebSocket):
    try:
        while True:
            await ws.send_text("Estado actualizado")
            await asyncio.sleep(5)
    except WebSocketDisconnect:
        pass
```

Integra esta tarea con el ciclo de vida de la conexión y cancélala al desconectarse el cliente. Si varias tareas envían mensajes por el mismo socket, coordina los envíos para evitar problemas de orden.

## Reglas prácticas

- Usa bibliotecas asíncronas para operaciones de red y bases de datos.
    
- No ejecutes `time.sleep()` ni E/S bloqueante dentro de una corrutina.
    
- Usa `asyncio.to_thread()` para trasladar funciones bloqueantes de E/S a un hilo, cuando corresponda.
    
- Usa procesos o trabajadores separados para cálculos intensivos de CPU.
    
- Controla las tareas creadas con `asyncio.create_task()` y cancélalas cuando dejen de ser necesarias.
    
- Limita colas y tareas pendientes para evitar un consumo ilimitado de memoria.
    

**Referencia:** [Python — asyncio](https://docs.python.org/3/library/asyncio.html) · [FastAPI — WebSockets](https://fastapi.tiangolo.com/advanced/websockets/).