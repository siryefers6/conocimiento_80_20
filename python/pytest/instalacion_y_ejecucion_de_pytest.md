# Instalación y ejecución de pytest

Instala `pytest` en el entorno virtual del proyecto y ejecuta las pruebas desde la raíz del repositorio.

## Instalación

```sh
uv add --dev pytest
```

## Ejecución

```sh
# Ejecutar todas las pruebas
uv run pytest

# Mostrar cada prueba ejecutada
uv run pytest -v

# Ejecutar un archivo específico
uv run pytest tests/test_main.py

# Detenerse en el primer fallo
uv run pytest -x
```

## Estructura mínima

```
proyecto/
├── pyproject.toml
├── src/
└── tests/
    └── test_main.py
```

## Reglas esenciales

- Usa `uv run pytest` para ejecutar las pruebas dentro del entorno del proyecto.
    
- Nombra los archivos `test_*.py` o `*_test.py`.
    
- Nombra las funciones `test_*`.
    
- Ejecuta las pruebas desde la raíz para mantener una configuración coherente.
    

## Referencia

- Documentación oficial: [Getting Started — pytest](https://docs.pytest.org/en/stable/getting-started.html?utm_source=chatgpt.com)