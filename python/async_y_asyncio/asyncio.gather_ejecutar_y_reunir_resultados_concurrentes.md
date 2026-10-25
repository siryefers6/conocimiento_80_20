# `asyncio.gather()` — Ejecutar y reunir resultados concurrentes

Usa `asyncio.gather()` para ejecutar varias corutinas concurrentemente y obtener sus resultados en el orden de entrada, independientemente del orden en que terminen.

## Ejemplo mínimo

```python
import asyncio

async def obtener_dato(numero):
    await asyncio.sleep(1)
    return numero * 2

async def main():
    resultados = await asyncio.gather(
        obtener_dato(10),
        obtener_dato(20),
        obtener_dato(30),
    )
    print(resultados)  # [20, 40, 60]

asyncio.run(main())
```

## Reglas esenciales

- **Concurrencia:** permite que las operaciones avancen durante sus esperas.
    
- **Resultados:** devuelve una lista ordenada según el orden de entrada.
    
- **Errores:** por defecto, propaga la primera excepción que observa, pero no cancela automáticamente las demás operaciones.
    
- `**return_exceptions=True**`**:** devuelve las excepciones como elementos de la lista, en lugar de propagarlas directamente.
    
- **Comparación:** prefiere `asyncio.TaskGroup` cuando necesites un ciclo de vida estructurado y cancelación de tareas relacionadas ante errores.
    

## Regla 80/20

Utiliza `asyncio.gather()` para ejecutar varias operaciones independientes y recoger sus resultados en una sola espera. Evalúa el tratamiento de errores antes de usarlo en producción.