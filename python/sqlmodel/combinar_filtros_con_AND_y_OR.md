# SQLModel: combinar filtros con AND y OR

**Referencia:** SQLModel 0.0.48 · 24-10-2026

## Concepto

Combina condiciones dentro de `where()` para filtrar registros según varios criterios. Utiliza `and_()`, `or_()` o los operadores `&` y `|` de SQLAlchemy.

## 1. AND: todas las condiciones

Exige que se cumplan todas las condiciones.

```python
from sqlalchemy import and_
from sqlmodel import select

statement = select(Producto).where(
    and_(
        Producto.precio > 1000,
        Producto.disponible == True,
    )
)
```

Forma equivalente:

```python
statement = select(Producto).where(
    (Producto.precio > 1000) &
    (Producto.disponible == True)
)
```

## 2. OR: al menos una condición

Devuelve registros que cumplan cualquiera de las condiciones.

```python
from sqlalchemy import or_

statement = select(Producto).where(
    or_(
        Producto.nombre == "Teclado",
        Producto.nombre == "Ratón",
    )
)
```

Forma equivalente:

```python
statement = select(Producto).where(
    (Producto.nombre == "Teclado") |
    (Producto.nombre == "Ratón")
)
```

## 3. Combinar AND y OR

Agrupa las condiciones para controlar la lógica de la consulta.

```python
statement = select(Producto).where(
    (Producto.disponible == True) &
    (
        (Producto.precio < 5000) |
        (Producto.precio > 20000)
    )
)
```

Selecciona productos disponibles cuyo precio sea inferior a 5.000 o superior a 20.000.

## 4. Ejecutar la consulta

```python
with Session(engine) as session:
    productos = session.exec(statement).all()
```

## Reglas prácticas

- Usa `and_()` cuando todas las condiciones deban cumplirse.
    
- Usa `or_()` cuando baste con una condición.
    
- Coloca paréntesis al combinar `&` y `|`.
    
- No utilices `and` ni `or` de Python para combinar expresiones SQL.
    
- Usa `== None` o `is_(None)` para comprobar valores nulos; evita comparaciones ordinarias con `NULL`.
    

**Prioriza:** AND, OR, agrupación de condiciones y operadores de comparación.