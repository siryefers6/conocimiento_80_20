# Pruebas asíncronas con `pytest-asyncio`

Prueba corutinas declarando los tests con `async def` y marcándolos con `@pytest.mark.asyncio`.

Instala las dependencias:

```sh
uv add --dev pytest pytest-asyncio
```

Configura `pyproject.toml`:

```
[tool.pytest.ini_options]
asyncio_mode = "auto"
```

Ejemplo de pruebas:

```python
import asyncio
import pytest

async def sumar(a: int, b: int) -> int:
    await asyncio.sleep(0)
    return a + b

async def fallar():
    raise ValueError("Error de prueba")

async def test_sumar():
    assert await sumar(2, 3) == 5

async def test_error():
    with pytest.raises(ValueError, match="Error de prueba"):
        await fallar()

async def test_concurrencia():
    resultados = await asyncio.gather(
        sumar(1, 2),
        sumar(3, 4),
    )
    assert resultados == [3, 7]
```

- **Corutinas:** usa `await` para ejecutar la operación probada.
    
- **Errores:** utiliza `pytest.raises()` para comprobar excepciones.
    
- **Concurrencia:** verifica resultados, límites y comportamiento ante fallos.
    
- **Recursos asíncronos:** crea fixtures con `@pytest_asyncio.fixture` cuando necesites configurar y cerrar clientes, sesiones o conexiones.
    
- **Ejecución:** ejecuta `uv run pytest`.
    

Usa `asyncio_mode = "auto"` para que `pytest-asyncio` gestione automáticamente los tests asíncronos compatibles con su configuración.