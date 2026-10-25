# Concurrencia limitada — Controlar la cantidad de tareas simultáneas

Limita el trabajo concurrente para evitar consumir demasiada memoria, saturar conexiones o sobrecargar servicios externos.

## Patrón recomendado: `asyncio.Queue` y trabajadores

```python
import asyncio

async def procesar(dato):
    await asyncio.sleep(1)
    print(f"Procesado: {dato}")

async def trabajador(cola):
    while True:
        dato = await cola.get()
        try:
            if dato is None:
                return
            await procesar(dato)
        finally:
            cola.task_done()

async def main():
    cola = asyncio.Queue(maxsize=20)
    cantidad = 5

    async with asyncio.TaskGroup() as grupo:
        for _ in range(cantidad):
            grupo.create_task(trabajador(cola))

        for dato in range(100):
            await cola.put(dato)

        await cola.join()

        for _ in range(cantidad):
            await cola.put(None)

asyncio.run(main())
```

Este patrón procesa 100 elementos con solo 5 trabajadores concurrentes y limita la cola a 20 elementos pendientes.

## Estrategias esenciales

- `**asyncio.Semaphore(N)**`**:** limita cuántas tareas acceden simultáneamente a un recurso.
    
- `**asyncio.Queue(maxsize=N)**`**:** limita el trabajo pendiente y aplica contrapresión a los productores.
    
- **Grupo de trabajadores:** crea un número fijo de tareas para procesar muchos elementos.
    
- **Procesamiento por lotes:** divide grandes conjuntos de datos en bloques manejables.
    

## Regla 80/20

No crees una tarea por cada elemento cuando el volumen sea elevado. Usa una cola acotada y un número fijo de trabajadores para controlar tanto el trabajo pendiente como la concurrencia efectiva.