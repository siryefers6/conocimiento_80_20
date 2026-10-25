# SQLModel: transacciones y `commit()`

**Referencia:** SQLModel 0.0.48 · 24-10-2026

## Concepto

Utiliza las transacciones para agrupar operaciones de base de datos y mantener la integridad de los datos. Ejecuta `session.commit()` para confirmar los cambios pendientes de una sesión.

## 1. Confirmar cambios

```python
with Session(engine) as session:
    producto = Producto(nombre="Teclado", precio=25000)
    session.add(producto)
    session.commit()
    session.refresh(producto)
```

- `add()`: agrega el objeto a la sesión.
    
- `commit()`: confirma la transacción.
    
- `refresh()`: recupera los valores actualizados desde la base de datos.
    

## 2. Revertir cambios ante errores

```python
with Session(engine) as session:
    try:
        session.add(producto)
        session.commit()
    except Exception:
        session.rollback()
        raise
```

- `rollback()`: revierte los cambios pendientes de la transacción fallida.
    
- `raise`: propaga el error para que la aplicación lo gestione.
    
- No captures excepciones sin registrarlas o gestionarlas adecuadamente.
    

## 3. Agrupar operaciones en una transacción

```python
with Session(engine) as session:
    try:
        session.add(producto_a)
        session.add(producto_b)
        session.commit()
    except Exception:
        session.rollback()
        raise
```

Confirma ambas inserciones como una unidad: si la transacción falla, ninguna de las dos debe quedar confirmada.

## 4. Reglas esenciales

- Confirma explícitamente las escrituras con `commit()`.
    
- Ejecuta `rollback()` después de un error transaccional antes de reutilizar la sesión.
    
- No confundas `flush()` con `commit()`: `flush()` envía operaciones a la base de datos sin confirmar la transacción.
    
- Usa `with Session(engine)` para garantizar el cierre de la sesión.
    
- Evita confirmar cada operación por separado cuando deban formar una única unidad atómica.
    

**Prioriza:** `commit()`, `rollback()`, `flush()` y la atomicidad de las operaciones.