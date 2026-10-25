# Ciclo de una prueba — pytest

**Objetivo:** estructura cada prueba en tres fases: preparar los datos, ejecutar la operación y comprobar el resultado.

## Patrón Arrange–Act–Assert (AAA)

1. **Arrange (Preparar):** define los datos y las condiciones iniciales.
    
2. **Act (Ejecutar):** invoca la función que quieres probar.
    
3. **Assert (Comprobar):** verifica el resultado esperado mediante `assert`.
    

## Ejemplo

```python
# app/calculadora.py
def sumar(a: int, b: int) -> int:
    return a + b
```

```python
# tests/test_calculadora.py
from app.calculadora import sumar


def test_sumar_dos_numeros():
    # Arrange
    a, b = 2, 3

    # Act
    resultado = sumar(a, b)

    # Assert
    assert resultado == 5
```

## Reglas esenciales

- **Separa las tres fases** para identificar rápidamente el origen de un fallo.
    
- **Prueba un comportamiento concreto** por función de prueba.
    
- **Evita lógica innecesaria** dentro de las pruebas.
    
- **No repitas preparaciones complejas**; utiliza fixtures cuando varias pruebas compartan datos o recursos.
    

**Referencia:** [pytest — Getting Started](https://docs.pytest.org/en/stable/getting-started.html)