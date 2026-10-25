# SQLModel: cascadas y eliminación de relaciones

**Versión:** SQLModel 0.0.48 · **Referencia:** 24-10-2026

## Concepto

Controla qué ocurre con los registros relacionados cuando eliminas una entidad. Distingue entre **cascadas del ORM** y **reglas de eliminación de la base de datos**.

## 1. `ON DELETE CASCADE`

Elimina automáticamente los registros dependientes cuando se elimina el registro referenciado.

```python
from sqlmodel import SQLModel, Field


class Pedido(SQLModel, table=True):
    id: int | None = Field(default=None, primary_key=True)


class Bulto(SQLModel, table=True):
    id: int | None = Field(default=None, primary_key=True)
    pedido_id: int = Field(
        foreign_key="pedido.id",
        ondelete="CASCADE",
    )
```

La clave foránea establece la regla en la base de datos. En SQLite, activa también las claves foráneas mediante `PRAGMA foreign_keys=ON` para que se aplique.

## 2. Cascadas del ORM

Configura el comportamiento de las operaciones ejecutadas mediante `Session`.

```python
from sqlmodel import Relationship


class Pedido(SQLModel, table=True):
    id: int | None = Field(default=None, primary_key=True)

    bultos: list["Bulto"] = Relationship(
        back_populates="pedido",
        cascade_delete=True,
    )


class Bulto(SQLModel, table=True):
    id: int | None = Field(default=None, primary_key=True)
    pedido_id: int = Field(foreign_key="pedido.id")

    pedido: Pedido = Relationship(back_populates="bultos")
```

`cascade_delete=True` configura la eliminación de los objetos dependientes mediante el ORM. No reemplaza `ON DELETE CASCADE` en la base de datos.

## 3. Eliminar una entidad

```python
from sqlmodel import Session


with Session(engine) as session:
    pedido = session.get(Pedido, 1)

    if pedido:
        session.delete(pedido)
        session.commit()
```

Con la cascada del ORM configurada, al eliminar el pedido también se eliminan sus bultos asociados.

## Reglas prácticas

- `**cascade_delete=True**`**:** controla la eliminación en las operaciones del ORM.
    
- `**ondelete="CASCADE"**`**:** define la eliminación en la base de datos.
    
- `**ondelete="SET NULL"**`**:** establece la clave foránea como `NULL`; requiere que admita valores nulos.
    
- `**ondelete="RESTRICT"**`**:** impide eliminar el registro referenciado mientras existan dependencias.
    
- **Tablas intermedias:** al eliminar una relación muchos-a-muchos, normalmente elimina la fila de asociación, no las entidades relacionadas.
    
- **Migraciones:** modificar una clave foránea existente puede requerir reconstruir la tabla en SQLite o cambiar la restricción en PostgreSQL.
    

## Prioriza

1. Distingue las cascadas del ORM de las reglas de la base de datos.
    
2. Define qué registros deben eliminarse y cuáles deben conservarse.
    
3. Prueba las reglas con registros relacionados y verifica el comportamiento de SQLite y PostgreSQL.