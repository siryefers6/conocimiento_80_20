# SQLModel: eliminar registros con `session.delete()`

**Referencia:** SQLModel 0.0.48 · 24-10-2026

## Concepto

Utiliza `session.delete()` para marcar un registro persistido para su eliminación y `session.commit()` para confirmar el cambio en la base de datos.

## Ejemplo funcional

```python
from sqlmodel import Session


def eliminar_producto(engine, producto_id: int) -> bool:
    with Session(engine) as session:
        producto = session.get(Producto, producto_id)

        if producto is None:
            return False

        session.delete(producto)
        session.commit()
        return True
```

## Flujo de eliminación

1. Recupera el registro mediante `session.get()`.
    
2. Comprueba si existe.
    
3. Ejecuta `session.delete(producto)`.
    
4. Confirma la transacción con `session.commit()`.
    
5. Devuelve el resultado de la operación.
    

## Manejo de errores

```python
with Session(engine) as session:
    try:
        producto = session.get(Producto, producto_id)

        if producto is not None:
            session.delete(producto)
            session.commit()
    except Exception:
        session.rollback()
        raise
```

Revierte la transacción ante un error y propaga la excepción para que la aplicación la gestione.

## Reglas prácticas

- Comprueba la existencia del registro antes de eliminarlo.
    
- No confundas `delete()` con `commit()`: el primero prepara la eliminación; el segundo confirma la transacción.
    
- Gestiona las restricciones de claves foráneas.
    
- Evalúa la eliminación lógica cuando necesites conservar el historial de registros.
    

**Prioriza:** existencia del registro, confirmación transaccional y restricciones de integridad.