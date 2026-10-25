# SQLModel: índices con `index=True`

**Versión:** SQLModel 0.0.48 · **Referencia:** 24-10-2026

## Concepto

Un índice permite localizar registros más rápidamente sin recorrer toda la tabla. Úsalo en columnas que aparecen frecuentemente en filtros, búsquedas y uniones.

## 1. Crear un índice

```python
from sqlmodel import SQLModel, Field


class Producto(SQLModel, table=True):
    id: int | None = Field(default=None, primary_key=True)
    codigo: str = Field(index=True)
    nombre: str
    precio: float
```

`index=True` solicita a SQLAlchemy crear un índice para `codigo` al generar el esquema.

## 2. Consultar una columna indexada

```python
from sqlmodel import Session, select


with Session(engine) as session:
    consulta = select(Producto).where(Producto.codigo == "ABC123")
    producto = session.exec(consulta).first()
```

El motor puede aprovechar el índice para encontrar el código. **Un índice no garantiza una consulta más rápida**: depende del volumen, la selectividad, la consulta y el plan de ejecución.

## 3. Índices compuestos y únicos

Usa un índice compuesto cuando las consultas filtren habitualmente por varias columnas.

```python
from sqlalchemy import Index
from sqlmodel import SQLModel, Field


class Pedido(SQLModel, table=True):
    __table_args__ = (
        Index("ix_pedido_estado_fecha", "estado", "fecha"),
    )

    id: int | None = Field(default=None, primary_key=True)
    estado: str
    fecha: str
```

El orden de las columnas importa: el índice `(estado, fecha)` suele ser útil para filtrar por `estado` o por `estado` y `fecha`, pero no necesariamente para filtrar solo por `fecha`.

Para impedir valores duplicados, utiliza `unique=True`:

```python
codigo: str = Field(unique=True, index=True)
```

Esto crea una restricción de unicidad con soporte de índice según el motor. No agregues índices redundantes sin medir su utilidad.

## Costes y reglas prácticas

- **Lecturas:** pueden acelerarse con índices adecuados.
    
- **Escrituras:** `INSERT`, `UPDATE` y `DELETE` deben mantener los índices, lo que añade coste.
    
- **Espacio:** cada índice consume almacenamiento.
    
- **Claves foráneas:** considera indexar las columnas usadas en uniones y búsquedas; la necesidad depende del motor y de las consultas.
    
- **Migraciones:** `SQLModel.metadata.create_all()` no actualiza de forma general los índices de tablas existentes; aplica los cambios de esquema mediante el procedimiento de migración del proyecto.
    
- **Medición:** usa `EXPLAIN QUERY PLAN` en SQLite o `EXPLAIN ANALYZE` en PostgreSQL para comprobar si el índice se utiliza y si mejora el rendimiento.
    

## Prioriza

1. Identifica las consultas frecuentes y sus filtros.
    
2. Añade índices a columnas selectivas que realmente se consultan.
    
3. Evalúa índices compuestos según el orden de los filtros.
    
4. Mide rendimiento y coste de escritura antes de añadir índices adicionales.