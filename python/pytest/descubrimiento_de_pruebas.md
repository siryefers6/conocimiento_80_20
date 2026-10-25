# Descubrimiento de pruebas — pytest

**Objetivo:** permitir que `pytest` encuentre automáticamente los archivos, funciones y clases de prueba.

## Convenciones de nombres

```python
# Archivo: tests/test_usuario.py

def test_crear_usuario():
    assert 2 + 2 == 4


class TestUsuario:
    def test_nombre_valido(self):
        assert "Ana".isalpha()
```

- **Archivos:** utiliza `test_*.py` o `*_test.py`.
    
- **Funciones:** comienza los nombres con `test_`.
    
- **Clases:** utiliza el prefijo `Test` y evita definir `__init__`.
    
- **Métodos:** dentro de las clases, utiliza el prefijo `test_`.
    

## Ejecución

```sh
uv run pytest --collect-only
```

Inspecciona las pruebas descubiertas sin ejecutarlas.

```sh
uv run pytest tests/test_usuario.py
```

Ejecuta las pruebas de un archivo específico.

**Regla:** respeta las convenciones de nombres para evitar que las pruebas queden fuera de la ejecución automática.

**Referencia:** [pytest — Conventions for Python test discovery](https://docs.pytest.org/en/stable/explanation/goodpractices.html#test-discovery)