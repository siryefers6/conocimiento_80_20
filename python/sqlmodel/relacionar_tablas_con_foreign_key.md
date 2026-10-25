# SQLModel: relacionar tablas con `foreign_key`

**Referencia:** SQLModel 0.0.48 · 24-10-2026

## Concepto

Utiliza `foreign_key` para vincular una columna con la clave primaria de otra tabla y garantizar la integridad referencial.

## Ejemplo: pedidos y bultos

```python
from sqlmodel import Field, SQLModel


class Pedido(SQLModel, table=True):
    id: int | None = Field(default=None, primary_key=True)
    numero: str


class Bulto(SQLModel, table=True):
    id: int | None = Field(default=None, primary_key=True)
    pedido_id: int = Field(foreign_key="pedido.id")
```

- `foreign_key="pedido.id"`: referencia la columna `id` de la tabla `pedido`.
    
- `pedido_id`: almacena el identificador del pedido asociado.
    
- La clave foránea impide referencias a pedidos inexistentes cuando la restricción está habilitada en la base de datos.
    

## Insertar registros relacionados

```python
with Session(engine) as session:
    pedido = Pedido(numero="PED-001")
    session.add(pedido)
    session.commit()
    session.refresh(pedido)

    bulto = Bulto(pedido_id=pedido.id)
    session.add(bulto)
    session.commit()
```

Crea primero el pedido para obtener su identificador y después registra el bulto asociado.

## Reglas prácticas

- Escribe la referencia como `"nombre_tabla.nombre_columna"`.
    
- Asegúrate de que la tabla referenciada exista.
    
- Activa y verifica las restricciones de claves foráneas en el motor de base de datos; SQLite requiere habilitarlas explícitamente por conexión.
    
- Usa `Relationship()` cuando necesites navegar entre objetos relacionados; `foreign_key` por sí solo define la restricción, no el atributo de navegación.
    
- Define las reglas de eliminación según el dominio para evitar borrar registros relacionados accidentalmente.
    

**Prioriza:** claves primarias, claves foráneas, integridad referencial y diferencia entre `Field(foreign_key=...)` y `Relationship()`.