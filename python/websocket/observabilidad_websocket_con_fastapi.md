# Observabilidad WebSocket con FastAPI

Registra métricas, eventos y errores para detectar problemas de disponibilidad, latencia, rendimiento y consumo de recursos.

## Métricas esenciales

|Métrica|Propósito|
|---|---|
|`websocket_connections_active`|Conexiones abiertas actualmente.|
|`websocket_connections_total`|Total de conexiones establecidas.|
|`websocket_disconnects_total`|Desconexiones por código de cierre.|
|`websocket_errors_total`|Errores por categoría.|
|`websocket_messages_total`|Mensajes recibidos y enviados.|
|`websocket_message_duration_seconds`|Tiempo de procesamiento por mensaje.|

Utiliza contadores para eventos acumulados, gauges para valores actuales e histogramas para latencias.

## Implementación básica

```python
import logging
import time

from fastapi import FastAPI, WebSocket, WebSocketDisconnect

app = FastAPI()
logger = logging.getLogger("app.websocket")


@app.websocket("/ws")
async def websocket_endpoint(ws: WebSocket):
    await ws.accept()
    logger.info("websocket_connected")

    try:
        while True:
            mensaje = await ws.receive_text()
            inicio = time.perf_counter()

            try:
                await ws.send_text(f"Recibido: {mensaje}")
            finally:
                duracion = time.perf_counter() - inicio
                logger.info(
                    "websocket_message_processed",
                    extra={"duration_seconds": duracion},
                )

    except WebSocketDisconnect as exc:
        logger.info(
            "websocket_disconnected",
            extra={"close_code": exc.code},
        )
```

Este ejemplo registra eventos y duración del envío, pero no implementa métricas agregadas ni mide por separado el tiempo de procesamiento de negocio.

## Instrumentación recomendada

- **Logging:** registra conexión, desconexión, errores y códigos de cierre.
    
- **Prometheus:** expón contadores, gauges e histogramas para crear métricas consultables.
    
- **Grafana:** visualiza conexiones activas, tasas de error y percentiles de latencia.
    
- **OpenTelemetry:** correlaciona operaciones WebSocket con trazas de servicios HTTP y bases de datos cuando corresponda.
    

## Reglas prácticas

- Incrementa el contador de conexiones activas al aceptar y decrementa exactamente una vez al finalizar.
    
- Mide por separado latencia de procesamiento, envío y respuesta de negocio.
    
- Registra códigos de cierre y excepciones sin incluir tokens ni datos personales.
    
- Evita etiquetas de alta cardinalidad, como identificadores de usuario o conexión, en métricas.
    
- Calcula percentiles de latencia y tasas de error, no solo promedios.
    
- Añade alertas para conexiones anormalmente altas, errores frecuentes y retrasos de procesamiento.
    
- En despliegues con varios trabajadores, agrega las métricas entre procesos.
    

**Referencia:** [Prometheus — Instrumentation](https://prometheus.io/docs/practices/instrumentation/) · [OpenTelemetry — Python](https://opentelemetry.io/docs/languages/python/).