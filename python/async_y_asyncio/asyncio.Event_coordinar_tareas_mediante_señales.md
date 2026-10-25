# `asyncio.Event` — Coordinar tareas mediante señales

Usa `asyncio.Event` para permitir que varias tareas esperen una señal compartida. Cuando una tarea activa el evento, todas las tareas que esperan en `wait()` pueden continuar.

## Ejemplo mínimo

```python
import asyncio

async def trabajador(evento):
    print("Esperando señal...")
    await evento.wait()
    print("Señal recibida")

async def main():
    evento = asyncio.Event()

    async with asyncio.TaskGroup() as grupo:
        grupo.create_task(trabajador(evento))
        grupo.create_task(trabajador(evento))

        await asyncio.sleep(1)
        evento.set()

asyncio.run(main())
```

## Métodos esenciales

- `**asyncio.Event()**`**:** crea un evento inicialmente desactivado.
    
- `**await evento.wait()**`**:** espera hasta que el evento se active.
    
- `**evento.set()**`**:** activa el evento y despierta a las tareas que esperan.
    
- `**evento.clear()**`**:** desactiva el evento para futuras esperas.
    
- `**evento.is_set()**`**:** comprueba si está activado.
    

## Reglas esenciales

- Usa `Event` para comunicar que una condición se ha cumplido o que una tarea puede continuar.
    
- Recuerda que `set()` mantiene la señal activada hasta llamar a `clear()`.
    
- No lo confundas con `Lock`: `Event` comunica una señal; `Lock` protege una sección crítica.
    

## Regla 80/20

Utiliza `asyncio.Event` para coordinar el inicio, la disponibilidad o la finalización de etapas entre tareas asíncronas.