# SQLModel: relaciones uno-a-muchos y muchos-a-uno

**Referencia:** SQLModel 0.0.48 · 24-10-2026

## Concepto

Modela una relación **uno-a-muchos** cuando una entidad principal puede tener varios registros dependientes. Desde el lado dependiente, la misma asociación se interpreta como **muchos-a-uno**.

## 1. Define los modelos

```python
from sqlmodel import Field, Relationship, SQLModel


class Pedido(SQLModel, table=True):
    id: int | None = Field(default=None, primary_key=True)
    numero: str

    bultos: list["Bulto"] = Relationship(
        back_populates="pedido"
    )


class Bulto(SQLModel, table=True):
    id: int | None = Field(default=None, primary_key=True)
    pedido_id: int = Field(foreign_key="pedido.id")

    pedido: Pedido | None = Relationship(
        back_populates="bultos"
    )
```

- `Pedido.bultos`: representa el lado **uno-a-muchos**.
    
- `Bulto.pedido`: representa el lado **muchos-a-uno**.
    
- `pedido_id`: almacena la clave foránea.
    
- `back_populates`: vincula ambos atributos de navegación.
    

## 2. Crear registros relacionados

```python
with Session(engine) as session:
    pedido = Pedido(numero="PED-001")
    pedido.bultos.append(Bulto())
    pedido.bultos.append(Bulto())

    session.add(pedido)
    session.commit()
```

Añade el objeto principal y sus dependientes a la sesión. SQLAlchemy gestiona la persistencia de la asociación y la asignación de claves foráneas.

## 3. Consultar la relación

```python
with Session(engine) as session:
    pedido = session.get(Pedido, 1)

    if pedido is not None:
        print(len(pedido.bultos))

    bulto = session.get(Bulto, 1)

    if bulto is not None:
        print(bulto.pedido)
```

Accede a los dependientes mediante `pedido.bultos` y al principal mediante `bulto.pedido`.

## Reglas prácticas

- Define la clave foránea en la tabla dependiente.
    
- Utiliza `list[Modelo]` para representar una colección de objetos relacionados.
    
- Utiliza `Modelo | None` cuando el atributo de navegación pueda no tener un objeto asociado.
    
- Distingue entre `pedido_id`, que almacena la referencia, y `pedido`, que permite navegar por el ORM.
    
- Configura las reglas de eliminación según las necesidades del dominio.
    

**Prioriza:** ubicación de la clave foránea, cardinalidad, `Relationship()` y `back_populates`.