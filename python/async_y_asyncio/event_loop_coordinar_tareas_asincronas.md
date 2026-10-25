# Event Loop — Coordinar tareas asíncronas en Python

## Función principal

El _event loop_ o bucle de eventos coordina las corutinas, las tareas y las operaciones de E/S asíncronas. Cuando una tarea encuentra un `await` que suspende su ejecución, permite avanzar a otras tareas mientras espera el resultado.

## Ejemplo mínimo

```python
import asyncio

async def tarea(nombre, demora):
    await asyncio.sleep(demora)
    print(nombre)

async def main():
    await asyncio.gather(
        tarea("A", 2),
        tarea("B", 1),
    )

asyncio.run(main())
```

Las tareas se ejecutan concurrentemente. `B` termina antes que `A` porque su espera es menor.

## Conceptos esenciales

- `**asyncio.run()**`**:** crea y gestiona el bucle de eventos para ejecutar la corutina principal.
    
- `**await**`**:** suspende la corutina actual cuando debe esperar una operación asíncrona.
    
- `**asyncio.create_task()**`**:** programa una corutina para ejecutarla como tarea.
    
- `**asyncio.gather()**`**:** espera varias operaciones concurrentes y recoge sus resultados.
    
- **Bloqueo:** una función síncrona bloqueante puede impedir que el bucle atienda otras tareas.
    

## Regla 80/20

Comprende cómo el bucle de eventos alterna tareas durante las esperas de E/S. Evita ejecutar operaciones bloqueantes dentro de una corutina y no confundas concurrencia con paralelismo.