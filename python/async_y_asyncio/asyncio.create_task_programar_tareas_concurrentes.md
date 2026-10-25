# `asyncio.create_task()` — Programar tareas concurrentes

Usa `asyncio.create_task()` para programar una corutina como una tarea que el bucle de eventos pueda ejecutar concurrentemente con otras tareas.

## Ejemplo mínimo

```python
import asyncio

async def tarea():
    await asyncio.sleep(1)
    return "Completado"

async def main():
    tarea_programada = asyncio.create_task(tarea())
    resultado = await tarea_programada
    print(resultado)

asyncio.run(main())
```

## Reglas esenciales

- Pasa un objeto corutina a `asyncio.create_task()`.
    
- Guarda la tarea devuelta para esperarla o cancelar su ejecución.
    
- Usa `await tarea` para esperar su resultado.
    
- Programa varias tareas antes de esperarlas para permitir su ejecución concurrente.
    
- Gestiona las excepciones y evita dejar tareas sin supervisión.
    

## Ejecución concurrente

```python
async def main():
    tarea_a = asyncio.create_task(tarea())
    tarea_b = asyncio.create_task(tarea())

    resultado_a = await tarea_a
    resultado_b = await tarea_b
```

**Regla 80/20:** utiliza `create_task()` para programar una corutina sin esperar inmediatamente su finalización. Recuerda que crea concurrencia, no paralelismo de CPU.