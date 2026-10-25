# SQLModel: anotaciones de tipos de Python

**Referencia:** SQLModel 0.0.48 · 24-10-2026

## Concepto

Utiliza las anotaciones de tipos para definir los campos, validar datos y determinar los tipos de columnas SQL. SQLModel combina las anotaciones de Python con Pydantic y SQLAlchemy.

## Tipos habituales

```python
from datetime import date, datetime

from sqlmodel import Field, SQLModel


class Producto(SQLModel, table=True):
    id: int | None = Field(default=None, primary_key=True)
    nombre: str
    cantidad: int
    precio: float
    disponible: bool = True
    fecha_registro: datetime
    fecha_vencimiento: date | None = None
```

| Anotación     | Uso habitual        |
| ------------- | ------------------- |
| `str`         | Texto               |
| `int`         | Números enteros     |
| `float`       | Números decimales   |
| `bool`        | Valores booleanos   |
| `datetime`    | Fecha y hora        |
| `date`        | Fecha sin hora      |
| `str \| None` | Texto opcional      |
| `int \| None` | Entero opcional     |
| `list[str]`   | Colección de textos |

## Opcionalidad y valores predeterminados

Distingue entre permitir `None` y asignar un valor predeterminado.

```python
descripcion: str | None
observaciones: str | None = None
cantidad: int = 0
```

- `str | None`: permite `None`, pero no establece por sí solo un valor predeterminado.
    
- `str | None = None`: permite omitir el campo y asignar `None`.
    
- `int = 0`: establece `0` como valor predeterminado.
    

## Colecciones

No asumas que `list[str]` crea automáticamente una columna SQL convencional.

```python
etiquetas: list[str] = []
```

Utiliza colecciones con modelos anidados para validar datos estructurados. Para persistir colecciones, define una estrategia compatible con SQLAlchemy, como relaciones entre tablas o tipos JSON, según el caso.

**Prioriza:** tipos escalares, campos opcionales, valores predeterminados y la diferencia entre tipos de validación y tipos de almacenamiento SQL.