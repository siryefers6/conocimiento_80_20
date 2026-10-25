# SQLModel: `Session` y `Session.exec()`

**Referencia:** SQLModel 0.0.48 · 24-10-2026

## Concepto

Utiliza `Session` para administrar las operaciones sobre la base de datos y `Session.exec()` para ejecutar consultas SQL o expresiones SQLAlchemy y obtener sus resultados.

## 1. Crear una sesión

```python
from sqlmodel import Session, create_engine

engine = create_engine("sqlite:///tienda.db")

with Session(engine) as session:
    ...
```

- `Session(engine)`: crea una sesión asociada al motor.
    
- `with`: garantiza el cierre de la sesión al salir del bloque.
    
- `session`: administra las consultas y los cambios pendientes.
    

## 2. Consultar registros

```python
from sqlmodel import select

with Session(engine) as session:
    statement = select(Producto)
    productos = session.exec(statement).all()
```

Utiliza `select()` para construir la consulta y `exec()` para ejecutarla.

| Método           | Función                               |
| ---------------- | ------------------------------------- |
| `.all()`         | Obtiene todos los resultados.         |
| `.first()`       | Obtiene el primer resultado o `None`. |
| `.one()`         | Exige exactamente un resultado.       |
| `.one_or_none()` | Permite cero o un resultado.          |

## 3. Insertar y confirmar cambios

```python
with Session(engine) as session:
    producto = Producto(nombre="Teclado", precio=25000)

    session.add(producto)
    session.commit()
    session.refresh(producto)
```

- `add()`: incorpora el objeto a la sesión.
    
- `commit()`: confirma la transacción.
    
- `refresh()`: actualiza el objeto con los datos persistidos, como un identificador generado por la base de datos.
    

## 4. Reglas esenciales

- Crea sesiones de corta duración, normalmente por petición en FastAPI.
    
- Confirma los cambios con `commit()`.
    
- Usa `rollback()` cuando debas revertir una transacción fallida.
    
- Cierra la sesión mediante `with Session(engine)`.
    
- No confundas `engine` —gestiona conexiones— con `Session` —gestiona operaciones y transacciones—.
    

**Prioriza:** domina `Session`, `select()`, `exec()`, `add()`, `commit()`, `refresh()` y `rollback()` antes de implementar CRUD completo.