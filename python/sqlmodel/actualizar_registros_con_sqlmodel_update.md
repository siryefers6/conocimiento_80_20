# SQLModel: actualizar registros con `sqlmodel_update()`

**Referencia:** SQLModel 0.0.48 · 24-10-2026

## Concepto

Combina `model_validate()` y `sqlmodel_update()` para validar los datos de entrada y aplicar cambios a un registro existente sin reemplazarlo por una instancia nueva.

## 1. Define los modelos

```python
from sqlmodel import Field, SQLModel


class ProductoBase(SQLModel):
    nombre: str
    precio: float


class Producto(ProductoBase, table=True):
    id: int | None = Field(default=None, primary_key=True)


class ProductoActualizar(SQLModel):
    nombre: str | None = None
    precio: float | None = None
```

## 2. Aplica una actualización parcial

```python
with Session(engine) as session:
    producto = session.get(Producto, 1)

    if producto is not None:
        datos = {"precio": 29990}
        producto.sqlmodel_update(datos)
        session.add(producto)
        session.commit()
        session.refresh(producto)
```

`sqlmodel_update()` modifica los atributos del objeto existente. No confirma la transacción automáticamente.

## 3. Valida los datos antes de actualizar

```python
datos = {"precio": 29990}

actualizacion = ProductoActualizar.model_validate(datos)

cambios = actualizacion.model_dump(exclude_unset=True)
producto.sqlmodel_update(cambios)
```

- `model_validate()`: valida los datos según el modelo.
    
- `model_dump(exclude_unset=True)`: conserva únicamente los campos enviados.
    
- `sqlmodel_update()`: aplica los cambios al objeto existente.
    

## Reglas prácticas

- Usa `exclude_unset=True` para no sobrescribir campos omitidos.
    
- Distingue un campo omitido de uno enviado explícitamente como `None`.
    
- No permitas actualizar campos protegidos, como `id`, mediante datos arbitrarios.
    
- Comprueba que el registro exista antes de modificarlo.
    
- Ejecuta `commit()` para persistir los cambios.
    

**Prioriza:** validación de entrada, actualizaciones parciales y separación entre los campos omitidos y los enviados.