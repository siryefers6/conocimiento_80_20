# Debugging de `asyncio` — Detectar errores y tareas pendientes

Activa el modo de depuración y revisa las advertencias para detectar corutinas no esperadas, tareas sin finalizar y operaciones que bloquean el bucle de eventos.

## Activar el modo de depuración

Ejecuta la aplicación con `asyncio.run(main(), debug=True)` o configura la variable de entorno `PYTHONASYNCIODEBUG=1`.

```python
import asyncio

async def main():
    tarea = asyncio.create_task(asyncio.sleep(1))
    await tarea

asyncio.run(main(), debug=True)
```

## Problemas frecuentes

- **Corutina no esperada:** llamas a una función `async def` sin usar `await` ni programar la corutina como tarea.
    
- **Tarea pendiente:** una tarea sigue activa cuando la aplicación termina o se cierra el bucle.
    
- **Operación bloqueante:** una llamada síncrona, como `time.sleep()`, detiene temporalmente el bucle de eventos.
    
- **Excepción no recuperada:** una tarea falla y nadie consulta su resultado o excepción.
    

## Herramientas esenciales

- `**asyncio.run(main(), debug=True)**`**:** activa las comprobaciones de depuración.
    
- `**asyncio.all_tasks()**`**:** obtiene las tareas pendientes del bucle actual.
    
- `**task.done()**`**:** comprueba si una tarea terminó.
    
- `**task.exception()**`**:** consulta la excepción de una tarea finalizada que no fue cancelada.
    
- `**asyncio.get_running_loop().slow_callback_duration**`**:** ajusta el umbral para detectar callbacks lentos en modo de depuración.
    
- `**logging**`**:** registra errores, tiempos y eventos relevantes.
    

## Regla 80/20

Activa el modo de depuración durante el desarrollo, espera o cancela todas las tareas creadas y revisa las advertencias. Sustituye las operaciones bloqueantes por alternativas asíncronas o ejecútalas mediante `asyncio.to_thread()` cuando corresponda.