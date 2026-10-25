# Parametrización (`@pytest.mark.parametrize`) — pytest

**Objetivo:** ejecuta una misma prueba con múltiples entradas y resultados esperados, evitando duplicar funciones de prueba.

## Uso básico

```python
import pytest


def sumar(a: int, b: int) -> int:
    return a + b


@pytest.mark.parametrize(
    "a, b, esperado",
    [
        (2, 3, 5),
        (0, 0, 0),
        (-1, 1, 0),
        (10, 5, 15),
    ],
)
def test_sumar(a, b, esperado):
    assert sumar(a, b) == esperado
```

## Parámetros esenciales

- `**@pytest.mark.parametrize**`: define los casos que debe ejecutar la prueba.
    
- **Primer argumento**: especifica los nombres de los parámetros separados por comas.
    
- **Segundo argumento**: proporciona los conjuntos de valores; cada tupla representa un caso.
    
- **Resultado**: pytest ejecuta una prueba independiente por cada conjunto de datos.
    

## Aplicación práctica

```python
@pytest.mark.parametrize(
    "nombre, valido",
    [
        ("Ana", True),
        ("", False),
        ("   ", False),
    ],
)
def test_validar_nombre(nombre, valido):
    assert validar_nombre(nombre) is valido
```

## Reglas esenciales

- **Agrupa casos relacionados** bajo una misma función.
    
- **Incluye casos normales, límites e inválidos**.
    
- **Usa nombres descriptivos** para identificar los casos fallidos.
    
- **Evita combinar escenarios con comportamientos distintos** en una sola parametrización.
    

**Referencia:** [pytest — Parametrization](https://docs.pytest.org/en/stable/how-to/parametrize.html)