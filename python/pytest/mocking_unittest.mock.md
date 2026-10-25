# Mocking (`unittest.mock`) — pytest

**Objetivo:** sustituye dependencias externas para aislar la unidad bajo prueba y verificar las llamadas realizadas sin ejecutar servicios reales.

## Uso básico con `patch`

```python
# app/notificaciones.py
from app.email import enviar_email


def registrar_usuario(email: str) -> None:
    enviar_email(email, "Registro completado")
```

```python
# tests/test_notificaciones.py
from unittest.mock import patch
from app.notificaciones import registrar_usuario


def test_enviar_notificacion():
    with patch("app.notificaciones.enviar_email") as mock_email:
        registrar_usuario("ana@example.com")

    mock_email.assert_called_once_with(
        "ana@example.com",
        "Registro completado",
    )
```

**Importante:** aplica `patch` sobre el nombre utilizado por el módulo probado, no necesariamente sobre el módulo donde se definió la función.

## Configurar resultados y errores

```python
from unittest.mock import Mock


servicio = Mock()
servicio.obtener.return_value = {"id": 1}
assert servicio.obtener() == {"id": 1}

servicio.obtener.side_effect = RuntimeError("Error externo")
```

- `return_value`: define el resultado simulado.
    
- `side_effect`: provoca una excepción o define resultados sucesivos.
    
- `assert_called_once_with()`: verifica que se realizó exactamente una llamada con los argumentos esperados.
    
- `assert_not_called()`: comprueba que no se realizaron llamadas.
    

## Reglas esenciales

- **Simula únicamente dependencias externas** cuando necesites aislar el comportamiento.
    
- **Verifica llamadas** solo cuando formen parte del comportamiento esperado.
    
- **Prefiere pruebas reales** para la lógica que puedas ejecutar de forma rápida y fiable.
    
- **Usa** `**with patch(...)**` para limitar el reemplazo al contexto necesario.
    
- **Evita simular demasiados detalles internos**; acopla las pruebas a comportamientos observables.
    

**Referencia:** [Python —](https://docs.python.org/3/library/unittest.mock.html) `[unittest.mock](https://docs.python.org/3/library/unittest.mock.html)`