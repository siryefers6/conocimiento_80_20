# SQLModel 0.0.48: compatibilidad con SQLAlchemy 2.1

**Referencia:** 24-10-2026

## 1. Compatibilidad oficial

SQLModel **0.0.48 incorpora soporte para SQLAlchemy 2.1**. La versión anterior, 0.0.47, restringía la dependencia a SQLAlchemy 2.0. <Cite refs={["turn547498search0","turn547498search5"]}/>

## 2. API esenciales compatibles

Continúa utilizando las API modernas de SQLAlchemy 2.x:

| API                                | Uso                                      |
| ---------------------------------- | ---------------------------------------- |
| `create_engine()`                  | Crear el motor de conexión.              |
| `Session`                          | Gestionar sesiones y transacciones.      |
| `select()`                         | Construir consultas SQL.                 |
| `session.exec()`                   | Ejecutar consultas mediante SQLModel.    |
| `session.add()` / `delete()`       | Insertar y eliminar entidades.           |
| `session.commit()` / `rollback()`  | Confirmar y revertir transacciones.      |
| `Relationship()`                   | Definir relaciones ORM en SQLModel.      |
| `selectinload()` / `joinedload()`  | Cargar relaciones eficientemente.        |
| `func.count()` / `func.sum()`      | Ejecutar agregaciones SQL.               |
| `text()` / `update()` / `delete()` | Ejecutar SQL textual y operaciones Core. |

## 3. Ejemplo de consulta moderna

```python
from sqlmodel import Session, select
from sqlalchemy.orm import selectinload

with Session(engine) as session:
    consulta = (
        select(Pedido)
        .options(selectinload(Pedido.bultos))
        .where(Pedido.id == 1)
    )

    pedido = session.exec(consulta).first()
```

Prioriza `select()` y las API de estilo 2.x. Evita basar código nuevo en patrones heredados como `session.query()`.

## 4. Cambios importantes de SQLAlchemy 2.1

- **Python:** SQLAlchemy 2.1 requiere Python 3.11 o superior. <Cite refs={["turn547498search8","turn547498search10"]}/>
    
- **AsyncIO:** `greenlet` ya no se instala automáticamente; instala `sqlalchemy[asyncio]` si necesitas las dependencias correspondientes. <Cite refs={["turn547498search6","turn547498search10"]}/>
    
- **Migración:** revisa la guía oficial de cambios de 2.0 a 2.1 antes de actualizar. <Cite refs={["turn547498search8"]}/>
    

## 5. Instala y verifica con `uv`

```sh
uv add "sqlmodel==0.0.48"
uv lock
uv run python -c "import sqlmodel, sqlalchemy; print('SQLModel:', sqlmodel.__version__); print('SQLAlchemy:', sqlalchemy.__version__)"
uv run pytest
```

Deja que `uv` resuelva la versión compatible de SQLAlchemy y registre las dependencias exactas en `uv.lock`. Si necesitas fijar una versión concreta, verifica primero que esté dentro del rango permitido por SQLModel.

## Reglas prácticas

- Usa SQLModel 0.0.48 o posterior que declare explícitamente soporte para SQLAlchemy 2.1.
    
- Mantén las API modernas de SQLAlchemy 2.x.
    
- Comprueba `uv.lock` y ejecuta toda la suite de pruebas tras actualizar.
    
- Si utilizas SQLite, PostgreSQL o MySQL, prueba las operaciones específicas del dialecto y sus restricciones.
    
- No confundas compatibilidad declarada con garantía de que toda aplicación antigua funcionará sin cambios.
    

## Prioriza

1. Verifica las versiones reales instaladas.
    
2. Domina `select()`, `Session`, transacciones y relaciones.
    
3. Revisa los cambios de Python y AsyncIO.
    
4. Ejecuta las pruebas antes de desplegar la actualización.
    
