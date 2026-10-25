# SQLModel: inspección del SQL generado

**Versión:** SQLModel 0.0.48 · SQLAlchemy 2.x · **Referencia:** 24-10-2026

## 1. Activar `echo=True`

Muestra en la consola las consultas SQL y los parámetros que SQLAlchemy ejecuta.

```python
from sqlmodel import create_engine

engine = create_engine(
    "sqlite:///app.db",
    echo=True,
)
```

Úsalo durante el desarrollo para identificar consultas repetidas, filtros ausentes y operaciones inesperadas. Desactívalo en producción si puede exponer datos sensibles o generar demasiado registro.

## 2. Inspeccionar una consulta sin ejecutarla

```python
from sqlmodel import select

consulta = select(Producto).where(Producto.precio > 100)

print(consulta)
```

Para compilarla con el dialecto de SQLite:

```python
sql = consulta.compile(
    engine,
    compile_kwargs={"literal_binds": False},
)

print(sql)
print(sql.params)
```

Inspecciona por separado el SQL y los parámetros. Evita interpolar manualmente valores en consultas SQL.

## 3. Registrar eventos de ejecución

Usa los eventos de SQLAlchemy para contar consultas o medir su duración.

```python
from sqlalchemy import event
from time import perf_counter


@event.listens_for(engine, "before_cursor_execute")
def iniciar_consulta(
    conn, cursor, statement, parameters, context, executemany
):
    context._inicio_consulta = perf_counter()


@event.listens_for(engine, "after_cursor_execute")
def finalizar_consulta(
    conn, cursor, statement, parameters, context, executemany
):
    inicio = getattr(context, "_inicio_consulta", None)

    if inicio is not None:
        duracion_ms = (perf_counter() - inicio) * 1000
        print(f"{duracion_ms:.2f} ms | {statement}")
```

Estos eventos son útiles para detectar consultas lentas y repeticiones. En aplicaciones concurrentes, utiliza un sistema de logging estructurado en lugar de imprimir directamente.

## 4. Detectar consultas ineficientes

Busca patrones como:

- **N+1:** una consulta principal seguida de muchas consultas para cargar relaciones.
    
- **Consultas duplicadas:** recuperar repetidamente el mismo registro.
    
- **Lecturas excesivas:** usar `select(Modelo)` cuando solo necesitas unas columnas.
    
- **Falta de índices:** filtros frecuentes que provocan recorridos completos de tablas.
    
- **Consultas sin límites:** devolver miles de filas sin paginación.
    

## 5. Examinar el plan de ejecución

En SQLite, utiliza `EXPLAIN QUERY PLAN` para comprobar si el motor aprovecha los índices.

```python
with engine.connect() as conn:
    resultado = conn.exec_driver_sql(
        "EXPLAIN QUERY PLAN "
        "SELECT * FROM producto WHERE precio > ?",
        (100,),
    )

    for fila in resultado:
        print(fila)
```

Para PostgreSQL, utiliza `EXPLAIN ANALYZE` con precaución: ejecuta realmente la consulta y puede tener efectos importantes si se aplica a sentencias que modifican datos.

## Reglas prácticas

- Activa `echo=True` para inspecciones rápidas durante el desarrollo.
    
- Usa eventos para medir duración y frecuencia de las consultas.
    
- Comprueba los planes de ejecución antes de añadir índices.
    
- Mide con datos representativos; no optimices únicamente por la apariencia del SQL.
    
- Evita registrar contraseñas, tokens y parámetros sensibles.
    
- Desactiva el registro detallado en producción o configura filtros y niveles de logging adecuados.
    

## Prioriza

1. Inspecciona las consultas con `echo=True`.
    
2. Identifica N+1 y lecturas innecesarias.
    
3. Mide duración y frecuencia con eventos.
    
4. Verifica los índices con el plan de ejecución.