# SQLModel: `create_engine()` y conexión a la base de datos

**Referencia:** SQLModel 0.0.48 · 24-10-2026

## Concepto

Utiliza `create_engine()` para configurar el motor de SQLAlchemy que administra las conexiones y permite ejecutar operaciones contra una base de datos.

## 1. Conexión con SQLite

```python
from sqlmodel import create_engine

DATABASE_URL = "sqlite:///database.db"

engine = create_engine(
    DATABASE_URL,
    echo=False,
)
```

- `DATABASE_URL`: especifica el motor y la ubicación de la base de datos.
    
- `echo=False`: evita imprimir las consultas SQL en la consola.
    
- `engine`: administra las conexiones; no representa una sesión ni una conexión permanente por sí sola.
    

Para SQLite con FastAPI, habilita `check_same_thread=False` cuando sea necesario compartir la base de datos entre distintos hilos:

```python
engine = create_engine(
    "sqlite:///database.db",
    connect_args={"check_same_thread": False},
)
```

## 2. Conexión con PostgreSQL

Instala un controlador compatible y utiliza una URL adecuada:

```python
engine = create_engine(
    "postgresql+psycopg://usuario:clave@localhost:5432/tienda"
)
```

Instala el controlador Psycopg 3 con `uv add "psycopg[binary]"`. No guardes credenciales directamente en el código.

## 3. Crear las tablas

```python
from sqlmodel import SQLModel

SQLModel.metadata.create_all(engine)
```

Ejecuta esta instrucción después de importar los modelos. Crea las tablas inexistentes, pero **no modifica automáticamente las tablas existentes** para reflejar cambios en los modelos.

## 4. Parámetros esenciales

| Parámetro       | Función                                             |
| --------------- | --------------------------------------------------- |
| `echo`          | Activa o desactiva el registro de SQL.              |
| `connect_args`  | Pasa argumentos específicos al controlador.         |
| `pool_pre_ping` | Comprueba conexiones del pool antes de utilizarlas. |
| `pool_recycle`  | Define cuándo reciclar conexiones del pool.         |

**Prioriza:** comprende `DATABASE_URL`, las diferencias entre SQLite y PostgreSQL, el ciclo de vida de `engine` y la creación inicial de tablas antes de estudiar `Session`.