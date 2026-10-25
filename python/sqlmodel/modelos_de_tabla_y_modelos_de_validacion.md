# SQLModel: modelos de tabla y modelos de validación

**Referencia:** SQLModel 0.0.48 · 24-10-2026

## Modelo de tabla: `table=True`

Utiliza `table=True` para asociar una clase con una tabla de la base de datos.

```python
from sqlmodel import Field, SQLModel


class Producto(SQLModel, table=True):
    id: int | None = Field(default=None, primary_key=True)
    nombre: str
    precio: float
```

- Permite representar registros mediante objetos Python.
    
- Define columnas y claves primarias.
    
- Permite operaciones ORM mediante sesiones.
    

## Modelo de validación: sin `table=True`

Omite `table=True` para definir esquemas de entrada y salida sin crear una tabla asociada.

```python
class ProductoCrear(SQLModel):
    nombre: str
    precio: float
```

- Valida los datos recibidos.
    
- No representa una tabla ni permite persistir registros directamente mediante el ORM.
    
- Facilita separar los datos de entrada de los datos almacenados.
    

## Comparación

| Característica            | `table=True`  | Sin `table=True` |
| ------------------------- | ------------- | ---------------- |
| Asociado a una tabla      | Sí            | No               |
| Validación de datos       | Sí            | Sí               |
| Columnas SQL              | Sí            | No               |
| Persistencia mediante ORM | Sí            | No directamente  |
| Uso habitual              | Base de datos | API y validación |

## Regla práctica

Separa las responsabilidades en aplicaciones FastAPI:

```python
class Producto(SQLModel, table=True):
    id: int | None = Field(default=None, primary_key=True)
    nombre: str
    precio: float


class ProductoCrear(SQLModel):
    nombre: str
    precio: float
```

Utiliza `ProductoCrear` para recibir los datos y `Producto` para almacenarlos.

**Prioriza:** comprende esta separación antes de estudiar sesiones, operaciones CRUD y relaciones entre tablas.