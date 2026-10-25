# Autenticación y autorización en WebSocket con FastAPI

Autentica al usuario antes de aceptar la conexión y verifica sus permisos antes de enviar información o ejecutar operaciones. Mantén las mismas reglas de seguridad que en los endpoints HTTP.

## Métodos de autenticación

- **Cookies de sesión:** aprovecha la sesión existente del navegador y valida su identidad en el servidor.
    
- **JWT:** valida firma, expiración, emisor y audiencia del token.
    
- **Dependencias de FastAPI:** reutiliza funciones de autenticación y autorización en endpoints HTTP y WebSocket.
    
- **Autorización por recurso:** comprueba que el usuario tenga permiso para acceder al canal, proceso o registro solicitado.
    

## Ejemplo: validar un token antes de aceptar

```python
from fastapi import FastAPI, WebSocket, WebSocketException, status

app = FastAPI()


async def autenticar_token(token: str) -> dict | None:
    # Sustituye por la validación real de firma, expiración y permisos.
    return await verificar_jwt(token)


@app.websocket("/ws")
async def endpoint(ws: WebSocket):
    token = ws.query_params.get("token")
    usuario = await autenticar_token(token) if token else None

    if not usuario:
        raise WebSocketException(
            code=status.WS_1008_POLICY_VIOLATION
        )

    await ws.accept()

    try:
        while True:
            mensaje = await ws.receive_text()

            if not usuario_tiene_permiso(usuario, "enviar_mensajes"):
                await ws.close(code=1008)
                return

            await ws.send_text(f"Recibido: {mensaje}")
    except WebSocketDisconnect:
        pass
```

_Nota:_ `verificar_jwt()` y `usuario_tiene_permiso()` son funciones ilustrativas que debes implementar. Importa `WebSocketDisconnect` desde `fastapi` para completar el ejemplo.

## Reglas de seguridad

- Autentica antes de ejecutar `accept()`.
    
- Usa `wss://` en producción.
    
- Valida el encabezado `Origin` para mitigar ataques de _Cross-Site WebSocket Hijacking_ cuando utilices cookies.
    
- No confíes en identificadores de usuario enviados por el cliente.
    
- Evita incluir tokens sensibles en la URL, ya que pueden aparecer en registros.
    
- Revalida permisos cuando cambien durante una conexión persistente.
    
- Limita conexiones, frecuencia de mensajes y tamaño de los datos.
    
- No asumas que un token válido autoriza el acceso a todos los recursos.
    

**Regla:** autentica la identidad, autoriza cada operación sensible y cierra la conexión cuando ya no cumpla las políticas de seguridad.

**Referencia:** [FastAPI — WebSockets](https://fastapi.tiangolo.com/advanced/websockets/) · [OWASP — WebSocket Security Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/WebSocket_Security_Cheat_Sheet.html).