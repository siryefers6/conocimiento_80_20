# SQLModel 0.0.48: pruebas de persistencia

**Referencia:** SQLModel 0.0.48 · Pytest · 24-10-2026

## 1. Usa una base de datos aislada

Crea un motor exclusivo para pruebas y genera las tablas antes de cada caso o conjunto de pruebas.

```python
import pytest
from sqlmodel import SQLModel, Session, create_engine

@pytest.fixture
def engine():
    engine = create_engine(
        "sqlite://",
        connect_args={"check_same_thread": False},
        poolclass=StaticPool,
    )
    SQLModel.metadata.create_all(engine)

    yield engine

    SQLModel.metadata.drop_all(engine)
    engine.dispose()
```

Importa `StaticPool` desde `sqlalchemy.pool`. Con SQLite en memoria, permite que las sesiones compartan la misma conexión y base de datos.

## 2. Prueba restricciones de integridad

Comprueba que la base de datos rechace datos inválidos aunque se omita la validación de la aplicación.

- Claves únicas duplicadas.
    
- Claves foráneas inexistentes.
    
- Restricciones `CHECK` incumplidas.
    
- Columnas obligatorias con valores nulos.
    

```python
import pytest
from sqlalchemy.exc import IntegrityError
from sqlmodel import Session

def test_codigo_unico(engine):
    with Session(engine) as session:
        session.add(Embalaje(codigo="ABC"))
        session.commit()

        session.add(Embalaje(codigo="ABC"))

        with pytest.raises(IntegrityError):
            session.commit()

        session.rollback()
```

Adapta `Embalaje` a tu modelo. En SQLite, habilita `PRAGMA foreign_keys=ON` para cada conexión si vas a probar claves foráneas.

## 3. Prueba transacciones y rollback

Verifica que una operación fallida no deje cambios parciales.

```python
from sqlmodel import Session, select

def test_rollback_no_persiste(engine):
    with Session(engine) as session:
        session.add(Producto(nombre="Temporal"))
        session.flush()
        session.rollback()

    with Session(engine) as session:
        resultado = session.exec(
            select(Producto).where(Producto.nombre == "Temporal")
        ).first()

        assert resultado is None
```

`flush()` envía operaciones a la base de datos sin confirmar la transacción; `rollback()` revierte los cambios pendientes.

## 4. Prueba relaciones

Comprueba que las claves foráneas y las relaciones funcionen en ambos sentidos cuando corresponda.

- Crear un registro relacionado.
    
- Consultar la relación desde el padre y el hijo.
    
- Rechazar referencias inexistentes.
    
- Verificar las reglas de eliminación y cascada.
    
- Confirmar que las relaciones sobrevivan a cerrar y abrir una sesión.
    

No asumas que `Relationship()` crea restricciones de integridad ni que eliminar un objeto elimina automáticamente sus relaciones.

## 5. Prueba persistencia real

No compruebes únicamente los objetos en memoria. Confirma los cambios, cierra la sesión y consulta de nuevo.

```python
def test_persistencia(engine):
    with Session(engine) as session:
        producto = Producto(nombre="Caja")
        session.add(producto)
        session.commit()
        producto_id = producto.id

    with Session(engine) as session:
        recuperado = session.get(Producto, producto_id)
        assert recuperado is not None
        assert recuperado.nombre == "Caja"
```

## 6. Compara SQLite y PostgreSQL

Mantén las mismas pruebas funcionales y ejecútalas contra ambos motores.

| Área          | Qué verificar                                     |
| ------------- | ------------------------------------------------- |
| Tipos         | Fechas UTC, decimales, UUID y valores nulos.      |
| Restricciones | `UNIQUE`, `CHECK` y claves foráneas.              |
| Transacciones | `commit()`, `rollback()` y errores.               |
| Relaciones    | Carga, integridad y eliminaciones.                |
| Concurrencia  | Escrituras simultáneas y bloqueos.                |
| Consultas     | Resultados y comportamiento específico del motor. |

SQLite sirve para pruebas rápidas, pero no reemplaza las pruebas con PostgreSQL si ese será el motor de producción.

## 7. Organiza las pruebas

- **Unitarias:** lógica de negocio sin depender de una base de datos real.
    
- **Integración:** modelos, sesiones, restricciones y transacciones.
    
- **API:** endpoints con una sesión de prueba mediante `app.dependency_overrides`.
    
- **Multimotor:** mismas pruebas de integración con SQLite y PostgreSQL.
    

Ejecuta las pruebas con:

```sh
uv run pytest
uv run pytest tests/test_persistencia.py -v
```

**Orden recomendado:** aislamiento de la base → restricciones → transacciones → relaciones → persistencia tras reconectar → pruebas en PostgreSQL.

**Prioriza:** verifica primero la integridad, el rollback y la persistencia tras cerrar la sesión. Son las pruebas que detectan errores críticos antes de producción.

**Referencias:**

- [SQLModel: pruebas con FastAPI](https://sqlmodel.tiangolo.com/tutorial/fastapi/tests/)
    
- [SQLAlchemy: transacciones y sesiones](https://docs.sqlalchemy.org/en/20/orm/session_transaction.html)
    
- [Pytest: fixtures](https://docs.pytest.org/en/stable/how-to/fixtures.html)