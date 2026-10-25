# `asyncio.Lock` — Proteger recursos compartidos

Usa `asyncio.Lock` para garantizar que una sola tarea asíncrona acceda a una sección crítica a la vez. Evita condiciones de carrera cuando varias tareas modifican un mismo recurso compartido.

## Ejemplo mínimo

```python
import asyncio

contador = 0
lock = asyncio.Lock()

async def incrementar():
    global contador

    async with lock:
        valor = contador
        await asyncio.sleep(0)
        contador = valor + 1

async def main():
    await asyncio.gather(
        *(incrementar() for _ in range(100))
    )
    print(contador)  # 100

asyncio.run(main())
```

## Reglas esenciales

- `**asyncio.Lock()**`**:** crea un bloqueo inicialmente libre.
    
- `**async with lock**`**:** adquiere el bloqueo y lo libera automáticamente al salir del bloque.
    
- `**await lock.acquire()**`**:** adquiere el bloqueo manualmente; libera con `lock.release()` en un bloque `finally`.
    
- **Sección crítica:** mantén dentro del bloqueo únicamente las operaciones que requieren exclusión mutua.
    
- **Alcance:** protege tareas del mismo entorno asíncrono; no es un bloqueo para coordinar hilos o procesos.
    

## Regla 80/20

Utiliza `asyncio.Lock` cuando varias tareas puedan leer y modificar un estado compartido. Prefiere `async with lock` para garantizar su liberación incluso si ocurre una excepción.