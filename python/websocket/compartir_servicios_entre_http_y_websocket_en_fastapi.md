# Compartir servicios entre HTTP y WebSocket en FastAPI

Extrae las reglas de negocio a una capa de servicios independiente. Invoca esos servicios desde los endpoints HTTP y WebSocket para evitar duplicar validaciones, consultas y operaciones.

## Arquitectura

```
Endpoint HTTP ──────┐
                    ├── Servicio de negocio ── Repositorio ── Base de datos
Endpoint WebSocket ┘
```

- **Endpoints:** reciben solicitudes y gestionan el protocolo de comunicación.
    
- **Servicios:** ejecutan las reglas de negocio y validan operaciones.
    
- **Repositorios:** encapsulan el acceso a datos cuando sea necesario.
    

## Servicio compartido

```python
# services/procesos.py

async def obtener_estado(session, proceso_id: int) -> dict:
    proceso = await consultar_proceso(session, proceso_id)

    if proceso is None:
        raise ValueError("Proceso no encontrado")

    return {
        "id": proceso.id,
        "estado": proceso.estado,
    }
```

## Endpoint HTTP

```python
@app.get("/procesos/{proceso_id}")
async def consultar_http(proceso_id: int, session=Depends(get_session)):
    return await obtener_estado(session, proceso_id)
```

## Endpoint WebSocket

```python
@app.websocket("/ws/procesos/{proceso_id}")
async def consultar_ws(ws: WebSocket, proceso_id: int):
    await ws.accept()

    try:
        async for _ in iterar_solicitudes(ws):
            async with session_factory() as session:
                estado = await obtener_estado(session, proceso_id)
            await ws.send_json({"tipo": "estado", "datos": estado})
    except WebSocketDisconnect:
        pass
```

_Nota:_ `consultar_proceso`, `get_session`, `session_factory` e `iterar_solicitudes` representan componentes que debes implementar según tu proyecto. El ejemplo muestra la separación de responsabilidades, no una aplicación completa.

## Reglas prácticas

- Mantén las reglas de negocio en los servicios, no en los endpoints.
    
- Comparte servicios entre HTTP y WebSocket, pero adapta la entrada y la salida a cada protocolo.
    
- Gestiona las sesiones de base de datos por operación; no compartas una misma sesión concurrentemente entre tareas.
    
- Traduce las excepciones de negocio a códigos HTTP en REST y a mensajes de error estructurados en WebSocket.
    
- Usa transacciones para operaciones que modifiquen datos.
    
- No confundas reutilizar un servicio con reutilizar una conexión de base de datos.
    

**Regla:** comparte la lógica de negocio; conserva independientes los mecanismos de transporte, autenticación y gestión de conexiones.