# Omisión condicional en Pytest (`skip`, `skipif`)

Omite pruebas cuando no pueden ejecutarse o cuando sus requisitos previos no se cumplen. Pytest informa las pruebas omitidas en el resultado.

## 1. `pytest.mark.skip`

Omite una prueba siempre, por ejemplo, mientras una funcionalidad está pendiente.

```python
import pytest

@pytest.mark.skip(reason="Funcionalidad pendiente")
def test_exportar_informe():
    ...
```

## 2. `pytest.mark.skipif`

Omite una prueba cuando una condición es verdadera.

```python
import sys
import pytest

@pytest.mark.skipif(
    sys.platform == "win32",
    reason="No compatible con Windows",
)
def test_comando_linux():
    ...
```

## 3. `pytest.skip()`

Omite dinámicamente una prueba cuando detecta que falta un requisito durante su ejecución.

```python
import pytest

def test_api_externa():
    if not servicio_disponible():
        pytest.skip("Servicio externo no disponible")

    assert consultar_servicio() == "OK"
```

## Reglas

- Usa `skip` para exclusiones permanentes y explícitas.
    
- Usa `skipif` para condiciones conocidas antes de ejecutar la prueba.
    
- Usa `pytest.skip()` para requisitos detectados durante la ejecución.
    
- Incluye siempre un motivo claro.
    
- No uses omisiones para ocultar errores que deberían corregirse.
    

**Prioriza:** `skipif` para diferencias de entorno y `pytest.skip()` para dependencias que pueden no estar disponibles.