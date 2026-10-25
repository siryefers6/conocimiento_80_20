# `asyncio.sleep()` — Suspender una tarea sin bloquear el bucle

Usa `await asyncio.sleep(segundos)` para suspender la corutina actual durante un intervalo y permitir que el bucle de eventos ejecute otras tareas.

## Ejemplo mínimo

```python
import asyncio

async def tarea(nombre, demora):
    print(f"Inicia {nombre}")
    await asyncio.sleep(demora)
    print(f"Termina {nombre}")

async def main():
    await asyncio.gather(
        tarea("A", 2),
        tarea("B", 1),
    )

asyncio.run(main())
```

Las tareas avanzan concurrentemente durante sus esperas.

## Reglas esenciales

- `**await asyncio.sleep(t)**`**:** suspende la tarea actual durante al menos el intervalo indicado, sujeto a la planificación.
    
- `**await asyncio.sleep(0)**`**:** cede el control para permitir que otras tareas avancen.
    
- **No bloqueante:** no detiene el hilo como `time.sleep()`.
    
- **Uso:** simular demoras, introducir pausas y permitir que otras tareas se ejecuten.
    

## Regla 80/20

Sustituye `time.sleep()` por `await asyncio.sleep()` dentro de corutinas cuando necesites esperar sin bloquear el bucle de eventos.