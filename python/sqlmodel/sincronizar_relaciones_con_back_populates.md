# SQLModel: sincronizar relaciones con `back_populates`

**Referencia:** SQLModel 0.0.48 · 24-10-2026

## Concepto

Utiliza `back_populates` para vincular explícitamente los atributos de navegación de dos modelos relacionados. Permite que SQLAlchemy sincronice la asociación entre objetos en ambos sentidos.

## Ejemplo: pedido y bultos

```python
from sqlmodel import Field, Relationship, SQLModel


class Pedido(SQLModel, table=True):
    id: int | None = Field(default=None, primary_key=True)

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

## Cómo funciona

- `Pedido.bultos`: permite navegar desde un pedido hacia sus bultos.
    
- `Bulto.pedido`: permite navegar desde un bulto hacia su pedido.
    
- `back_populates="pedido"`: conecta `Pedido.bultos` con `Bulto.pedido`.
    
- `back_populates="bultos"`: establece el vínculo inverso.
    

Los nombres deben coincidir con los atributos definidos en el modelo contrario.

## Ejemplo de asociación

```python
pedido = Pedido()
bulto = Bulto()

pedido.bultos.append(bulto)

assert bulto.pedido is pedido
```

La sincronización entre objetos relacionados funciona dentro de la instrumentación ORM de SQLAlchemy.

## Reglas prácticas

- Define `back_populates` en ambos extremos de una relación bidireccional.
    
- No lo confundas con `foreign_key`: uno configura la navegación ORM y el otro la integridad referencial SQL.
    
- No asumas que `back_populates` guarda los objetos automáticamente; utiliza una sesión y confirma la transacción con `commit()`.
    

**Prioriza:** nombres de atributos, navegación bidireccional y diferencia entre asociar objetos en memoria y persistirlos en la base de datos.