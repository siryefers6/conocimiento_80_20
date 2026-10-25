# Pydantic: validación y serialización de modelos

Distingue entre **validar datos de entrada** y **serializar modelos** para transmitirlos o almacenarlos.

## Validación de datos

Utiliza `model_validate()` para validar un diccionario y `model_validate_json()` para validar una cadena JSON.

```python
from pydantic import BaseModel

class Usuario(BaseModel):
    nombre: str
    edad: int

usuario = Usuario.model_validate({
    "nombre": "Ana",
    "edad": 25,
})

usuario2 = Usuario.model_validate_json(
    '{"nombre": "Ana", "edad": 25}'
)
```

Ambos métodos construyen una instancia validada de `Usuario`.

## Serialización de datos

Utiliza `model_dump()` para obtener un diccionario y `model_dump_json()` para generar una cadena JSON.

```python
datos = usuario.model_dump()
json_texto = usuario.model_dump_json()

print(datos)       # {'nombre': 'Ana', 'edad': 25}
print(json_texto)  # {"nombre":"Ana","edad":25}
```

## Métodos principales

| Método                       | Función                                   |
| ---------------------------- | ----------------------------------------- |
| `model_validate(datos)`      | Valida datos Python y crea una instancia. |
| `model_validate_json(texto)` | Valida JSON y crea una instancia.         |
| `model_dump()`               | Serializa el modelo a estructuras Python. |
| `model_dump_json()`          | Serializa el modelo a una cadena JSON.    |

## Opciones útiles

```python
usuario.model_dump(exclude_none=True)
usuario.model_dump(exclude_unset=True)
usuario.model_dump_json(indent=2)
```

- `exclude_none=True`: omite campos cuyo valor sea `None`.
    
- `exclude_unset=True`: omite campos que no se proporcionaron explícitamente.
    
- `indent=2`: genera JSON con sangría.
    

**Regla:** valida al recibir datos externos y serializa al producir una salida. No confundas un diccionario Python con una cadena JSON.

**Referencia:** Pydantic 2.14.1.