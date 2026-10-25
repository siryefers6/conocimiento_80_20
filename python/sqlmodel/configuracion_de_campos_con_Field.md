# SQLModel: configuración de campos con `Field()`

**Referencia:** SQLModel 0.0.48 · 24-10-2026

## Concepto

Utiliza `Field()` para configurar valores predeterminados, claves primarias, restricciones, índices y metadatos de los campos de un modelo.

## Ejemplo práctico

```python
from sqlmodel import Field, SQLModel


class Producto(SQLModel, table=True):
    id: int | None = Field(
        default=None,
        primary_key=True,
    )
    nombre: str = Field(
        index=True,
        min_length=2,
        max_length=100,
    )
    precio: float = Field(gt=0)
    stock: int = Field(default=0, ge=0)
```

## Parámetros esenciales

| Parámetro                   | Función                                                       |
| --------------------------- | ------------------------------------------------------------- |
| `default`                   | Define el valor predeterminado en Python.                     |
| `default_factory`           | Genera un valor predeterminado mediante una función.          |
| `primary_key=True`          | Marca la clave primaria.                                      |
| `index=True`                | Solicita un índice para la columna.                           |
| `unique=True`               | Solicita valores únicos en la columna.                        |
| `nullable=False`            | Impide valores `NULL` en la columna SQL.                      |
| `min_length` / `max_length` | Valida la longitud del texto.                                 |
| `gt` / `ge`                 | Valida que un número sea mayor que un límite o mayor o igual. |
| `lt` / `le`                 | Valida que un número sea menor que un límite o menor o igual. |
| `description`               | Documenta el campo para esquemas y herramientas compatibles.  |
| `sa_column`                 | Permite configurar directamente una columna de SQLAlchemy.    |

## Valores predeterminados dinámicos

Utiliza `default_factory` para crear valores independientes por instancia.

```python
from datetime import datetime, timezone
from sqlmodel import Field, SQLModel


class Registro(SQLModel, table=True):
    id: int | None = Field(default=None, primary_key=True)
    fecha: datetime = Field(
        default_factory=lambda: datetime.now(timezone.utc)
    )
```

Evita evaluar `datetime.now()` directamente como valor predeterminado: se ejecutaría al definir la clase, no al crear cada instancia.

## Restricciones importantes

- Distingue entre validaciones de Pydantic y restricciones de la base de datos.
    
- No asumas que `gt`, `ge`, `min_length` o `max_length` crean restricciones SQL equivalentes.
    
- Utiliza migraciones o restricciones SQL explícitas cuando la integridad deba garantizarse en la base de datos.
    
- Define `nullable` y `default` según las necesidades reales del esquema.
    

**Prioriza:** `primary_key`, `default`, `default_factory`, `index`, `unique` y la diferencia entre validación de datos y restricciones SQL.