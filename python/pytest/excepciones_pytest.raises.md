# Excepciones (`pytest.raises`) — pytest

**Objetivo:** verifica que una operación genere la excepción esperada y, cuando corresponda, comprueba su mensaje.

## Uso básico

```python
import pytest


def dividir(a: float, b: float) -> float:
    if b == 0:
        raise ValueError("El divisor no puede ser cero")
    return a / b


def test_dividir_por_cero():
    with pytest.raises(ValueError):
        dividir(10, 0)
```

## Comprobar el mensaje

```python
def test_mensaje_de_error():
    with pytest.raises(ValueError, match="divisor no puede ser cero"):
        dividir(10, 0)
```

- `pytest.raises(ValueError)`: exige la excepción indicada o una subclase.
    
- `match`: comprueba el mensaje mediante una expresión regular.
    
- Si no se produce la excepción esperada, la prueba falla.
    

## Capturar la excepción

```python
def test_excepcion_capturada():
    with pytest.raises(ValueError) as error:
        dividir(10, 0)

    assert str(error.value) == "El divisor no puede ser cero"
```

**Reglas esenciales**

- Comprueba excepciones concretas; evita `Exception` salvo que sea necesario.
    
- Incluye dentro de `pytest.raises` únicamente la operación que debe fallar.
    
- Verifica el mensaje cuando forme parte del comportamiento esperado.
    

**Referencia:** [pytest — Assertions about expected exceptions](https://docs.pytest.org/en/stable/how-to/assert.html#assertions-about-expected-exceptions)