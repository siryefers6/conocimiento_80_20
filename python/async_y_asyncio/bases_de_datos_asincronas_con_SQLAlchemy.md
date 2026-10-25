# Bases de datos asíncronas con SQLAlchemy

Ejecuta consultas SQL sin bloquear el event loop mediante controladores asíncronos y `AsyncSession`.

```python
from sqlalchemy import select
from sqlalchemy.ext.asyncio import (
    async_sessionmaker,
    create_async_engine,
)

engine = create_async_engine(
    "sqlite+aiosqlite:///app.db"
)
Session = async_sessionmaker(engine, expire_on_commit=False)

async def obtener_usuarios():
    async with Session() as sesion:
        resultado = await sesion.scalars(select(Usuario))
        return resultado.all()

async def cerrar_conexiones():
    await engine.dispose()
```

- **Controlador asíncrono:** utiliza `aiosqlite` para SQLite o `asyncpg` para PostgreSQL.
    
- **Sesiones:** crea una sesión por tarea o unidad de trabajo; no compartas una `AsyncSession` entre tareas concurrentes.
    
- **Consultas:** aplica `await` a las operaciones asíncronas, como `execute()`, `scalars()` y `commit()`.
    
- **Transacciones:** usa `async with sesion.begin():` para confirmar automáticamente si todo termina bien y revertir ante errores.
    
- **Recursos:** cierra las sesiones y ejecuta `await engine.dispose()` al finalizar el ciclo de vida de la aplicación.
    
- **SQLModel:** para operaciones asíncronas, utiliza `AsyncSession` de SQLAlchemy con modelos SQLModel y un controlador compatible.
    

Instala las dependencias para SQLite con `uv add sqlalchemy aiosqlite`.