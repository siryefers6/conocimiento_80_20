# Datos temporales (`monkeypatch`) — pytest

**Objetivo:** modifica atributos, variables de entorno, diccionarios o funciones durante una prueba y restaura automáticamente el estado al finalizarla.

## Modificar atributos y funciones

```python
# app/configuracion.py
def obtener_entorno():
    return "produccion"
```

```python
# tests/test_configuracion.py
import app.configuracion as config


def test_entorno_pruebas(monkeypatch):
    monkeypatch.setattr(
        config, "obtener_entorno", lambda: "test"
    )

    assert config.obtener_entorno() == "test"
```

## Variables de entorno

```python
import os


def test_variable_entorno(monkeypatch):
    monkeypatch.setenv("APP_ENV", "test")

    assert os.getenv("APP_ENV") == "test"
```

## Diccionarios y eliminación de atributos

```python
def test_configuracion(monkeypatch):
    config = {"debug": False}

    monkeypatch.setitem(config, "debug", True)
    assert config["debug"] is True

    monkeypatch.delitem(config, "debug")
    assert "debug" not in config
```

## Métodos esenciales

- `setattr()`: sustituye temporalmente un atributo.
    
- `setenv()` / `delenv()`: modifica o elimina variables de entorno.
    
- `setitem()` / `delitem()`: modifica o elimina elementos de un diccionario.
    
- `syspath_prepend()`: añade temporalmente una ruta a `sys.path`.
    
- `undo()`: revierte explícitamente los cambios pendientes; normalmente no lo necesitas.
    

## Reglas esenciales

- **Usa** `**monkeypatch**` para aislar dependencias externas y evitar modificar el entorno real.
    
- **Aplica los cambios antes de ejecutar** la operación que quieres probar.
    
- **Parchea el nombre donde se utiliza**, no necesariamente donde se definió originalmente.
    
- **Prefiere** `**monkeypatch**` **sobre restauraciones manuales** para mantener las pruebas aisladas.
    

**Referencia:** [pytest — Monkeypatch](https://docs.pytest.org/en/stable/how-to/monkeypatch.html)