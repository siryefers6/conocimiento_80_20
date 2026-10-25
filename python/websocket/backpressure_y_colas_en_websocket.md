# Backpressure y colas en WebSocket

Aplica _backpressure_ para controlar el flujo de mensajes cuando el productor genera datos más rápido de lo que el cliente puede procesarlos. La API `WebSocket` clásica del navegador no proporciona backpressure automático, por lo que debes limitar las colas y regular el envío.

## Estrategias principales

- **Cola acotada:** limita la cantidad de mensajes pendientes.
    
- **Descarte:** elimina mensajes antiguos cuando solo importa el estado más reciente.
    
- **Coalescing:** combina varias actualizaciones en una sola.
    
- **Control de flujo:** reduce el ritmo de envío cuando el cliente indica saturación.
    
- **Límite de búfer:** consulta `ws.bufferedAmount` en JavaScript para controlar los bytes pendientes de envío.
    

## Cliente JavaScript

```js
const MAX_BUFFER = 256 * 1024;

function enviar(mensaje) {
  if (ws.readyState !== WebSocket.OPEN) return false;

  const datos = JSON.stringify(mensaje);
  const bytes = new TextEncoder().encode(datos).byteLength;

  if (ws.bufferedAmount + bytes > MAX_BUFFER) {
    return false; // Rechaza o aplaza el envío.
  }

  ws.send(datos);
  return true;
}
```

`bufferedAmount` mide los bytes pendientes de transmisión desde el cliente. No indica cuántos mensajes quedan pendientes de procesamiento en el servidor.

## Servidor FastAPI: cola acotada

```python
import asyncio
from fastapi import WebSocket, WebSocketDisconnect

async def enviar_actualizaciones(ws: WebSocket):
    cola = asyncio.Queue(maxsize=100)

    async def productor():
        while True:
            mensaje = await obtener_actualizacion()
            try:
                cola.put_nowait(mensaje)
            except asyncio.QueueFull:
                # Descarta o combina datos según el caso de uso.
                pass

    async def consumidor():
        while True:
            mensaje = await cola.get()
            try:
                await ws.send_json(mensaje)
            finally:
                cola.task_done()

    tareas = [
        asyncio.create_task(productor()),
        asyncio.create_task(consumidor()),
    ]

    try:
        await asyncio.gather(*tareas)
    except WebSocketDisconnect:
        pass
    finally:
        for tarea in tareas:
            tarea.cancel()
        await asyncio.gather(*tareas, return_exceptions=True)
```

Este ejemplo es un patrón ilustrativo: implementa `obtener_actualizacion()` y adapta la política de descarte. Si necesitas entrega fiable, no descartes mensajes sin confirmación.

## Selecciona la estrategia adecuada

| Caso de uso             | Estrategia                                       |
| ----------------------- | ------------------------------------------------ |
| Posición GPS o progreso | Conserva el último estado.                       |
| Gráficos en vivo        | Agrupa muestras y limita frecuencia.             |
| Notificaciones críticas | Cola acotada con confirmaciones y persistencia.  |
| Transacciones           | Identificadores, confirmaciones y deduplicación. |

## Reglas prácticas

- Limita la cola por cantidad de mensajes y, si es necesario, por bytes.
    
- No bloquees indefinidamente al productor cuando la cola se llene.
    
- Separa el envío de mensajes de su generación.
    
- Cierra o degrada conexiones que permanezcan saturadas.
    
- Mide latencia, tamaño de colas y descartes.
    
- No confundas el envío exitoso con el procesamiento confirmado por el destinatario.
    

**Referencia:** [MDN — WebSocket](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket) · [MDN —](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket/bufferedAmount) `[bufferedAmount](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket/bufferedAmount)`.