# Pub/Sub con Redis para WebSocket en FastAPI

Utiliza Redis Pub/Sub para distribuir eventos entre varias instancias de FastAPI cuando las conexiones WebSocket están repartidas entre distintos procesos o servidores.

## Arquitectura

```
Cliente A ── WebSocket ── FastAPI 1 ──┐
                                      ├── Redis Pub/Sub
Cliente B ── WebSocket ── FastAPI 2 ──┘
```

Cada instancia mantiene sus propias conexiones activas y se suscribe a un canal Redis compartido. Cuando una instancia publica un evento, las demás instancias suscritas lo reciben y lo envían a sus clientes locales.

## Instalación

```sh
uv add redis
```

## Publicar eventos

```python
import json
from redis.asyncio import Redis

redis = Redis.from_url(
    "redis://localhost:6379",
    decode_responses=True,
)

async def publicar_evento(canal: str, evento: dict):
    await redis.publish(canal, json.dumps(evento))
```

## Suscribirse y distribuir mensajes

```python
import asyncio
import json

async def escuchar_eventos(manager, canal: str):
    async with redis.pubsub() as pubsub:
        await pubsub.subscribe(canal)

        try:
            async for mensaje in pubsub.listen():
                if mensaje["type"] != "message":
                    continue

                evento = json.loads(mensaje["data"])
                await manager.broadcast_local(canal, evento)
        finally:
            await pubsub.unsubscribe(canal)
```

Implementa `broadcast_local()` para enviar el evento únicamente a las conexiones registradas en la instancia actual. Ejecuta `escuchar_eventos()` como tarea de fondo durante el ciclo de vida de la aplicación.

## Reglas prácticas

- Mantén las conexiones WebSocket en memoria local; utiliza Redis para distribuir eventos entre instancias.
    
- Crea una suscripción por canal o agrupa canales según el volumen de eventos.
    
- Valida el contenido recibido de Redis antes de distribuirlo.
    
- Gestiona reconexiones y errores de conexión con Redis.
    
- Cierra el cliente Redis durante el apagado de FastAPI.
    
- Evita publicar secretos o datos que los clientes no estén autorizados a recibir.
    

**Limitación crítica:** Redis Pub/Sub ofrece entrega _at-most-once_. Si un suscriptor está desconectado, pierde los mensajes publicados durante ese período. Utiliza Redis Streams u otro sistema persistente cuando necesites recuperación, confirmaciones o procesamiento fiable.

**Referencia:** [Redis — Pub/Sub](https://redis.io/docs/latest/develop/pubsub/) · [FastAPI — WebSockets](https://fastapi.tiangolo.com/advanced/websockets/).