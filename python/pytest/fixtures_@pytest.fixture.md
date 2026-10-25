# Fixtures (`@pytest.fixture`) — pytest

**Objetivo:** prepara y reutiliza datos, objetos y recursos necesarios para ejecutar pruebas sin duplicar código.

## Uso básico

```python
import pytest


@pytest.fixture
def usuario():
    return {"nombre": "Ana", "activo": True}


def test_nombre_usuario(usuario):
    assert usuario["nombre"] == "Ana"


def test_usuario_activo(usuario):
    assert usuario["activo"] is True
```

Pytest detecta el parámetro `usuario`, ejecuta la fixture y entrega su resultado a cada prueba que la solicita.

## Preparar y limpiar recursos

Utiliza `yield` para liberar recursos después de la prueba.

```python
import pytest


@pytest.fixture
def archivo_temporal(tmp_path):
    archivo = tmp_path / "datos.txt"
    archivo.write_text("contenido", encoding="utf-8")

    yield archivo

    # Limpieza adicional, si es necesaria
    archivo.unlink(missing_ok=True)


def test_leer_archivo(archivo_temporal):
    assert archivo_temporal.read_text(encoding="utf-8") == "contenido"
```

## Alcance (`scope`)

Define cuánto tiempo se conserva una instancia de la fixture.

| `scope`    | Duración                          |
| ---------- | --------------------------------- |
| `function` | Por prueba; valor predeterminado. |
| `class`    | Por clase de prueba.              |
| `module`   | Por módulo de pruebas.            |
| `session`  | Durante toda la sesión.           |

```python
@pytest.fixture(scope="module")
def configuracion():
    return {"entorno": "test"}
```

## Reglas esenciales

- **Reutiliza fixtures** para datos y recursos compartidos.
    
- **Usa** `**yield**` cuando necesites ejecutar tareas de limpieza.
    
- **Prefiere** `**function**` para aislar las pruebas.
    
- **Evita compartir estado mutable** entre pruebas sin una razón concreta.
    
- **Usa** `**tmp_path**` para crear archivos temporales aislados.
    

**Referencia:** [pytest — Fixtures](https://docs.pytest.org/en/stable/how-to/fixtures.html)