# SQLModel: filtrar registros con `where()`

**Referencia:** SQLModel 0.0.48 · 24-10-2026

## Concepto

Utiliza `where()` para filtrar registros mediante condiciones SQLAlchemy. Aplica los filtros en la consulta para recuperar únicamente los datos necesarios.

## 1. Igualdad y desigualdad

```python
select(Producto).where(Producto.nombre == "Teclado")
select(Producto).where(Producto.precio != 1000)
```

| Operador    | Significado                   |
| ----------- | ----------------------------- |
| `==`        | Igual a                       |
| `!=`        | Distinto de                   |
| `>` / `<`   | Mayor / menor que             |
| `>=` / `<=` | Mayor o igual / menor o igual |

## 2. Rangos y pertenencia

```python
# Precio dentro de un rango
select(Producto).where(
    (Producto.precio >= 1000) & (Producto.precio <= 5000)
)

# Coincidencia con varios valores
select(Producto).where(Producto.id.in_([1, 2, 3]))
```

## 3. Valores nulos

```python
select(Producto).where(Producto.descripcion == None)
select(Producto).where(Producto.descripcion != None)
```

Estas expresiones generan comparaciones SQL equivalentes a `IS NULL` e `IS NOT NULL`. Para mayor claridad, también puedes utilizar `is_(None)` e `is_not(None)`.

## 4. Combinar condiciones

```python
from sqlmodel import select

statement = select(Producto).where(
    (Producto.precio >= 1000) &
    (Producto.disponible == True)
)
```

- `&`: combina condiciones con AND.
    
- `|`: combina condiciones con OR.
    
- `~`: niega una condición.
    
- Coloca paréntesis alrededor de cada condición al utilizar `&` y `|`.
    

## 5. Ejecutar la consulta

```python
with Session(engine) as session:
    productos = session.exec(statement).all()
```

## Reglas prácticas

- Filtra en la base de datos, no después de cargar todos los registros.
    
- Usa `in_()` para comparar con una colección de valores.
    
- Usa `is_(None)` e `is_not(None)` para comprobar valores nulos.
    
- Construye las condiciones con expresiones SQLAlchemy, no con `and` ni `or` de Python.
    

**Prioriza:** igualdad, rangos, `in_()`, valores nulos y combinaciones AND/OR.