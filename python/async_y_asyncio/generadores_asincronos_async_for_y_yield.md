# Generadores asíncronos — `async for` y `yield`

Usa `async def` con `yield` para producir datos progresivamente y `async for` para consumirlos. Python no tiene una instrucción `async yield`: `yield` funciona dentro de una función declarada con `async def` para crear un generador asíncrono.

```python
import asyncio

async def generar_datos():
    for numero in range(3):
        await asyncio.sleep(1)
        yield numero

async def main():
    async for dato in generar_datos():
        print(dato)

asyncio.run(main())
```

- `**async def**` **+** `**yield**`**:** crea un generador asíncrono que produce valores progresivamente.
    
- `**async for**`**:** consume los valores sin bloquear el event loop mientras el generador espera operaciones asíncronas.
    
- **Procesamiento incremental:** evita acumular todos los resultados en una lista cuando puedes procesarlos uno a uno.
    
- **Finalización:** el generador termina cuando deja de producir valores o ejecuta `return`.
    
- **Uso habitual:** leer registros por lotes, consumir eventos o transmitir resultados progresivamente.
    

**Diferencia clave:** una corrutina devuelve un resultado mediante `return`; un generador asíncrono produce una secuencia de valores mediante `yield`.