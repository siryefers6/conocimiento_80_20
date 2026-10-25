# `asyncio.Condition` — Esperar a que se cumpla una condición

Usa `asyncio.Condition` para coordinar tareas que comparten un estado y deben esperar hasta que se cumpla una condición específica. Combina un bloqueo con mecanismos para suspender y despertar tareas.

## Ejemplo mínimo

```python
import asyncio

async def consumidor(condicion, estado):
    async with condicion:
        await condicion.wait_for(lambda: estado["listo"])
        print("Condición cumplida")

async def productor(condicion, estado):
    await asyncio.sleep(1)

    async with condicion:
        estado["listo"] = True
        condicion.notify_all()

async def main():
    condicion = asyncio.Condition()
    estado = {"listo": False}

    async with asyncio.TaskGroup() as grupo:
        grupo.create_task(consumidor(condicion, estado))
        grupo.create_task(productor(condicion, estado))

asyncio.run(main())
```

## Métodos esenciales

- `**async with condicion**`**:** adquiere el bloqueo asociado.
    
- `**await condicion.wait()**`**:** libera temporalmente el bloqueo y espera una notificación; lo readquiere antes de continuar.
    
- `**await condicion.wait_for(predicado)**`**:** espera hasta que el predicado sea verdadero.
    
- `**condicion.notify(n)**`**:** despierta hasta `n` tareas que estén esperando.
    
- `**condicion.notify_all()**`**:** despierta a todas las tareas que esperan.
    

## Reglas esenciales

- Comprueba siempre la condición después de despertar; una notificación no garantiza que siga siendo verdadera.
    
- Modifica el estado compartido y notifica mientras mantienes adquirido el bloqueo.
    
- Usa `asyncio.Event` para señales simples y `asyncio.Condition` para condiciones vinculadas a un estado compartido.
    

## Regla 80/20

Utiliza `asyncio.Condition` cuando una tarea deba esperar hasta que cambie un estado compartido, como la disponibilidad de elementos o la liberación de capacidad.