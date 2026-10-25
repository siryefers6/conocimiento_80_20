# SQLModel: construir y ejecutar consultas con `select()`

**Referencia:** SQLModel 0.0.48 · 24-10-2026

## Concepto

Utiliza `select()` para construir consultas tipadas y `session.exec()` para ejecutarlas y obtener resultados.

## 1. Consultar todos los registros

```python
from sqlmodel import Session, select

with Session(engine) as session:
    statement = select(Producto)
    productos = session.exec(statement).all()
```

## 2. Filtrar resultados con `where()`

```python
statement = select(Producto).where(Producto.precio > 10000)

with Session(engine) as session:
    productos = session.exec(statement).all()
```

Combina condiciones con `and_()`, `or_()` o el operador `&` para consultas más específicas.

## 3. Obtener un único resultado

```python
with Session(engine) as session:
    producto = session.exec(
        select(Producto).where(Producto.nombre == "Teclado")
    ).first()
```

- `.first()`: devuelve el primer resultado o `None`.
    
- `.all()`: devuelve todos los resultados.
    
- `.one()`: exige exactamente un resultado.
    
- `.one_or_none()`: acepta cero o un resultado.
    

Usa `session.get(Producto, id)` cuando busques directamente por clave primaria.

## 4. Ordenar y limitar resultados

```python
statement = (
    select(Producto)
    .order_by(Producto.nombre)
    .limit(10)
)

with Session(engine) as session:
    productos = session.exec(statement).all()
```

Aplica `order_by()` para ordenar y `limit()` para restringir la cantidad de registros recuperados.

## Reglas prácticas

- Construye consultas con `select()` en lugar de concatenar SQL manualmente.
    
- Ejecuta la consulta mediante `session.exec()`.
    
- Aplica filtros, orden y límites en la base de datos.
    
- Evita cargar todos los registros cuando solo necesitas unos pocos.
    
- Usa `select()` para consultas y `session.get()` para búsquedas por clave primaria.
    

**Prioriza:** `select()`, `where()`, `order_by()`, `limit()`, `exec()`, `.all()` y `.first()`.