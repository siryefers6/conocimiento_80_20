# SQLModel 0.0.48: restricciones de integridad

**Referencia:** SQLModel 0.0.48 · SQLAlchemy 2.x · 24-10-2026

## 1. Claves únicas (`UNIQUE`)

Impide almacenar valores duplicados en una columna o combinación de columnas.

```python
from sqlmodel import SQLModel, Field

class Embalaje(SQLModel, table=True):
    id: int | None = Field(default=None, primary_key=True)
    codigo: str = Field(unique=True, index=True)
```

Usa `unique=True` para valores únicos en una columna. Para combinaciones de columnas, utiliza `UniqueConstraint`.

## 2. Restricciones `CHECK`

Obliga a que los datos cumplan una condición definida por la base de datos.

```python
from sqlalchemy import CheckConstraint
from sqlmodel import SQLModel, Field

class Producto(SQLModel, table=True):
    __table_args__ = (
        CheckConstraint("precio >= 0", name="ck_producto_precio"),
    )

    id: int | None = Field(default=None, primary_key=True)
    precio: float
```

Usa `CHECK` para reglas como cantidades no negativas o rangos válidos. No reemplaza la validación de entradas con Pydantic.

## 3. Claves foráneas (`FOREIGN KEY`)

Garantiza que una referencia apunte a un registro existente.

```python
from sqlmodel import SQLModel, Field

class Pedido(SQLModel, table=True):
    id: int | None = Field(default=None, primary_key=True)

class Bulto(SQLModel, table=True):
    id: int | None = Field(default=None, primary_key=True)
    pedido_id: int = Field(foreign_key="pedido.id", index=True)
```

La columna `pedido_id` debe referenciar un `Pedido` existente. La clave foránea no implica por sí sola que SQLModel elimine objetos relacionados en cascada.

## 4. Restricciones compuestas

Usa `UniqueConstraint` cuando la combinación de columnas deba ser única.

```python
from sqlalchemy import UniqueConstraint

__table_args__ = (
    UniqueConstraint(
        "pedido_id",
        "numero_bulto",
        name="uq_bulto_pedido_numero",
    ),
)
```

Incluye esta declaración dentro de la clase de tabla que contiene ambas columnas.

## 5. Integridad en SQLite y PostgreSQL

- **SQLite:** activa las claves foráneas mediante `PRAGMA foreign_keys=ON` en cada conexión; no des por hecho que están habilitadas.
    
- **PostgreSQL:** las claves foráneas se aplican normalmente, salvo que se altere explícitamente su comportamiento.
    
- Los dos motores pueden diferir en tipos, valores nulos y evaluación de restricciones.
    
- Las restricciones nuevas sobre tablas existentes requieren migraciones; modificar el modelo no actualiza automáticamente el esquema.
    

## 6. Buenas prácticas

- Define restricciones en la base de datos para proteger la integridad incluso si los datos llegan desde otra aplicación.
    
- Usa nombres explícitos: `uq_`, `ck_` y `fk_`.
    
- Valida también con Pydantic para ofrecer errores claros antes de insertar.
    
- Gestiona `IntegrityError` y ejecuta `session.rollback()` cuando falle una transacción.
    
- Prueba duplicados, referencias inexistentes, valores fuera de rango y valores nulos.
    

**Orden recomendado:** `UNIQUE` → `CHECK` → `FOREIGN KEY` → restricciones compuestas → errores de integridad y migraciones.

**Prioriza:** `UNIQUE`, `CHECK` y `FOREIGN KEY`. Son la última barrera de protección de los datos, incluso cuando la validación de la aplicación falla.

**Referencias:**

- [SQLModel: campos y claves foráneas](https://sqlmodel.tiangolo.com/tutorial/connect/create-connected-tables/)
    
- [SQLAlchemy: restricciones de esquema](https://docs.sqlalchemy.org/en/20/core/constraints.html)
    
- [SQLite: restricciones](https://www.sqlite.org/lang_createtable.html#constraints)