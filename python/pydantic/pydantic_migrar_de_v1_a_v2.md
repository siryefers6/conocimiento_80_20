# Pydantic: migrar de v1 a v2

Utiliza la API de Pydantic v2 en código nuevo. Actualiza los métodos de modelos, validadores y configuración cuando migres proyectos existentes.

## Cambios principales

| Pydantic v1             | Pydantic v2                       |
| ----------------------- | --------------------------------- |
| `model.dict()`          | `model.model_dump()`              |
| `model.json()`          | `model.model_dump_json()`         |
| `Model.parse_obj(data)` | `Model.model_validate(data)`      |
| `Model.parse_raw(json)` | `Model.model_validate_json(json)` |
| `Model.schema()`        | `Model.model_json_schema()`       |
| `class Config:`         | `model_config = ConfigDict(...)`  |
| `@validator`            | `@field_validator`                |
| `@root_validator`       | `@model_validator`                |
| `__fields__`            | `model_fields`                    |

## Ejemplo de migración

**Antes (v1):**

```python
from pydantic import BaseModel, validator

class Usuario(BaseModel):
    nombre: str

    @validator("nombre")
    def validar_nombre(cls, valor):
        return valor.strip()
```

**Después (v2):**

```python
from pydantic import BaseModel, field_validator

class Usuario(BaseModel):
    nombre: str

    @field_validator("nombre")
    @classmethod
    def validar_nombre(cls, valor: str) -> str:
        return valor.strip()
```

## Reglas de migración

- Sustituye los métodos antiguos por sus equivalentes de v2.
    
- Cambia `Config` por `ConfigDict`.
    
- Revisa los validadores: sus firmas y modos de ejecución pueden haber cambiado.
    
- Comprueba el comportamiento de campos opcionales: en v2, `T | None` sin valor predeterminado sigue siendo obligatorio.
    
- Ejecuta las pruebas después de cada cambio y revisa las advertencias de deprecación.
    

**Regla:** escribe código nuevo con la API v2 y migra por etapas, verificando que la validación y la serialización mantengan el comportamiento esperado.