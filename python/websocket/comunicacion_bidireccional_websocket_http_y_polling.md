# Comunicación bidireccional: WebSocket, HTTP y polling

Selecciona el mecanismo según la frecuencia de actualización y la necesidad de comunicación en tiempo real.

## Comparación

|Mecanismo|Funcionamiento|Uso recomendado|
|---|---|---|
|HTTP tradicional|El cliente solicita y el servidor responde.|CRUD, formularios y consultas puntuales.|
|Polling|El cliente repite solicitudes HTTP a intervalos fijos.|Actualizaciones periódicas poco frecuentes.|
|Long polling|El servidor mantiene una solicitud abierta hasta tener datos o alcanzar un límite de tiempo.|Notificaciones sin soporte WebSocket.|
|WebSocket|Cliente y servidor intercambian mensajes mediante una conexión persistente y bidireccional.|Notificaciones instantáneas, estados en vivo y seguimiento de procesos.|

## Ejemplos mínimos

**Polling con JavaScript**

```js
setInterval(async () => {
  const response = await fetch("/api/estado");
  console.log(await response.json());
}, 5000);
```

**WebSocket con JavaScript**

```js
const ws = new WebSocket("wss://ejemplo.com/ws");

ws.onmessage = (event) => {
  console.log(JSON.parse(event.data));
};

ws.send(JSON.stringify({ tipo: "actualizar" }));
```

## Criterios de selección

- Usa **HTTP** para operaciones independientes de solicitud y respuesta.
    
- Usa **polling** cuando toleres retrasos y prefieras una implementación sencilla.
    
- Usa **long polling** cuando necesites reducir solicitudes repetitivas sin mantener un canal WebSocket.
    
- Usa **WebSocket** cuando necesites baja latencia y comunicación frecuente en ambas direcciones.
    

**Considera:** WebSocket mantiene conexiones abiertas y requiere gestionar desconexiones, reconexiones, autenticación y escalabilidad. No garantiza por sí mismo la entrega de mensajes ni su persistencia.

**Referencia:** [RFC 6455 — The WebSocket Protocol](https://datatracker.ietf.org/doc/html/rfc6455).