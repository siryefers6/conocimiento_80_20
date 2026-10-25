# `asyncio.TaskGroup` — Agrupar tareas con control estructurado

Usa `asyncio.TaskGroup` para crear y gestionar un conjunto de tareas relacionadas. El bloque espera su finalización y, si una tarea falla con una excepción distinta de `CancelledError`, cancela las tareas restantes y propaga los errores al salir.

## Ejemplo mínimo

```python
import asyncio

async def obtener_dato(numero):
    await asyncio.sleep(1)
    return numero * 2

async def main():
    async with asyncio.TaskGroup() as grupo:
        tarea_a = grupo.create_task(obtener_dato(10))
        tarea_b = grupo.create_task(obtener_dato(20))

    print(tarea_a.result())  # 20
    print(tarea_b.result())  # 40

asyncio.run(main())
```

## Reglas esenciales

- `**async with**`**:** delimita el ciclo de vida del grupo.
    
- `**grupo.create_task()**`**:** programa cada corutina dentro del grupo.
    
- **Finalización:** espera a que todas las tareas terminen antes de salir del bloque.
    
- **Errores:** cancela las tareas restantes cuando una falla y propaga las excepciones mediante `ExceptionGroup` o `BaseExceptionGroup`, según corresponda.
    
- **Cancelación:** no ocultes `asyncio.CancelledError`; permite que la cancelación se propague correctamente.
    

## Regla 80/20

Prefiere `asyncio.TaskGroup` cuando varias tareas forman una misma operación y deben compartir un ciclo de vida. Utiliza `asyncio.gather()` cuando necesites principalmente reunir resultados y su comportamiento de errores sea adecuado para el caso.