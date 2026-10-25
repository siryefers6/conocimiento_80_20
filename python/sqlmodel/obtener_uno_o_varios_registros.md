# SQLModel: obtener uno o varios registros

**Referencia:** SQLModel 0.0.48 · 24-10-2026

## Concepto

Selecciona el método según la cantidad de resultados esperada y la forma de identificar el registro.

## 1. `get()`: buscar por clave primaria

```python
with Session(engine) as session:
    producto = session.get(Producto, 1)
```

Devuelve el registro o `None` si no existe.

## 2. `first()`: obtener el primer resultado

```python
statement = select(Producto).where(Producto.precio > 1000)

with Session(engine) as session:
    producto = session.exec(statement).first()
```

Devuelve el primer resultado o `None`. Usa `order_by()` si necesitas determinar cuál registro debe aparecer primero.

## 3. `one()`: exigir exactamente un resultado

```python
with Session(engine) as session:
    producto = session.exec(
        select(Producto).where(Producto.id == 1)
    ).one()
```

Exige exactamente un resultado. Lanza una excepción si no encuentra ninguno o encuentra más de uno.

## 4. `all()`: recuperar todos los resultados

```python
with Session(engine) as session:
    productos = session.exec(select(Producto)).all()
```

Devuelve una secuencia con todos los registros coincidentes. Aplica `where()` y `limit()` cuando no necesites recuperar la tabla completa.

## Comparación

| Método    | Resultado                      | Si no hay coincidencias |
| --------- | ------------------------------ | ----------------------- |
| `get()`   | Un registro por clave primaria | `None`                  |
| `first()` | Primer resultado               | `None`                  |
| `one()`   | Exactamente un resultado       | Excepción               |
| `all()`   | Todos los resultados           | Colección vacía         |

## Regla práctica

- Usa `get()` para búsquedas por clave primaria.
    
- Usa `first()` cuando aceptes cero o un resultado seleccionado entre varios.
    
- Usa `one()` cuando la existencia de un único resultado sea obligatoria.
    
- Usa `all()` para colecciones; limita los resultados cuando sea necesario.
    

**Prioriza:** diferencia entre ausencia de resultados, unicidad obligatoria y recuperación de colecciones.