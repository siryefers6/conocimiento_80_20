# Alcance de fixtures (`scope`) — pytest

**Objetivo:** controla cuánto tiempo se conserva una fixture y cuándo se vuelve a ejecutar.

## Alcances disponibles

| `scope`    | Duración                                        |
| ---------- | ----------------------------------------------- |
| `function` | Una instancia por prueba. Valor predeterminado. |
| `class`    | Una instancia por clase de prueba.              |
| `module`   | Una instancia por archivo de pruebas.           |
| `package`  | Una instancia por paquete de pruebas.           |
| `session`  | Una instancia por sesión de pytest.             |

## Ejemplo

```python
import pytest


@pytest.fixture(scope="function")
def usuario():
    return {"nombre": "Ana"}


@pytest.fixture(scope="session")
def configuracion():
    return {"entorno": "test"}


def test_usuario(usuario, configuracion):
    assert usuario["nombre"] == "Ana"
    assert configuracion["entorno"] == "test"
```

## Criterios de selección

- `**function**`**:** úsalo por defecto para aislar pruebas y evitar estados compartidos.
    
- `**class**` **/** `**module**`**:** reutiliza recursos costosos dentro de una clase o archivo.
    
- `**package**`**:** comparte recursos entre módulos del mismo paquete de pruebas.
    
- `**session**`**:** inicializa recursos costosos una sola vez por sesión, como una conexión o configuración global.
    

## Reglas esenciales

- **Elige el alcance más corto posible** que satisfaga la necesidad.
    
- **Evita compartir datos mutables** entre pruebas si pueden alterar los resultados.
    
- **Respeta las dependencias:** una fixture de alcance amplio no puede depender de otra de alcance más corto.
    
- **Recuerda:** `session` no significa persistencia entre distintas ejecuciones de pytest.
    

**Referencia:** [pytest — Fixture scopes](https://docs.pytest.org/en/stable/how-to/fixtures.html#fixture-scopes)