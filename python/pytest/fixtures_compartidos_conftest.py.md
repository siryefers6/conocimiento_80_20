# Fixtures compartidos (`conftest.py`) — pytest

**Objetivo:** centraliza fixtures y utilidades de prueba para reutilizarlas automáticamente sin importarlas manualmente.

## Estructura recomendada

```
proyecto/
├── app/
│   └── usuarios.py
└── tests/
    ├── conftest.py
    ├── test_usuarios.py
    └── test_pedidos.py
```

## Definir fixtures compartidos

```python
# tests/conftest.py
import pytest


@pytest.fixture
def usuario():
    return {"id": 1, "nombre": "Ana"}


@pytest.fixture
def precio():
    return 100
```

## Utilizarlas en las pruebas

```python
# tests/test_usuarios.py

def test_nombre_usuario(usuario):
    assert usuario["nombre"] == "Ana"


def test_id_usuario(usuario):
    assert usuario["id"] == 1
```

Pytest descubre las fixtures de `conftest.py` automáticamente y las inyecta mediante los argumentos de las pruebas.

## Reglas esenciales

- **Centraliza** las fixtures comunes en `conftest.py`.
    
- **Evita importar** `conftest.py` directamente.
    
- **Mantén** las fixtures específicas dentro de sus módulos cuando no se compartan.
    
- **Organiza** varios archivos `conftest.py` por directorio si necesitas limitar el alcance de las fixtures.
    
- **No conviertas** `conftest.py` en un módulo genérico con utilidades que no sean necesarias para las pruebas.
    

**Referencia:** [pytest — Sharing fixtures across multiple files](https://docs.pytest.org/en/stable/how-to/fixtures.html#conftest-py-sharing-fixtures-across-multiple-files)