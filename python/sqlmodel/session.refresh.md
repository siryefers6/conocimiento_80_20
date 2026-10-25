# SQLModel: `session.refresh()`

**Referencia:** SQLModel 0.0.48 · 24-10-2026

## Concepto

Utiliza `session.refresh(objeto)` para volver a consultar un registro y actualizar los atributos del objeto con los valores actuales de la base de datos.

## Ejemplo práctico

```python
from sqlmodel import Field, Session, SQLModel, create_engine


class Producto(SQLModel, table=True):
    id: int | None = Field(default=None, primary_key=True)
    nombre: str


engine = create_engine("sqlite:///tienda.db")
SQLModel.metadata.create_all(engine)

with Session(engine) as session:
    producto = Producto(nombre="Teclado")

    session.add(producto)
    session.commit()
    session.refresh(producto)

    print(producto.id)
```

Después de guardar el objeto, `refresh()` permite recuperar el identificador generado por la base de datos y otros valores persistidos.

## Diferencias esenciales

| Método      | Función                                                                       |
| ----------- | ----------------------------------------------------------------------------- |
| `add()`     | Añade el objeto a la sesión.                                                  |
| `flush()`   | Envía los cambios pendientes a la base de datos sin confirmar la transacción. |
| `commit()`  | Confirma la transacción.                                                      |
| `refresh()` | Recarga los atributos desde la base de datos.                                 |

## Reglas prácticas

- Ejecuta `refresh()` después de `commit()` cuando necesites recuperar valores generados o actualizados por la base de datos.
    
- No lo confundas con `commit()`: actualizar el objeto no confirma una transacción.
    
- Evita consultas adicionales si no necesitas los valores actualizados.
    
- Mantén la sesión abierta mientras necesites refrescar el objeto.
    

**Prioriza:** comprende cuándo la base de datos genera valores automáticamente y cuándo necesitas recuperarlos mediante `refresh()`.