# SQLModel: consultas JOIN entre tablas

**Versión:** SQLModel 0.0.48 · **Referencia:** 24-10-2026

## Concepto

Usa `JOIN` para recuperar datos relacionados de varias tablas en una consulta SQL. En SQLModel, combina `select()`, `join()` y `Session.exec()`.

## Ejemplo: pedidos y clientes

```python
from sqlmodel import SQLModel, Field, Session, create_engine, select


class Cliente(SQLModel, table=True):
    id: int | None = Field(default=None, primary_key=True)
    nombre: str


class Pedido(SQLModel, table=True):
    id: int | None = Field(default=None, primary_key=True)
    cliente_id: int = Field(foreign_key="cliente.id")
    total: float


engine = create_engine("sqlite:///tienda.db")
SQLModel.metadata.create_all(engine)

with Session(engine) as session:
    consulta = (
        select(Pedido, Cliente)
        .join(Cliente, Pedido.cliente_id == Cliente.id)
    )

    for pedido, cliente in session.exec(consulta):
        print(cliente.nombre, pedido.total)
```

## Tipos de JOIN

- `**JOIN**` **/** `**INNER JOIN**`**:** devuelve solo registros con coincidencias en ambas tablas.
    
- `**LEFT JOIN**`**:** conserva todos los registros de la tabla izquierda, aunque no tengan coincidencia.
    
- `**OUTER JOIN**`**:** permite conservar registros sin coincidencia; concreta el tipo según necesites.
    

## Reglas prácticas

- Define la condición de unión explícitamente cuando haya ambigüedad: `.join(Cliente, Pedido.cliente_id == Cliente.id)`.
    
- Selecciona las entidades o columnas necesarias con `select()`.
    
- Usa `where()` para filtrar y `order_by()` para ordenar los resultados.
    
- Usa `JOIN` para construir consultas; usa `Relationship()` para navegar entre entidades relacionadas.
    
- Una clave foránea establece la relación en la base de datos, pero no sustituye la consulta `JOIN`.