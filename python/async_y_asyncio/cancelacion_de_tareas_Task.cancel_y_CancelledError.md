# Cancelación de tareas — `Task.cancel()` y `CancelledError`

Cancela tareas que ya no necesitas y garantiza la liberación de recursos mediante `try/finally`.

## Solicitar la cancelación

Usa `task.cancel()` para solicitar que una tarea se cancele. La excepción `asyncio.CancelledError` se inyecta cuando la tarea alcanza un punto de suspensión adecuado.

```python
import asyncio

async def tarea():
    try:
        while True:
            print("Procesando...")
            await asyncio.sleep(1)
    finally:
        print("Liberando recursos")

async def main():
    task = asyncio.create_task(tarea())
    await asyncio.sleep(2)
    task.cancel()

    try:
        await task
    except asyncio.CancelledError:
        print("Tarea cancelada")

asyncio.run(main())
```

## Reglas esenciales

- `**task.cancel()**`**:** solicita la cancelación; no garantiza que sea inmediata.
    
- `**asyncio.CancelledError**`**:** señala que la tarea está siendo cancelada.
    
- `**try/finally**`**:** libera archivos, conexiones y otros recursos.
    
- `**await task**`**:** espera la finalización de la tarea y permite observar su cancelación.
    
- **Propagación:** captura `CancelledError` solo cuando necesites realizar una acción específica y, normalmente, vuelve a lanzarla con `raise`.
    

## Regla 80/20

Solicita la cancelación con `task.cancel()`, limpia los recursos con `finally` y permite que `CancelledError` se propague correctamente. Evita suprimir la cancelación sin una razón concreta.