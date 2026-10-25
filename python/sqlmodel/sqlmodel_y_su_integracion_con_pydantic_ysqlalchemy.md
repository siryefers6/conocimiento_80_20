# SQLModel y su integración con Pydantic y SQLAlchemy

**Referencia:** SQLModel 0.0.48 · 24-10-2026

## Concepto

Comprende cómo SQLModel combina dos tecnologías:

- **Pydantic:** valida datos y define esquemas de entrada y salida.
    
- **SQLAlchemy:** gestiona la persistencia relacional mediante un ORM.
    
- **SQLModel:** integra ambas capas mediante modelos Python que pueden utilizarse para validar datos y representar tablas de una base de datos.
    

## Ejemplo mínimo

```python
from sqlmodel import Field, SQLModel


class Producto(SQLModel, table=True):
    id: int | None = Field(default=None, primary_key=True)
    nombre: str
    precio: float
```

- `SQLModel`: proporciona la clase base.
    
- `table=True`: declara un modelo asociado a una tabla.
    
- `Field()`: configura columnas y restricciones.
    
- `id`: representa la clave primaria autogenerada por la base de datos.
    

## Modelo de tabla frente a esquema de datos

Separa los modelos de persistencia de los esquemas de entrada y salida cuando necesites controlar qué datos acepta o expone la API.

```python
class ProductoCrear(SQLModel):
    nombre: str
    precio: float


class ProductoPublico(SQLModel):
    id: int
    nombre: str
    precio: float
```

**Regla:** utiliza modelos de tabla para persistencia y modelos sin `table=True` para validar y transferir datos.

## Prioriza

1. Diferencia entre Pydantic, SQLAlchemy y SQLModel.
    
2. Diferencia entre modelos de tabla y modelos de datos.
    
3. Uso de `SQLModel`, `Field()` y `table=True`.
    
4. Separación entre datos recibidos, almacenados y devueltos por la API.