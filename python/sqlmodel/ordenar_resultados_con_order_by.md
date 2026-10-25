# SQLModel: ordenar resultados con `order_by()`

**Referencia:** SQLModel 0.0.48 · 24-10-2026

## Concepto

Utiliza `order_by()` para ordenar los registros directamente en la base de datos, sin ordenar manualmente los resultados en Python.

## 1. Orden ascendente

```python
from sqlmodel import select

statement = select(Producto).order_by(Producto.nombre)
```

Ordena por nombre de forma ascendente. `asc()` permite indicar explícitamente el sentido:

```python
statement = select(Producto).order_by(Producto.nombre.asc())
```

## 2. Orden descendente

```python
statement = select(Producto).order_by(Producto.precio.desc())
```

Utiliza `desc()` para mostrar primero los valores más altos.

## 3. Ordenar por varias columnas

```python
statement = select(Producto).order_by(
    Producto.precio.desc(),
    Producto.nombre.asc(),
)
```

Ordena primero por precio descendente y, cuando los precios coincidan, por nombre ascendente.

## 4. Ejecutar la consulta

```python
with Session(engine) as session:
    productos = session.exec(statement).all()
```

## Reglas prácticas

- Usa `.asc()` para ordenar de menor a mayor.
    
- Usa `.desc()` para ordenar de mayor a menor.
    
- Combina columnas para definir criterios secundarios.
    
- Añade una clave única como criterio final cuando necesites un orden determinista entre registros con valores iguales.
    

**Prioriza:** `order_by()`, `asc()`, `desc()` y el ordenamiento por múltiples columnas.