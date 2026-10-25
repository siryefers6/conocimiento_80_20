# Excepciones asíncronas — Capturar y propagar errores en `asyncio`

Captura las excepciones esperables con `try/except` y permite que los errores inesperados se propaguen. Espera las tareas para recoger sus resultados y detectar sus fallos.

## Capturar errores en una corutina

```python
import asyncio

async def obtener_dato():
    raise ValueError("Dato inválido")

async def main():
    try:
        await obtener_dato()
    except ValueError as error:
        print(f"Error: {error}")

asyncio.run(main())
```

## Gestionar errores en tareas

```python
async def main():
    tarea = asyncio.create_task(obtener_dato())

    try:
        await tarea
    except ValueError as error:
        print(f"Tarea fallida: {error}")
```

## Reglas esenciales

- `**try/except**`**:** captura errores en el punto donde puedes gestionarlos.
    
- `**await tarea**`**:** recupera el resultado o propaga la excepción de la tarea.
    
- `**asyncio.TaskGroup**`**:** cancela las tareas restantes cuando una falla y propaga los errores mediante un grupo de excepciones.
    
- `**asyncio.gather()**`**:** propaga la primera excepción observada por defecto, sin cancelar automáticamente las demás tareas.
    
- `**asyncio.CancelledError**`**:** no la confundas con un error de negocio; permite que la cancelación se propague normalmente.
    
- **Tareas sin supervisión:** conserva referencias y gestiona sus resultados o excepciones.
    

## Regla 80/20

Espera las tareas, captura las excepciones que puedas resolver y garantiza la limpieza de recursos. Prefiere `asyncio.TaskGroup` cuando varias tareas formen una misma operación y deban gestionarse conjuntamente.