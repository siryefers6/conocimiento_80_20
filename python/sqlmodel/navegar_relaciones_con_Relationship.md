# SQLModel: navegar relaciones con `Relationship()`

**Referencia:** SQLModel 0.0.48 · 24-10-2026

## Concepto

Utiliza `Relationship()` para acceder a objetos relacionados mediante atributos Python. Combínalo con `Field(foreign_key=...)`, que define la clave foránea en la base de datos.

## 1. Relación uno a muchos

Un pedido puede contener varios bultos.

```python
from sqlmodel import Field, Relationship, SQLModel


class Pedido(SQLModel, table=True):
    id: int | None = Field(default=None, primary_key=True)
    numero: str

    bultos: list["Bulto"] = Relationship(back_populates="pedido")


class Bulto(SQLModel, table=True):
    id: int | None = Field(default=None, primary_key=True)
    pedido_id: int = Field(foreign_key="pedido.id")

    pedido: Pedido | None = Relationship(back_populates="bultos")
```

- `Relationship()`: define la navegación ORM entre objetos.
    
- `back_populates`: sincroniza los atributos de navegación en ambos modelos.
    
- `pedido_id`: almacena la referencia real mediante una clave foránea.
    
- `bultos`: permite acceder a los bultos asociados a un pedido.
    
- `pedido`: permite acceder al pedido asociado a un bulto.
    

## 2. Consultar objetos relacionados

```python
from sqlmodel import Session

with Session(engine) as session:
    pedido = session.get(Pedido, 1)

    if pedido is not None:
        for bulto in pedido.bultos:
            print(bulto.id)
```

Accede a los objetos relacionados mediante atributos, sin construir manualmente la consulta de unión.

## 3. Diferencia entre `Field()` y `Relationship()`

| Elemento                 | Responsabilidad                                       |
| ------------------------ | ----------------------------------------------------- |
| `Field(foreign_key=...)` | Define la referencia en la base de datos.             |
| `Relationship()`         | Define la navegación entre objetos ORM.               |
| `back_populates`         | Vincula los atributos de navegación de ambos modelos. |

## Reglas prácticas

- Define la clave foránea en el modelo que contiene la referencia.
    
- Configura `Relationship()` en ambos modelos cuando necesites navegación bidireccional.
    
- Importa todos los modelos antes de crear las tablas.
    
- Recuerda que acceder a relaciones puede ejecutar consultas adicionales; controla la carga de datos cuando trabajes con colecciones grandes.
    

**Prioriza:** relaciones uno a muchos, `back_populates` y la diferencia entre la integridad referencial y la navegación ORM.