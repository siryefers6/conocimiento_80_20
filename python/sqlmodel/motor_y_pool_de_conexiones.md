# SQLModel 0.0.48: motor y pool de conexiones

**Referencia:** SQLAlchemy 2.x · 24-10-2026

## 1. Crear el motor

Usa `create_engine()` para configurar la conexión a la base de datos. Crea un motor compartido por la aplicación, no uno por petición.

```python
from sqlmodel import create_engine

engine = create_engine(
    "sqlite:///./datos.db",
    echo=False,
)
```

- `echo=True`: muestra el SQL generado; úsalo para depurar.
    
- `connect_args`: pasa opciones específicas al controlador.
    
- `pool_pre_ping=True`: comprueba conexiones del pool antes de utilizarlas.
    

## 2. Opciones principales del pool

| Parámetro       | Función                                                    |
| --------------- | ---------------------------------------------------------- |
| `pool_size`     | Número de conexiones permanentes del pool.                 |
| `max_overflow`  | Conexiones temporales adicionales permitidas.              |
| `pool_timeout`  | Tiempo máximo de espera por una conexión disponible.       |
| `pool_recycle`  | Renueva conexiones que superan una antigüedad configurada. |
| `pool_pre_ping` | Detecta conexiones desconectadas antes de usarlas.         |

Estos parámetros dependen del tipo de pool y del motor. No los configures sin necesidad.

## 3. Configuración para SQLite

```python
engine = create_engine(
    "sqlite:///./datos.db",
    connect_args={"timeout": 30},
)
```

- `timeout`: segundos que SQLite espera cuando la base de datos está bloqueada.
    
- Las conexiones SQLite tienen particularidades respecto al uso entre hilos.
    
- No apliques automáticamente `pool_size` y `max_overflow`: el pool predeterminado depende de la URL y la configuración.
    

Para una base SQLite en memoria compartida entre pruebas, revisa `StaticPool` y `check_same_thread`; no son necesarios para todas las aplicaciones.

## 4. Configuración para PostgreSQL

```python
engine = create_engine(
    DATABASE_URL,
    pool_size=5,
    max_overflow=10,
    pool_timeout=30,
    pool_pre_ping=True,
)
```

El ejemplo permite hasta 15 conexiones simultáneas por motor. Ajusta los valores según la carga y el límite de conexiones del servidor.

## 5. Parámetros del controlador

`connect_args` permite configurar opciones que SQLAlchemy no interpreta directamente.

```python
engine = create_engine(
    DATABASE_URL,
    connect_args={"connect_timeout": 10},
)
```

Este ejemplo corresponde a controladores PostgreSQL compatibles con `connect_timeout`; verifica las opciones admitidas por el controlador elegido.

## 6. Buenas prácticas

- Crea el motor una vez durante la inicialización de la aplicación.
    
- Crea una `Session` por petición o unidad de trabajo y ciérrala siempre.
    
- No compartas sesiones entre tareas concurrentes.
    
- Dimensiona el pool considerando el número de procesos: cada proceso puede mantener su propio pool.
    
- No uses `pool_pre_ping` como sustituto de una estrategia para gestionar transacciones interrumpidas.
    
- En pruebas, utiliza un motor separado y limpia los datos entre casos.
    

**Orden recomendado:** `create_engine()` → opciones específicas de SQLite/PostgreSQL → parámetros del pool → ciclo de vida de `Session` → pruebas de carga.

**Prioriza:** configurar correctamente el motor y el ciclo de vida de las sesiones antes de ajustar el pool por rendimiento.

**Referencias:**

- [SQLAlchemy: Engine Configuration](https://docs.sqlalchemy.org/en/20/core/engines.html)
    
- [SQLAlchemy: Connection Pooling](https://docs.sqlalchemy.org/en/20/core/pooling.html)
    
- [SQLModel: sesiones de base de datos](https://sqlmodel.tiangolo.com/tutorial/fastapi/session-with-dependency/)