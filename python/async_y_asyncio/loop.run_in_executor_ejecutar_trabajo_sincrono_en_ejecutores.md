# `loop.run_in_executor()` — Ejecutar trabajo síncrono en ejecutores

Usa `loop.run_in_executor()` para ejecutar funciones síncronas en un ejecutor de hilos o procesos sin bloquear el bucle de eventos.

## Ejemplo con hilos

```python
import asyncio
import time
from concurrent.futures import ThreadPoolExecutor

def operacion():
    time.sleep(2)
    return "Completado"

async def main():
    loop = asyncio.get_running_loop()

    with ThreadPoolExecutor(max_workers=4) as executor:
        resultado = await loop.run_in_executor(
            executor, operacion
        )
        print(resultado)

asyncio.run(main())
```

## Ejemplo con procesos

```python
import asyncio
from concurrent.futures import ProcessPoolExecutor

def calcular(numero):
    return numero ** 2

async def main():
    loop = asyncio.get_running_loop()

    with ProcessPoolExecutor() as executor:
        resultado = await loop.run_in_executor(
            executor, calcular, 10
        )
        print(resultado)  # 100

if __name__ == "__main__":
    asyncio.run(main())
```

## Reglas esenciales

- `**ThreadPoolExecutor**`**:** integra E/S bloqueante y funciones que necesitan ejecutarse en hilos.
    
- `**ProcessPoolExecutor**`**:** distribuye cálculos intensivos de CPU entre procesos.
    
- `**asyncio.get_running_loop()**`**:** obtiene el bucle de eventos activo.
    
- **Argumentos:** pásalos después de la función; para argumentos con nombre, utiliza `functools.partial()`.
    
- **Ciclo de vida:** gestiona el ejecutor y espera los resultados antes de finalizar.
    

## Diferencia con `asyncio.to_thread()`

- Usa `asyncio.to_thread()` para ejecutar funciones de E/S bloqueante con menos código.
    
- Usa `loop.run_in_executor()` cuando necesites elegir explícitamente el ejecutor, configurar sus trabajadores o utilizar procesos.
    

## Regla 80/20

Elige un ejecutor de hilos para E/S bloqueante y uno de procesos para cálculos intensivos de CPU. Mantén el bucle de eventos libre de trabajo síncrono prolongado.