# `asyncio.Semaphore` — Limitar operaciones concurrentes

Usa `asyncio.Semaphore(N)` para limitar cuántas tareas pueden acceder simultáneamente a un recurso. Es útil para controlar solicitudes HTTP, conexiones o llamadas a servicios externos.

## Ejemplo mínimo

```python
import asyncio

semaforo = asyncio.Semaphore(3)

async def solicitar(numero):
    async with semaforo:
        print(f"Solicitud {numero} iniciada")
        await asyncio.sleep(1)
        print(f"Solicitud {numero} finalizada")

async def main():
    await asyncio.gather(
        *(solicitar(i) for i in range(10))
    )

asyncio.run(main())
```

En este ejemplo, como máximo tres solicitudes ejecutan simultáneamente el bloque protegido.

## Reglas esenciales

- `**asyncio.Semaphore(N)**`**:** permite hasta `N` operaciones simultáneas.
    
- `**async with semaforo**`**:** adquiere un permiso y lo libera automáticamente al salir.
    
- **Contrapresión:** limita el acceso concurrente, pero no la cantidad total de tareas creadas.
    
- `**asyncio.BoundedSemaphore(N)**`**:** detecta liberaciones que exceden la capacidad inicial.
    

## Regla 80/20

Utiliza `asyncio.Semaphore` para controlar la concurrencia sobre recursos limitados. Combínalo con un cliente HTTP asíncrono y define límites acordes con la capacidad del servicio remoto.