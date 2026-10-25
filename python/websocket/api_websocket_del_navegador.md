# API WebSocket del navegador

**Referencia:** JavaScript WebSocket API · Octubre de 2026

Utiliza `WebSocket` para establecer una conexión bidireccional con un servidor. Gestiona el envío, la recepción y el cierre mediante sus métodos y eventos.

## Métodos y propiedades

| Elemento             | Función                                         |
| -------------------- | ----------------------------------------------- |
| `new WebSocket(url)` | Crea la conexión.                               |
| `send(data)`         | Envía texto, datos binarios o un objeto `Blob`. |
| `close()`            | Inicia el cierre de la conexión.                |
| `readyState`         | Consulta el estado actual de la conexión.       |

Estados de `readyState`:

- `WebSocket.CONNECTING` (`0`): conectando.
    
- `WebSocket.OPEN` (`1`): conexión abierta.
    
- `WebSocket.CLOSING` (`2`): cerrando.
    
- `WebSocket.CLOSED` (`3`): conexión cerrada.
    

## Eventos principales

| Evento    | Cuándo ocurre                    |
| --------- | -------------------------------- |
| `open`    | Se establece la conexión.        |
| `message` | Se recibe un mensaje.            |
| `error`   | Ocurre un error de comunicación. |
| `close`   | La conexión se cierra.           |

## Ejemplo funcional

```js
const ws = new WebSocket("ws://localhost:8000/ws");

ws.addEventListener("open", () => {
  console.log("Conectado");
  ws.send(JSON.stringify({ tipo: "saludo" }));
});

ws.addEventListener("message", (event) => {
  console.log("Mensaje:", event.data);
});

ws.addEventListener("error", (event) => {
  console.error("Error WebSocket:", event);
});

ws.addEventListener("close", (event) => {
  console.log("Cerrado:", event.code, event.reason);
});

// Envía datos solo cuando la conexión esté abierta.
function enviar(datos) {
  if (ws.readyState === WebSocket.OPEN) {
    ws.send(JSON.stringify(datos));
  }
}

// Cierra la conexión de forma ordenada.
function desconectar() {
  if (
    ws.readyState === WebSocket.OPEN ||
    ws.readyState === WebSocket.CONNECTING
  ) {
    ws.close(1000, "Cierre normal");
  }
}
```

## Reglas prácticas

- Espera al evento `open` antes de enviar mensajes.
    
- Comprueba `readyState` antes de llamar a `send()`.
    
- Recuerda que `send()` no espera una confirmación del servidor.
    
- Interpreta `event.data` según el formato recibido.
    
- Usa `close(1000, "Cierre normal")` para iniciar un cierre normal.
    
- Implementa la reconexión explícitamente cuando sea necesaria; el navegador no la realiza automáticamente.
    

**Referencia:** [MDN — WebSocket](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket).