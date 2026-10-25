# SQLModel: tipos especiales y campos avanzados

**Versión:** SQLModel 0.0.48 · Pydantic 2.x · **Referencia:** 24-10-2026

## 1. UUID: identificadores únicos

Usa `UUID` para generar identificadores únicos sin depender de una secuencia numérica.

```python
from uuid import UUID, uuid4
from sqlmodel import SQLModel, Field


class Usuario(SQLModel, table=True):
    id: UUID = Field(default_factory=uuid4, primary_key=True)
    nombre: str
```

`default_factory=uuid4` genera un identificador nuevo para cada instancia.

## 2. Decimal: precisión monetaria

Usa `Decimal` para representar importes sin los errores de precisión típicos de `float`.

```python
from decimal import Decimal
from sqlmodel import SQLModel, Field
from sqlalchemy import Column, Numeric


class Producto(SQLModel, table=True):
    id: int | None = Field(default=None, primary_key=True)
    precio: Decimal = Field(
        sa_column=Column(Numeric(10, 2), nullable=False)
    )
```

- `Numeric(10, 2)`: permite hasta 10 dígitos, incluidos 2 decimales.
    
- Construye importes con cadenas, por ejemplo `Decimal("19.99")`.
    
- Define la escala y las reglas de redondeo según el negocio.
    

## 3. Fechas y horas

Usa `date` para fechas sin hora y `datetime` para instantes temporales.

```python
from datetime import date, datetime, timezone
from sqlmodel import SQLModel, Field


class Registro(SQLModel, table=True):
    id: int | None = Field(default=None, primary_key=True)
    fecha: date
    creado_en: datetime = Field(
        default_factory=lambda: datetime.now(timezone.utc)
    )
```

- `date`: representa una fecha de calendario.
    
- `datetime`: representa fecha y hora.
    
- `datetime.now(timezone.utc)`: genera una fecha y hora con zona horaria UTC.
    
- Mantén una política consistente de zonas horarias; no mezcles fechas conscientes de zona con fechas ingenuas sin una decisión explícita.
    

## 4. Enumeraciones: valores permitidos

Usa `Enum` para restringir un campo a un conjunto definido de opciones.

```python
from enum import Enum
from sqlmodel import SQLModel, Field


class Estado(str, Enum):
    pendiente = "pendiente"
    completado = "completado"
    cancelado = "cancelado"


class Pedido(SQLModel, table=True):
    id: int | None = Field(default=None, primary_key=True)
    estado: Estado = Field(default=Estado.pendiente)
```

La herencia de `str` y `Enum` facilita la validación y la interoperabilidad con JSON. SQLAlchemy gestiona la persistencia del tipo enumerado según el dialecto y su configuración.

## 5. Tipos SQLAlchemy personalizados

Usa `sa_type` cuando necesites especificar el tipo SQLAlchemy de una columna sin reemplazar toda la definición del campo.

```python
from sqlalchemy import String
from sqlmodel import SQLModel, Field


class Producto(SQLModel, table=True):
    id: int | None = Field(default=None, primary_key=True)
    codigo: str = Field(sa_type=String(32), index=True)
```

Usa `sa_column` cuando necesites controlar propiedades de la columna, como `nullable`, `unique`, `default` o `server_default`.

```python
from sqlalchemy import Column, String
from sqlmodel import SQLModel, Field


class Producto(SQLModel, table=True):
    id: int | None = Field(default=None, primary_key=True)
    codigo: str = Field(
        sa_column=Column(String(32), nullable=False, unique=True)
    )
```

**Regla:** `sa_type` especifica el tipo SQL; `sa_column` permite definir la columna completa. No los combines en el mismo `Field()`.

## Prioriza

1. Usa `UUID` para identificadores cuando necesites unicidad distribuida.
    
2. Usa `Decimal` y `Numeric` para importes monetarios.
    
3. Usa `date` y `datetime` según la precisión temporal requerida.
    
4. Usa `Enum` para estados y categorías con valores restringidos.
    
5. Aprende `sa_type` y `sa_column` para adaptar modelos a necesidades específicas del motor SQL.