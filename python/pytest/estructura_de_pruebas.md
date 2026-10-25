# Estructura de pruebas — pytest

**Objetivo:** organiza las pruebas por módulo, funcionalidad y caso de uso para facilitar su mantenimiento.

## Estructura recomendada

```
proyecto/
├── app/
│   ├── usuarios.py
│   └── pedidos.py
└── tests/
    ├── test_usuarios.py
    └── test_pedidos.py
```

- **Archivo:** agrupa las pruebas de un módulo o componente.
    
- **Función:** verifica un comportamiento específico.
    
- **Caso de uso:** separa los escenarios válidos, inválidos y límite.
    

## Ejemplo

```python
# tests/test_usuarios.py

from app.usuarios import validar_nombre


def test_nombre_valido():
    assert validar_nombre("Ana") is True


def test_nombre_vacio():
    assert validar_nombre("") is False


def test_nombre_con_espacios():
    assert validar_nombre("   ") is False
```

## Organización por funcionalidad

Agrupa las pruebas relacionadas con una misma operación y utiliza nombres que describan el comportamiento esperado.

```sh
# Ejecutar todas las pruebas
uv run pytest

# Ejecutar un módulo
uv run pytest tests/test_usuarios.py

# Ejecutar pruebas por nombre
uv run pytest -k "nombre_vacio"
```

## Reglas esenciales

- **Agrupa por responsabilidad**, no por cantidad de pruebas.
    
- **Mantén las pruebas fuera del código de producción**, dentro de `tests/`.
    
- **Describe el comportamiento esperado** en cada nombre.
    
- **Evita jerarquías de carpetas innecesarias** en proyectos pequeños.
    

**Referencia:** [pytest — Good Practices](https://docs.pytest.org/en/stable/explanation/goodpractices.html)