# SQLite y PostgreSQL: diferencias prácticas en SQLModel

**Versión de referencia:** SQLModel 0.0.48 · 24-10-2026

## 1. Conexiones y configuración

- **SQLite:** guarda la base de datos en un archivo; no requiere servidor.
    
- **PostgreSQL:** utiliza un servidor y conexiones de red.
    
- Mantén la configuración de la conexión separada de los modelos y servicios.
    

```python
# SQLite
DATABASE_URL = "sqlite:///./datos.db"

# PostgreSQL
DATABASE_URL = "postgresql+psycopg://usuario:clave@localhost:5432/app"
```

Instala el controlador de PostgreSQL:

```sh
uv add "psycopg[binary]"
```

## 2. Tipos de datos

- **SQLite:** utiliza tipado dinámico; las declaraciones de tipos no siempre restringen los valores almacenados.
    
- **PostgreSQL:** aplica tipos de datos más estrictos.
    
- Comprueba especialmente `bool`, `str`, `datetime`, `Decimal`, UUID y valores nulos.
    
- Usa `Field(max_length=...)` y validadores Pydantic para validar entradas; no asumas que SQLite impondrá las mismas restricciones.
    

## 3. Restricciones e integridad

- Define claves primarias, claves foráneas, `UNIQUE`, `NOT NULL` y `CHECK` según las reglas del dominio.
    
- En SQLite, activa explícitamente las claves foráneas en cada conexión.
    
- Prueba las restricciones en PostgreSQL antes de desplegar: ambos motores pueden comportarse de forma distinta.
    

## 4. Concurrencia y transacciones

- **SQLite:** permite múltiples lectores, pero solo un escritor a la vez. Puedes encontrar `database is locked`.
    
- **PostgreSQL:** permite mayor concurrencia de escritura mediante su arquitectura cliente-servidor.
    
- Mantén las transacciones cortas; confirma con `commit()` y revierte con `rollback()` ante errores.
    
- Usa una sesión por petición o unidad de trabajo. No compartas una misma `Session` entre peticiones concurrentes.
    

## 5. Diseña para migrar

- Usa SQLModel para compartir modelos y operaciones CRUD.
    
- Separa la URL de conexión de la lógica de negocio mediante configuración.
    
- Evita SQL específico de un motor cuando exista una alternativa portable.
    
- Prueba consultas, restricciones, valores por defecto y transacciones en ambos motores.
    
- Crear tablas con `SQLModel.metadata.create_all()` no migra automáticamente un esquema existente.
    

## Ejemplo mínimo

```python
from sqlmodel import SQLModel, Field, create_engine

class Producto(SQLModel, table=True):
    id: int | None = Field(default=None, primary_key=True)
    nombre: str = Field(index=True, unique=True)

engine = create_engine(DATABASE_URL)
SQLModel.metadata.create_all(engine)
```

## Orden recomendado

1. Configuración de `DATABASE_URL` y controladores.
    
2. Diferencias de tipos y restricciones.
    
3. Claves foráneas en SQLite.
    
4. Sesiones, transacciones y concurrencia.
    
5. Pruebas de integración con ambos motores.
    

**Prioriza:** desarrolla primero con SQLite si simplifica el entorno, pero ejecuta las pruebas de integración en PostgreSQL antes de producción cuando ese sea el motor de destino.

**Referencias:**

- [SQLModel: crear tablas y diferencias de tipos](https://sqlmodel.tiangolo.com/tutorial/create-db-and-table/)
    
- [SQLite: tipos y concurrencia](https://sqlite.org/faq.html)
    
- [SQLite: aislamiento y transacciones](https://www.sqlite.org/isolation.html)