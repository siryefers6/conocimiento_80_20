# Pruebas automatizadas WebSocket con FastAPI y Pytest

Utiliza `TestClient` para probar conexiones, intercambio de mensajes, validaciones, errores y desconexiones sin iniciar un servidor externo.

## 1. Implementa el endpoint

```python
# app/main.py
from fastapi import FastAPI, WebSocket, WebSocketDisconnect

app = FastAPI()

@app.websocket("/ws")
async def websocket_endpoint(ws: WebSocket):
    await ws.accept()

    try:
        while True:
            mensaje = await ws.receive_text()

            if mensaje == "error":
                await ws.close(code=1008, reason="Mensaje no permitido")
                return

            await ws.send_text(f"Recibido: {mensaje}")
    except WebSocketDisconnect:
        pass
```

## 2. Prueba la conexión y los mensajes

```python
# tests/test_websocket.py
from fastapi.testclient import TestClient
from app.main import app

client = TestClient(app)


def test_intercambio_mensajes():
    with client.websocket_connect("/ws") as ws:
        ws.send_text("Hola")
        assert ws.receive_text() == "Recibido: Hola"


def test_cierre_por_politica():
    with client.websocket_connect("/ws") as ws:
        ws.send_text("error")

        mensaje = ws.receive()
        assert mensaje["type"] == "websocket.close"
        assert mensaje["code"] == 1008


def test_desconexion_cliente():
    with client.websocket_connect("/ws") as ws:
        ws.send_text("Hola")
        assert ws.receive_text() == "Recibido: Hola"
    # El contexto cierra la conexión al finalizar.
```

## 3. Ejecuta las pruebas

Instala las dependencias y ejecuta Pytest:

```sh
uv add --dev pytest httpx
uv run pytest tests/test_websocket.py -v
```

`TestClient` utiliza un contexto para abrir y cerrar conexiones WebSocket. No requiere un servidor Uvicorn independiente.

## 4. Casos que debes cubrir

- **Conexión:** aceptación y rechazo de clientes no autorizados.
    
- **Mensajes:** texto, JSON, datos inválidos y tipos no admitidos.
    
- **Cierre:** códigos de estado, desconexiones y limpieza de recursos.
    
- **Seguridad:** autenticación, permisos, límites de tamaño y frecuencia.
    
- **Concurrencia:** múltiples clientes conectados y difusión selectiva.
    
- **Errores:** excepciones del servicio, fallos de Redis y recuperación de conexiones.
    

## Reglas prácticas

- Comprueba resultados y efectos en la base de datos, no solo las respuestas.
    
- Aísla las dependencias externas mediante sustitutos o fixtures.
    
- Usa `pytest.mark.anyio` y clientes WebSocket asíncronos cuando necesites probar concurrencia real con `async`/`await`.
    
- Prueba reconexiones, timeouts y proxy inverso mediante pruebas de integración separadas.
    
- Evita depender del orden de ejecución de las pruebas.
    

**Referencia:** [FastAPI — Testing WebSockets](https://fastapi.tiangolo.com/advanced/testing-websockets/) · [Starlette — TestClient](https://www.starlette.io/testclient/).