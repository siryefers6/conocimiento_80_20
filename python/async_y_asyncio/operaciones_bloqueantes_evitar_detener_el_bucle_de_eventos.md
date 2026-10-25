# Operaciones bloqueantes — Evitar detener el bucle de eventos

Identifica las funciones síncronas que impiden al _event loop_ ejecutar otras tareas mientras esperan o procesan datos.

## Ejemplo incorrecto

```python
import asyncio
import time

async def tarea():
    time.sleep(2)  # Bloquea el bucle de eventos
    print("Finalizada")

asyncio.run(tarea())
```

`time.sleep(2)` bloquea el hilo durante dos segundos, impidiendo que otras tareas asíncronas avancen.

## Alternativa correcta

```python
import asyncio

async def tarea():
    await asyncio.sleep(2)  # Permite avanzar a otras tareas
    print("Finalizada")

asyncio.run(tarea())
```

## Cómo tratar operaciones bloqueantes

- **Esperas:** sustituye `time.sleep()` por `await asyncio.sleep()`.
    
- **E/S bloqueante:** ejecuta la función con `await asyncio.to_thread(funcion)`.
    
- **Cálculos intensivos de CPU:** considera `ProcessPoolExecutor` o `multiprocessing`.
    
- **Bibliotecas de red:** utiliza clientes con API asíncrona cuando estén disponibles.
    

## Regla 80/20

No ejecutes funciones bloqueantes directamente dentro de una corutina. Identifica las operaciones síncronas de archivos, red, esperas y CPU, y elige una alternativa adecuada para evitar bloquear el bucle de eventos.