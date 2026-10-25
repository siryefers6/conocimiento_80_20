# Fixtures parametrizados — pytest

**Objetivo:** ejecuta una misma prueba con diferentes configuraciones o recursos mediante el parámetro `params` de `@pytest.fixture`.

## Uso básico

```python
import pytest


@pytest.fixture(params=[1, 2, 3])
def numero(request):
    return request.param


def test_numero_positivo(numero):
    assert numero > 0
```

Pytest ejecuta `test_numero_positivo` tres veces, una por cada valor. Accede al valor actual mediante `request.param`.

## Parametrizar configuraciones

```python
import pytest


@pytest.fixture(params=[
    ("admin", True),
    ("invitado", False),
])
def usuario(request):
    nombre, autorizado = request.param
    return {"nombre": nombre, "autorizado": autorizado}


def test_autorizacion(usuario):
    assert usuario["autorizado"] is (
        usuario["nombre"] == "admin"
    )
```

## Identificar escenarios

Añade `ids` para distinguir los casos en los informes.

```python
@pytest.fixture(
    params=["sqlite", "postgresql"],
    ids=["sqlite", "postgresql"],
)
def motor_bd(request):
    return request.param
```

## Reglas esenciales

- **Usa** `**params**` para reutilizar una fixture con múltiples variantes.
    
- **Accede al caso actual** mediante `request.param`.
    
- **Define** `**ids**` cuando los escenarios necesiten nombres descriptivos.
    
- **Prefiere** `**@pytest.mark.parametrize**` cuando solo cambien las entradas de una prueba; usa fixtures parametrizadas cuando cambie el recurso o la configuración preparada.
    

**Referencia:** [pytest — Parametrizing fixtures](https://docs.pytest.org/en/stable/how-to/fixtures.html#parametrizing-fixtures)