# HTTP Upgrade y handshake WebSocket

El _handshake_ establece una conexión WebSocket mediante una solicitud HTTP inicial. El servidor acepta la actualización con el estado `101 Switching Protocols` y ambas partes comienzan a intercambiar mensajes WebSocket.

## Flujo de conexión

1. **Cliente:** envía una solicitud HTTP con los encabezados `Upgrade: websocket` y `Connection: Upgrade`.
    
2. **Servidor:** valida la solicitud y responde con `101 Switching Protocols`.
    
3. **Cambio de protocolo:** ambas partes abandonan el intercambio HTTP convencional y utilizan WebSocket sobre la misma conexión TCP.
    

## Ejemplo de solicitud

```
GET /ws HTTP/1.1
Host: localhost:8000
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Key: dGhlIHNhbXBsZSBub25jZQ==
Sec-WebSocket-Version: 13
```

## Ejemplo de respuesta

```
HTTP/1.1 101 Switching Protocols
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Accept: s3pPLMBiTxaQ9kYGzzhZRbK+xOo=
```

`Sec-WebSocket-Key` contiene un valor aleatorio codificado en Base64. El servidor calcula `Sec-WebSocket-Accept` aplicando SHA-1 a la clave concatenada con el GUID definido por RFC 6455 y codificando el resultado en Base64. Esto verifica la aceptación del handshake, pero **no autentica al servidor ni cifra la conexión**.

## Consideraciones técnicas

- Usa `wss://` en producción para proteger el tráfico mediante TLS.
    
- Valida el origen (`Origin`) y la autenticación cuando corresponda.
    
- Considera que un handshake puede fallar por autenticación, políticas de origen, encabezados inválidos o restricciones de infraestructura.
    
- Con HTTP/2, WebSocket utiliza el mecanismo de _Extended CONNECT_ definido en RFC 8441, no el `Upgrade` tradicional de HTTP/1.1.
    

**Referencia:** [RFC 6455, sección 4: Opening Handshake](https://datatracker.ietf.org/doc/html/rfc6455#section-4).