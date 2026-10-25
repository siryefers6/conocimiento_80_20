# Protocolo WebSocket (RFC 6455)

Establece una conexión persistente y bidireccional entre cliente y servidor sobre TCP. Permite intercambiar mensajes sin iniciar una petición HTTP por cada actualización.

## Funcionamiento

1. **Handshake:** el cliente solicita actualizar la conexión HTTP a WebSocket mediante `Upgrade`.
    
2. **Conexión establecida:** el servidor acepta la solicitud y comienza la comunicación bidireccional.
    
3. **Intercambio de mensajes:** ambas partes envían y reciben mensajes de texto o binarios de forma independiente.
    
4. **Cierre:** cualquiera de las partes puede iniciar el cierre mediante una trama de control; la otra responde y se termina la conexión.
    

## Ejemplo mínimo

Cliente JavaScript:

```js
const ws = new WebSocket("ws://localhost:8000/ws");

ws.onopen = () => ws.send("Hola servidor");

ws.onmessage = (event) => {
  console.log("Mensaje:", event.data);
};

ws.onclose = () => console.log("Conexión cerrada");
ws.onerror = (error) => console.error(error);
```

Servidor FastAPI:

```python
from fastapi import FastAPI, WebSocket

app = FastAPI()

@app.websocket("/ws")
async def websocket_endpoint(ws: WebSocket):
    await ws.accept()

    try:
        while True:
            mensaje = await ws.receive_text()
            await ws.send_text(f"Recibido: {mensaje}")
    except Exception:
        await ws.close()
```

## Conceptos clave

- `ws://`: conexión WebSocket sin cifrado.
    
- `wss://`: conexión WebSocket protegida mediante TLS.
    
- **Trama:** unidad de transmisión del protocolo.
    
- **Mensaje:** contenido lógico que puede componerse de varias tramas.
    
- **Ping/Pong:** permite comprobar la conexión.
    
- **Close:** inicia el cierre ordenado del canal.
    

**Regla:** utiliza WebSocket para comunicación bidireccional en tiempo real. Para operaciones simples de solicitud y respuesta, conserva HTTP.

**Referencia:** [RFC 6455 — The WebSocket Protocol](https://datatracker.ietf.org/doc/html/rfc6455).