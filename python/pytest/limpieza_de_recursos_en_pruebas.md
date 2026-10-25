# Limpieza de recursos en pruebas con Pytest

Garantiza que conexiones, archivos y otros recursos se liberen incluso cuando una prueba falla o lanza una excepción.

## Usa `yield` en fixtures

Ejecuta la preparación antes de `yield` y la limpieza después. Pytest ejecuta la limpieza al finalizar la prueba.

```python
import pytest

@pytest.fixture
def archivo_temporal(tmp_path):
    archivo = tmp_path / "datos.txt"
    archivo.write_text("prueba")

    yield archivo

    archivo.unlink(missing_ok=True)
```

## Usa administradores de contexto

Prefiere `with` para cerrar automáticamente conexiones y archivos.

```python
def test_archivo(tmp_path):
    ruta = tmp_path / "datos.txt"

    with ruta.open("w") as archivo:
        archivo.write("contenido")

    assert ruta.read_text() == "contenido"
```

## Reglas

- Usa `yield` en fixtures para liberar recursos después de cada prueba.
    
- Prefiere `with` para gestionar archivos, conexiones y transacciones.
    
- Usa `try/finally` cuando necesites garantizar una limpieza explícita.
    
- No dependas de que la prueba termine correctamente.
    
- Recuerda: si una fixture falla antes de alcanzar `yield`, su bloque posterior no se ejecuta. Registra la limpieza de cada recurso tan pronto como lo crees.
    

**Prioriza:** `with` para recursos individuales y fixtures con `yield` para recursos compartidos entre los pasos de una prueba.