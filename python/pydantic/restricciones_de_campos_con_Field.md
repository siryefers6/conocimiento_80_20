# Pydantic: restricciones de campos con `Field`

Aplica restricciones declarativas para validar longitudes, rangos numéricos, patrones y precisión decimal.

## Ejemplo práctico

```python
from decimal import Decimal
from pydantic import BaseModel, Field

class Producto(BaseModel):
    nombre: str = Field(min_length=3, max_length=50)
    codigo: str = Field(pattern=r"^[A-Z]{3}\d{4}$")
    cantidad: int = Field(ge=1, le=100)
    precio: Decimal = Field(gt=0, max_digits=8, decimal_places=2)
    descuento: float = Field(ge=0, lt=1)
```

## Restricciones principales

| Parámetro        | Validación                                |
| ---------------- | ----------------------------------------- |
| `min_length`     | Longitud mínima de cadenas o colecciones. |
| `max_length`     | Longitud máxima de cadenas o colecciones. |
| `ge`             | Mayor o igual que (`>=`).                 |
| `le`             | Menor o igual que (`<=`).                 |
| `gt`             | Estrictamente mayor que (`>`).            |
| `lt`             | Estrictamente menor que (`<`).            |
| `pattern`        | Coincidencia con una expresión regular.   |
| `max_digits`     | Máximo de dígitos de un decimal.          |
| `decimal_places` | Máximo de cifras decimales.               |

## Comprueba la validación

```python
from pydantic import ValidationError

try:
    Producto(
        nombre="PC",
        codigo="abc1234",
        cantidad=0,
        precio="12.345",
        descuento=1,
    )
except ValidationError as error:
    print(error)
```

Pydantic rechaza los valores que incumplen las restricciones.

**Reglas:**

- Usa `Decimal` para importes que requieren precisión decimal.
    
- Define límites coherentes con las reglas del dominio.
    
- Usa `pattern` para validar formatos, no para sustituir reglas de negocio complejas.
    
- Recuerda que `max_digits` y `decimal_places` controlan aspectos distintos de un número decimal.
    

**Versión de referencia:** Pydantic 2.14.1 · 24-10-2026.