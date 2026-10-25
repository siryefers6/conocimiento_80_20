# SQLModel: consultas de columnas y agregaciones

**Versión:** SQLModel 0.0.48 · SQLAlchemy 2.x · **Referencia:** 24-10-2026

## 1. Seleccionar columnas específicas

Usa `select()` para recuperar únicamente las columnas necesarias en lugar de cargar entidades completas.

```python
from sqlmodel import select

consulta = select(Producto.nombre, Producto.precio)

for nombre, precio in session.exec(consulta):
    print(nombre, precio)
```

Reduce los datos transferidos cuando solo necesitas ciertos campos para informes o listados.

## 2. Contar registros con `count()`

```python
from sqlalchemy import func
from sqlmodel import select

consulta = select(func.count()).select_from(Producto)
total = session.exec(consulta).one()
```

Para contar solo los productos que cumplen una condición:

```python
consulta = (
    select(func.count())
    .select_from(Producto)
    .where(Producto.precio > 100)
)
total = session.exec(consulta).one()
```

`func.count()` genera la función SQL `COUNT()`.

## 3. Calcular sumas con `sum()`

```python
consulta = select(func.sum(Producto.precio))
total = session.exec(consulta).one()
```

`SUM()` devuelve `NULL` si no existen valores que sumar. Si necesitas un cero en ese caso, utiliza `func.coalesce()`:

```python
consulta = select(
    func.coalesce(func.sum(Producto.precio), 0)
)
total = session.exec(consulta).one()
```

## 4. Agrupar resultados con `group_by()`

Calcula el total de productos por categoría:

```python
consulta = (
    select(Producto.categoria, func.count())
    .group_by(Producto.categoria)
)

for categoria, cantidad in session.exec(consulta):
    print(categoria, cantidad)
```

Las columnas seleccionadas que no sean agregaciones deben formar parte de `group_by()`.

## 5. Filtrar grupos con `having()`

Usa `having()` para filtrar los resultados después de agruparlos.

```python
consulta = (
    select(Producto.categoria, func.sum(Producto.precio))
    .group_by(Producto.categoria)
    .having(func.sum(Producto.precio) > 1000)
)

for categoria, total in session.exec(consulta):
    print(categoria, total)
```

- `where()`: filtra registros antes de agrupar.
    
- `having()`: filtra los grupos después de aplicar las agregaciones.
    

## Funciones esenciales

| Función        | Utilidad                 |
| -------------- | ------------------------ |
| `func.count()` | Contar registros         |
| `func.sum()`   | Sumar valores            |
| `func.avg()`   | Calcular promedios       |
| `func.min()`   | Obtener el mínimo        |
| `func.max()`   | Obtener el máximo        |
| `group_by()`   | Agrupar registros        |
| `having()`     | Filtrar grupos agregados |

## Reglas prácticas

- Selecciona solo las columnas necesarias para reducir la carga de datos.
    
- Usa `session.exec(consulta).one()` cuando esperes exactamente una fila.
    
- Usa `session.exec(consulta).all()` para recuperar todas las filas resultantes.
    
- Usa `coalesce()` cuando una agregación pueda devolver `NULL` y necesites un valor alternativo.
    
- No uses `where()` para filtrar resultados agregados; utiliza `having()`.
    
- Comprueba que las columnas no agregadas estén incluidas en `group_by()` para mantener consultas compatibles con distintos motores SQL.
    

## Prioriza

1. Domina `select()` con columnas específicas.
    
2. Aprende `count()` y `sum()`.
    
3. Combina `group_by()` con `having()`.
    
4. Añade `avg()`, `min()` y `max()` para informes y análisis.