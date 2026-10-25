# Inyección de dependencias — pytest

**Objetivo:** solicita fixtures declarando sus nombres como parámetros de la función de prueba. Pytest las resuelve y entrega sus valores automáticamente.

## Uso básico

```python
import pytest


@pytest.fixture
def usuario():
    return {"nombre": "Ana", "activo": True}


@pytest.fixture
def usuario_activo(usuario):
    return usuario["activo"]


def test_usuario_activo(usuario, usuario_activo):
    assert usuario["nombre"] == "Ana"
    assert usuario_activo is True
```

## Dependencias entre fixtures

- **Declara el nombre** de cada fixture como parámetro.
    
- **Solicita fixtures desde otras fixtures** para componer preparaciones reutilizables.
    
- **Deja que pytest resuelva las dependencias** y gestione su ciclo de vida.
    
- **Evita invocar fixtures directamente** como funciones; solicita sus valores mediante parámetros.
    

## Ejemplo con dependencias encadenadas

```python
@pytest.fixture
def pedido():
    return {"total": 100}


@pytest.fixture
def pedido_con_descuento(pedido):
    pedido["total"] *= 0.9
    return pedido


def test_total_con_descuento(pedido_con_descuento):
    assert pedido_con_descuento["total"] == 90
```

## Regla esencial

**Inyecta únicamente las fixtures necesarias** para mantener las pruebas explícitas, aisladas y fáciles de mantener.

**Referencia:** [pytest — Fixtures](https://docs.pytest.org/en/stable/how-to/fixtures.html)