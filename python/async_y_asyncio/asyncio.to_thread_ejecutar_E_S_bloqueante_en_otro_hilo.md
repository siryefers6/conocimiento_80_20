# `asyncio.to_thread()` — Ejecutar E/S bloqueante en otro hilo

Usa `asyncio.to_thread()` para ejecutar una función síncrona bloqueante en un hilo separado y evitar que detenga el bucle de eventos.

## Ejemplo mínimo

```python
import asyncio
import time

def leer_archivo():
    time.sleep(2)  # Simula E/S bloqueante
    return "Datos leídos"

async def main():
    resultado = await asyncio.to_thread(leer_archivo)
    print(resultado)

asyncio.run(main())
```

## Reglas esenciales

- `**asyncio.to_thread(funcion, *args)**`**:** ejecuta una función síncrona en un hilo del grupo de trabajadores.
    
- `**await**`**:** espera el resultado sin bloquear el bucle de eventos.
    
- **Uso recomendado:** operaciones de archivos, bibliotecas HTTP síncronas y otras funciones de E/S bloqueante.
    
- **CPU intensiva:** no es la opción habitual para cálculos intensivos en Python debido al GIL; considera `ProcessPoolExecutor`.
    
- **Cancelación:** cancelar la tarea que espera el resultado no detiene automáticamente la función que ya se está ejecutando en el hilo.
    

## Regla 80/20

Utiliza `asyncio.to_thread()` para integrar funciones de E/S bloqueante en aplicaciones asíncronas sin detener el bucle de eventos.