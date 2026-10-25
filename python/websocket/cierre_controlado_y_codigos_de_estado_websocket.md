# Cierre controlado y códigos de estado WebSocket

Cierra las conexiones de forma ordenada, comunica el motivo mediante códigos de estado y libera los recursos asociados. Distingue los cierres normales de los errores de protocolo y las desconexiones inesperadas.

## Códigos de cierre principales

|Código|Significado|Uso|
|---|---|---|
|`1000`|Cierre normal|Operación completada.|
|`1001`|Going Away|Servidor detenido o cliente abandonando la página.|
|`1002`|Error de protocolo|Mensaje incompatible con el protocolo.|
|`1003`|Tipo no admitido|El servidor no admite el tipo de datos recibido.|
|`1007`|Datos inválidos|Contenido incompatible con el formato esperado.|
|`1008`|Violación de política|Autorización o reglas de aplicación incumplidas.|
|`1009`|Mensaje demasiado grande|Tamaño superior al límite permitido.|
|`1011`|Error interno|Fallo inesperado del servidor.|

No envíes `1005`, `1006` ni `1015` en una trama de cierre: son códigos reservados para uso interno.

## Cerrar desde FastAPI

```python
from fastapi import WebSocket, WebSocketDisconnect


async def endpoint(ws: WebSocket):
    await ws.accept()

    try:
        while True:
            mensaje = await ws.receive_text()

            if mensaje == "salir":
                await ws.close(code=1000, reason="Operación completada")
                return

            await ws.send_text(f"Recibido: {mensaje}")

    except WebSocketDisconnect:
        print("Conexión interrumpida")
    finally:
        # Elimina registros y libera recursos asociados.
        pass
```

## Detectar el cierre en JavaScript

```js
ws.addEventListener("close", (event) => {
  console.log("Código:", event.code);
  console.log("Motivo:", event.reason);
  console.log("Cierre limpio:", event.wasClean);
});
```

`wasClean` indica si el cierre se completó correctamente según el protocolo; no garantiza que todas las operaciones de negocio hayan terminado.

## Reglas prácticas

- Envía `1000` para cierres normales.
    
- Utiliza `1008` para incumplimientos de políticas de aplicación.
    
- Registra los errores internos sin exponer información sensible al cliente.
    
- Captura `WebSocketDisconnect` para gestionar desconexiones inesperadas.
    
- Utiliza `finally` para eliminar conexiones registradas, cancelar tareas y liberar recursos.
    
- No supongas que siempre recibirás un código de cierre: una caída de red puede terminar la conexión abruptamente.
    
- Implementa reconexión cuando corresponda y evita repetir operaciones no idempotentes sin mecanismos de deduplicación.
    

**Referencia:** [RFC 6455 — Close](https://datatracker.ietf.org/doc/html/rfc6455#section-7.4).