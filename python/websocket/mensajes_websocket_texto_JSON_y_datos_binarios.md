# Mensajes WebSocket: texto, JSON y datos binarios

Define un formato de mensajes consistente para intercambiar información entre JavaScript y FastAPI mediante WebSocket.

## Tipos de datos

| Tipo    | Uso                                   | Ejemplo                                     |
| ------- | ------------------------------------- | ------------------------------------------- |
| Texto   | Mensajes simples.                     | `"Proceso iniciado"`                        |
| JSON    | Datos estructurados y eventos.        | `{"tipo":"estado","datos":{"progreso":50}}` |
| Binario | Imágenes, archivos y datos compactos. | `bytes`, `Blob`, `ArrayBuffer`              |

JSON es un formato de texto, no un tipo de mensaje WebSocket independiente. Serializa objetos con `JSON.stringify()` y recupéralos con `JSON.parse()`.

## Define un contrato de mensajes

Utiliza una estructura común con tipo, identificador y datos:

```
{
  "tipo": "estado",
  "id": "req-001",
  "datos": {
    "progreso": 50,
    "estado": "procesando"
  }
}
```

- `tipo`: identifica el evento o la operación.
    
- `id`: correlaciona solicitudes y respuestas cuando sea necesario.
    
- `datos`: contiene la información específica del mensaje.
    

## JavaScript: enviar y recibir JSON

```js
ws.send(JSON.stringify({
  tipo: "suscripcion",
  id: "req-001",
  datos: { canal: "procesos" }
}));

ws.addEventListener("message", (event) => {
  if (typeof event.data !== "string") return;

  try {
    const mensaje = JSON.parse(event.data);

    switch (mensaje.tipo) {
      case "estado":
        console.log(mensaje.datos.progreso);
        break;
      case "error":
        console.error(mensaje.datos.mensaje);
        break;
    }
  } catch {
    console.error("JSON inválido");
  }
});
```

## FastAPI: recibir y enviar JSON

```python
from fastapi import FastAPI, WebSocket, WebSocketDisconnect

app = FastAPI()

@app.websocket("/ws")
async def endpoint(ws: WebSocket):
    await ws.accept()

    try:
        while True:
            mensaje = await ws.receive_json()

            if mensaje.get("tipo") == "suscripcion":
                await ws.send_json({
                    "tipo": "estado",
                    "id": mensaje.get("id"),
                    "datos": {"progreso": 50},
                })
    except WebSocketDisconnect:
        pass
```

## Datos binarios

Envía un `ArrayBuffer` o un `Blob` directamente:

```js
const buffer = await archivo.arrayBuffer();
ws.send(buffer);
```

En FastAPI, recibe los bytes con `await ws.receive_bytes()`. Define un protocolo separado para distinguir tipos de archivos, metadatos y contenido binario; no supongas que todos los mensajes son JSON.

## Reglas de diseño

- Define nombres de eventos estables: `estado`, `notificacion`, `error`.
    
- Valida estructura, tipos y tamaño en el servidor.
    
- Usa un esquema de versión cuando el protocolo pueda evolucionar.
    
- Establece límites de tamaño y evita enviar archivos grandes sin control de flujo.
    
- No confundas la recepción de un mensaje con la confirmación de que una operación se completó.
    

**Referencia:** [MDN — WebSocket](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket) · [FastAPI — WebSockets](https://fastapi.tiangolo.com/advanced/websockets/).