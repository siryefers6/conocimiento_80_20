# Configuración de Pytest en `pyproject.toml`

Centraliza las opciones de Pytest en `pyproject.toml` para mantener una configuración consistente sin repetir argumentos en cada ejecución.

## Configuración básica

```
[tool.pytest.ini_options]
testpaths = ["tests"]
python_files = ["test_*.py"]
addopts = "-ra"
markers = [
    "integration: pruebas de integración",
    "slow: pruebas de ejecución lenta",
]
```

- `testpaths`: directorios donde buscar pruebas.
    
- `python_files`: patrón de nombres de archivos.
    
- `addopts`: opciones predeterminadas para cada ejecución.
    
- `markers`: marcadores personalizados para clasificar pruebas.
    

## Ejecución selectiva

```sh
uv run pytest
uv run pytest tests/test_pedido.py
uv run pytest -m integration
uv run pytest -m "not slow"
uv run pytest -k "crear_pedido"
uv run pytest --collect-only
```

- `-m`: selecciona pruebas por marcador.
    
- `-k`: selecciona pruebas por nombre o expresión.
    
- `--collect-only`: muestra las pruebas detectadas sin ejecutarlas.
    

## Reglas

- Mantén la configuración común en `pyproject.toml`.
    
- Usa marcadores para separar pruebas unitarias, integración y lentas.
    
- Evita acumular opciones predeterminadas innecesarias en `addopts`.
    
- Registra los marcadores personalizados para evitar advertencias.
    

**Prioriza:** `testpaths`, `addopts`, `markers`, `-m` y `-k`. Consulta la [documentación oficial de configuración de Pytest](https://docs.pytest.org/en/stable/reference/customize.html#pyproject-toml).