# Pydantic: configuración de modelos con `ConfigDict`

Configura el comportamiento de los modelos mediante `model_config`. Usa `extra="forbid"` para rechazar campos inesperados en entradas externas.

## Configuración básica

```python
from pydantic import BaseModel, ConfigDict

class Usuario(BaseModel):
    model_config = ConfigDict(
        extra="forbid",
        strict=True,
        frozen=True,
        validate_assignment=True,
    )

    nombre: str
    edad: int
```

## Opciones principales

| Opción                     | Comportamiento                                                                            |
| -------------------------- | ----------------------------------------------------------------------------------------- |
| `extra="forbid"`           | Rechaza campos no declarados.                                                             |
| `extra="ignore"`           | Ignora campos adicionales; es el valor predeterminado.                                    |
| `extra="allow"`            | Conserva campos adicionales.                                                              |
| `strict=True`              | Evita las conversiones laxas habituales.                                                  |
| `frozen=True`              | Impide reasignar atributos del modelo.                                                    |
| `populate_by_name=True`    | Permite validar mediante el nombre del campo y su alias, según la configuración de alias. |
| `validate_assignment=True` | Valida los valores cuando reasignas atributos.                                            |

## Ejemplo: rechazar campos inesperados

```python
from pydantic import ValidationError

try:
    Usuario(nombre="Ana", edad=25, admin=True)
except ValidationError as error:
    print(error)
```

El campo `admin` provoca un error porque no está declarado.

## Consideraciones

- **Prioriza** `**extra="forbid"**` en contratos de API que deban detectar errores tipográficos o propiedades inesperadas.
    
- No combines `frozen=True` con la expectativa de modificar atributos después de crear el modelo.
    
- `validate_assignment=True` no permite modificar atributos de un modelo congelado.
    
- En Pydantic 2.11+, prefiere `validate_by_name=True` y `validate_by_alias=True` para controlar ambos comportamientos explícitamente; `populate_by_name` se mantiene por compatibilidad.
    

**Regla:** configura cada modelo según su función y no permitas campos adicionales sin una razón explícita.

**Referencia:** Pydantic 2.14.1.