# Heartbeat y detección de desconexiones WebSocket

Detecta conexiones inactivas mediante mensajes periódicos de comprobación (_heartbeat_). Cierra las conexiones que no respondan dentro del tiempo límite y libera sus recursos.

## Métodos

- **Ping/Pong del protocolo:** utiliza tramas de control para comprobar que el otro extremo responde. Las bibliotecas de servidor, como Uvicorn, pueden configurarlas mediante su implementación WebSocket.
    
- **Heartbeat de aplicación:** intercambia mensajes como `{"tipo":"ping"}` y `{"tipo":"pong"}`. Úsalo cuando necesites controlar la actividad desde la lógica de negocio.
    

## FastAPI: Ping/Pong del protocolo

Con Uvicorn y su implementación WebSocket compatible, configura los intervalos de Ping/Pong:

```sh
uv run uvicorn app.main:app \
  --ws websockets \
  --ws-ping-interval 20 \
  --ws-ping-timeout 20
```

El servidor envía un ping cada 20 segundos y espera la respuesta correspondiente. Si vence el tiempo límite, la conexión se considera inactiva y se cierra.

## Heartbeat de aplicación

Cliente JavaScript:

```js
const ws = new WebSocket("ws://localhost:8000/ws");

setInterval(() => {
  if (ws.readyState === WebSocket.OPEN) {
    ws.send(JSON.stringify({ tipo: "ping" }));
  }
}, 20000);

ws.onmessage = (event) => {
  const mensaje = JSON.parse(event.data);

  if (mensaje.tipo === "pong") {
    console.log("Servidor activo");
  }
};
```

Servidor FastAPI:

```python
@app.websocket("/ws")
async def endpoint(ws: WebSocket):
    await ws.accept()

    try:
        while True:
            mensaje = await ws.receive_json()

            if mensaje.get("tipo") == "ping":
                await ws.send_json({"tipo": "pong"})
    except WebSocketDisconnect:
        pass
```

## Reglas prácticas

- Prefiere Ping/Pong del protocolo para detectar conexiones rotas sin definir mensajes adicionales.
    
- Usa un heartbeat de aplicación si necesitas verificar que la lógica del servidor también responde.
    
- Define intervalos y tiempos límite adecuados para tu infraestructura.
    
- Considera que los proxies pueden cerrar conexiones inactivas.
    
- No interpretes la ausencia de mensajes de negocio como una desconexión.
    
- Cancela tareas de heartbeat y libera recursos al cerrar la conexión.
    

**Referencia:** [RFC 6455 — Control Frames](https://datatracker.ietf.org/doc/html/rfc6455#section-5.5) · [Uvicorn — Settings](https://www.uvicorn.org/settings/).