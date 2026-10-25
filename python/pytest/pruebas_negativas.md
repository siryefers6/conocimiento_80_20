# Pruebas negativas — pytest

**Objetivo:** verifica que el sistema rechace entradas inválidas, aplique las reglas de negocio y controle los valores límite.

## Ejemplo con parametrización

```python
import pytest


def validar_edad(edad: int) -> bool:
    if not 0 <= edad <= 120:
        raise ValueError("Edad fuera de rango")
    return edad >= 18


@pytest.mark.parametrize("edad", [-1, 121, 999])
def test_edad_fuera_de_rango(edad):
    with pytest.raises(ValueError, match="fuera de rango"):
        validar_edad(edad)


@pytest.mark.parametrize(
    "edad, esperado",
    [(0, False), (17, False), (18, True), (120, True)],
)
def test_limites_de_edad(edad, esperado):
    assert validar_edad(edad) is esperado
```

## Categorías esenciales

- **Entradas inválidas:** cadenas vacías, tipos incorrectos o formatos mal construidos.
    
- **Reglas de negocio:** operaciones prohibidas, duplicados o estados incompatibles.
    
- **Límites:** valores mínimos, máximos y valores inmediatamente fuera del rango permitido.
    
- **Excepciones:** comprueba el tipo de error y, cuando corresponda, su mensaje.
    

## Reglas esenciales

- **Define el comportamiento esperado** antes de escribir cada prueba.
    
- **Prueba ambos lados del límite:** valor válido y valor inválido.
    
- **Usa** `**@pytest.mark.parametrize**` para cubrir múltiples escenarios sin duplicar código.
    
- **Evita aceptar cualquier excepción:** verifica el tipo concreto esperado.
    
- **Comprueba que el error no modifique datos** cuando la operación deba ser atómica.
    

**Referencia:** [pytest — Parametrization](https://docs.pytest.org/en/stable/how-to/parametrize.html)