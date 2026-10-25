# Marcadores de Pytest (`@pytest.mark`)

Clasifica las pruebas por tipo, coste o propósito para ejecutar subconjuntos mediante `-m`.

## 1. Registrar marcadores

Define los marcadores personalizados en `pyproject.toml`:

```
[tool.pytest.ini_options]
markers = [
    "unit: pruebas unitarias",
    "integration: pruebas de integración",
    "slow: pruebas de ejecución lenta",
]
```

## 2. Aplicar marcadores

```python
import pytest

@pytest.mark.unit
def test_calcular_total():
    assert 2 + 3 == 5

@pytest.mark.integration
def test_guardar_pedido():
    ...

@pytest.mark.slow
def test_proceso_completo():
    ...
```

Los marcadores personalizados sirven para clasificar pruebas; debes definir su significado y utilizarlos consistentemente.

## 3. Ejecutar por categoría

```sh
uv run pytest -m unit
uv run pytest -m integration
uv run pytest -m "not slow"
uv run pytest -m "unit or integration"
```

## Reglas

- Registra todos los marcadores propios para evitar advertencias.
    
- Usa `@pytest.mark.parametrize` para variar datos; no lo confundas con marcadores de clasificación.
    
- Aplica marcadores a clases o módulos cuando corresponda.
    
- No asumas que `unit`, `integration` o `slow` son marcadores incorporados: son categorías personalizadas.
    

**Prioriza:** `unit`, `integration` y `slow`, junto con la selección mediante `-m`.