# `asyncio.wait_for()` y `asyncio.timeout()` — Limitar el tiempo de ejecución

Establece un tiempo máximo para completar una operación asíncrona y evita esperas indefinidas.

## `asyncio.wait_for()`

Limita una corutina o un objeto esperable. Si vence el plazo, cancela la operación y lanza `TimeoutError`.

```python
import asyncio

async def operacion():
    await asyncio.sleep(5)
    return "Completado"

async def main():
    try:
        resultado = await asyncio.wait_for(
            operacion(),
            timeout=2,
        )
        print(resultado)
    except TimeoutError:
        print("Tiempo agotado")

asyncio.run(main())
```

## `asyncio.timeout()`

Usa un administrador de contexto para limitar varias instrucciones dentro de un mismo bloque.

```python
async def main():
    try:
        async with asyncio.timeout(2):
            await operacion()
    except TimeoutError:
        print("Tiempo agotado")
```

## Diferencias esenciales

- `**asyncio.wait_for()**`**:** limita una operación concreta.
    
- `**asyncio.timeout()**`**:** limita un bloque completo de código asíncrono.
    
- `**TimeoutError**`**:** captura la excepción fuera del contexto o de la llamada correspondiente.
    
- **Cancelación:** ambos mecanismos utilizan la cancelación para interrumpir la espera; la operación puede tardar más en finalizar si debe procesar la cancelación.
    

## Regla 80/20

Usa `asyncio.wait_for()` para limitar una operación individual y `asyncio.timeout()` para establecer un plazo común para varias operaciones relacionadas.