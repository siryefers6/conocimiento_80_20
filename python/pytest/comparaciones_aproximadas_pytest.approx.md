# Comparaciones aproximadas (`pytest.approx`) — pytest

**Objetivo:** compara resultados numéricos con tolerancias para evitar fallos por diferencias de precisión en números de punto flotante.

## Uso básico

```python
import pytest


def test_division():
    resultado = 1 / 3

    assert resultado == pytest.approx(0.333333, abs=1e-6)
```

## Tolerancias

```python
# Tolerancia relativa (predeterminada: 1e-6)
assert 100.001 == pytest.approx(100, rel=1e-5)

# Tolerancia absoluta
assert 0.001 == pytest.approx(0, abs=0.002)

# Combinar ambas tolerancias
assert 10.01 == pytest.approx(10, rel=0.01, abs=0.001)
```

- `rel`: define la tolerancia relativa respecto al valor esperado.
    
- `abs`: define la tolerancia absoluta.
    
- `pytest.approx()`: acepta diferencias dentro de las tolerancias configuradas.
    

## Comparar colecciones

```python
assert [0.1 + 0.2, 2 / 3] == pytest.approx([0.3, 0.666667])
```

## Reglas esenciales

- **Usa** `**pytest.approx**` para resultados de cálculos con punto flotante.
    
- **Define** `**abs**` cuando necesites controlar la precisión cerca de cero.
    
- **Elige tolerancias justificadas** por los requisitos del cálculo.
    
- **Evita comparaciones aproximadas** para enteros, cadenas y valores que deban coincidir exactamente.
    

**Referencia:** [pytest —](https://docs.pytest.org/en/stable/reference/reference.html#pytest-approx) `[approx](https://docs.pytest.org/en/stable/reference/reference.html#pytest-approx)`