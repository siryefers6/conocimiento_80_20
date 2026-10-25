# Selección de pruebas con Pytest

Ejecuta únicamente las pruebas relevantes para reducir el tiempo de desarrollo y aislar errores.

## 1. Seleccionar por ruta

```sh
uv run pytest tests/test_pedido.py
uv run pytest tests/test_pedido.py::test_crear_pedido
uv run pytest tests/servicios/
```

- Archivo: ejecuta todas sus pruebas.
    
- Nodo `::`: ejecuta una función o clase específica.
    
- Directorio: ejecuta las pruebas contenidas en él.
    

## 2. Seleccionar por nombre con `-k`

```sh
uv run pytest -k "pedido"
uv run pytest -k "crear and not eliminar"
```

`-k` filtra por expresiones que coinciden con los nombres de pruebas, clases y archivos.

Operadores: `and`, `or`, `not`.

## 3. Seleccionar por marcador con `-m`

```python
import pytest

@pytest.mark.integration
def test_guardar_pedido():
    ...
```

```sh
uv run pytest -m integration
uv run pytest -m "not slow"
uv run pytest -m "integration and not slow"
```

`-m` selecciona pruebas etiquetadas con marcadores registrados en `pyproject.toml`.

## Reglas

- Usa rutas para limitar el alcance a un archivo, directorio o prueba.
    
- Usa `-k` para filtrar rápidamente por nombre.
    
- Usa `-m` para clasificar pruebas por tipo o coste.
    
- Combina filtros cuando necesites mayor precisión.
    

**Prioriza:** ruta específica para depurar, `-k` para búsquedas rápidas y `-m` para separar pruebas unitarias, integración y lentas.