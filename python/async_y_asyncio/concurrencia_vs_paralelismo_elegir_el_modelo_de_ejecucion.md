# Concurrencia vs. paralelismo — Elegir el modelo de ejecución en Python

## Concurrencia con `asyncio`

Alterna tareas durante las esperas de E/S, como solicitudes HTTP, consultas de red o lectura asíncrona de datos.

```python
import asyncio

async def tarea(nombre):
    await asyncio.sleep(1)
    print(nombre)

async def main():
    await asyncio.gather(tarea("A"), tarea("B"))

asyncio.run(main())
```

Las tareas pueden avanzar concurrentemente sin necesitar varios hilos.

## Paralelismo con hilos y procesos

- `**asyncio**`**:** gestiona tareas asíncronas, principalmente operaciones de E/S.
    
- `**threading**`**:** permite ejecutar operaciones bloqueantes en hilos distintos; resulta útil para E/S bloqueante.
    
- `**multiprocessing**`**:** ejecuta trabajo en procesos separados; resulta útil para cálculos intensivos de CPU.
    
- `**asyncio.to_thread()**`**:** permite ejecutar una función bloqueante en un hilo sin bloquear el bucle de eventos.
    

## Regla 80/20

| Tipo de trabajo                    | Herramienta recomendada             |
| ---------------------------------- | ----------------------------------- |
| Muchas solicitudes HTTP asíncronas | `asyncio`                           |
| E/S bloqueante                     | `asyncio.to_thread()` o `threading` |
| Cálculos intensivos de CPU         | `multiprocessing`                   |

**Clave:** la concurrencia aprovecha los tiempos de espera; el paralelismo ejecuta trabajo simultáneamente. `asyncio` por sí solo no ejecuta código de CPU en paralelo.