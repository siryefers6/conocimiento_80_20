# SQLModel: context managers para sesiones

**Referencia:** SQLModel 0.0.48 · 24-10-2026

## Concepto

Utiliza `with Session(engine) as session` para gestionar el ciclo de vida de una sesión y garantizar su cierre al salir del bloque, incluso si ocurre una excepción.

## Ejemplo básico

```python
from sqlmodel import Session

with Session(engine) as session:
    productos = session.exec(select(Producto)).all()
```

Al finalizar el bloque, la sesión se cierra y libera sus recursos. **No confundas el cierre con la confirmación de cambios:** cerrar una sesión no sustituye a `commit()`.

## Insertar datos y confirmar

```python
with Session(engine) as session:
    producto = Producto(nombre="Teclado", precio=25000)
    session.add(producto)
    session.commit()
    session.refresh(producto)
```

- `add()`: prepara el objeto para persistirlo.
    
- `commit()`: confirma la transacción.
    
- `refresh()`: actualiza el objeto con los datos de la base de datos.
    
- `with`: garantiza el cierre de la sesión.
    

## Gestionar errores

```python
with Session(engine) as session:
    try:
        session.add(producto)
        session.commit()
    except Exception:
        session.rollback()
        raise
```

Revierte la transacción con `rollback()` cuando falle una operación y propaga la excepción para no ocultar el error.

## Reglas prácticas

- Abre una sesión por unidad de trabajo.
    
- Cierra la sesión automáticamente mediante `with`.
    
- Confirma explícitamente las escrituras.
    
- Revierte las transacciones fallidas.
    
- No compartas una misma sesión entre solicitudes concurrentes.
    

**Prioriza:** cierre automático, confirmación explícita y recuperación mediante `rollback()`.