# Fixtures con `yield` — pytest

**Objetivo:** prepara recursos antes de la prueba y libéralos después de su ejecución, incluso cuando la prueba falla.

## Estructura básica

```python
import pytest


@pytest.fixture
def recurso():
    # Preparación
    conexion = abrir_conexion()

    yield conexion

    # Limpieza
    conexion.close()
```

- **Antes de** `**yield**`**:** prepara el recurso.
    
- `**yield**`**:** entrega el recurso a la prueba.
    
- **Después de** `**yield**`**:** ejecuta la limpieza cuando pytest finaliza la prueba.
    

## Ejemplo funcional

```python
import pytest


@pytest.fixture
def archivo_temporal(tmp_path):
    archivo = tmp_path / "datos.txt"
    archivo.write_text("contenido", encoding="utf-8")

    yield archivo

    archivo.unlink(missing_ok=True)


def test_archivo(archivo_temporal):
    assert archivo_temporal.read_text(encoding="utf-8") == "contenido"
```

`tmp_path` crea un directorio temporal aislado para cada prueba. Pytest administra su ciclo de vida; la eliminación explícita del archivo es opcional en este ejemplo.

## Reglas esenciales

- **Usa** `**yield**` para gestionar conexiones, archivos y otros recursos que requieran limpieza.
    
- **Coloca la limpieza después de** `**yield**`.
    
- **Utiliza** `**try/finally**` cuando necesites garantizar la liberación de recursos dentro de una operación de limpieza compleja.
    
- **Recuerda:** si la preparación falla antes de `yield`, el código posterior no se ejecuta; las fixtures anteriores que ya se hayan preparado sí se limpian según su ciclo de vida.
    

**Referencia:** [pytest — Fixtures: teardown y yield](https://docs.pytest.org/en/stable/how-to/fixtures.html#yield-fixtures-recommended)