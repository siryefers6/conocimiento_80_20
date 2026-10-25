# `asyncio.Queue` — Coordinar productores y consumidores asíncronos

Usa `asyncio.Queue` para transferir elementos entre corutinas sin bloquear el hilo. Permite implementar colas de trabajo con productores que agregan elementos y consumidores que los procesan.

## Ejemplo mínimo

```python
import asyncio

async def productor(cola):
    for dato in range(3):
        await cola.put(dato)
    await cola.put(None)  # Señal de finalización

async def consumidor(cola):
    while True:
        dato = await cola.get()
        try:
            if dato is None:
                return
            print(f"Procesando: {dato}")
        finally:
            cola.task_done()

async def main():
    cola = asyncio.Queue()

    async with asyncio.TaskGroup() as grupo:
        grupo.create_task(productor(cola))
        grupo.create_task(consumidor(cola))

    await cola.join()

asyncio.run(main())
```

## Métodos esenciales

- `**await cola.put(item)**`**:** agrega un elemento; espera si la cola está llena.
    
- `**await cola.get()**`**:** obtiene un elemento; espera si la cola está vacía.
    
- `**cola.task_done()**`**:** indica que un elemento obtenido terminó de procesarse.
    
- `**await cola.join()**`**:** espera hasta que todos los elementos agregados hayan sido marcados como procesados.
    
- `**asyncio.Queue(maxsize=N)**`**:** limita la capacidad y aplica contrapresión cuando se llena.
    

## Reglas esenciales

- Utiliza `asyncio.Queue` para distribuir trabajo entre tareas del mismo bucle de eventos.
    
- Ejecuta `task_done()` una vez por cada elemento obtenido, incluso si su procesamiento falla.
    
- Define un mecanismo explícito para detener los consumidores.
    
- No compartas `asyncio.Queue` directamente entre hilos; no es segura para uso entre hilos.
    

## Regla 80/20

Domina `put()`, `get()`, `task_done()` y `join()`. Añade una capacidad máxima cuando necesites controlar la acumulación de trabajo y evitar un consumo excesivo de memoria.