# SQL — `read_sql()` y `to_sql()` en Pandas

Integra Pandas con bases de datos SQL. Usa `pd.read_sql()` para consultar datos y `DataFrame.to_sql()` para guardar resultados en tablas.

## 1. Instalar dependencias

```
uv add pandas sqlalchemy
```

## 2. Leer datos desde SQL

```python
import pandas as pd
from sqlalchemy import create_engine

engine = create_engine("sqlite:///datos.db")

df = pd.read_sql(
    "SELECT * FROM pedidos",
    con=engine,
)
```

## 3. Filtrar datos mediante SQL

Usa consultas parametrizadas para evitar concatenar valores directamente en las consultas.

```python
from sqlalchemy import text

consulta = text("""
    SELECT numero_pedido, cantidad
    FROM pedidos
    WHERE cantidad > :minimo
""")

df = pd.read_sql(
    consulta,
    con=engine,
    params={"minimo": 10},
)
```

## 4. Guardar un DataFrame en SQL

```python
df.to_sql(
    "pedidos_procesados",
    con=engine,
    if_exists="append",
    index=False,
)
```

Valores de `if_exists`:

- `fail`: genera un error si la tabla existe.
    
- `replace`: elimina y recrea la tabla.
    
- `append`: agrega filas a la tabla existente.
    

**Precaución:** `replace` puede eliminar índices, restricciones y otros objetos asociados a la tabla. No lo uses sobre tablas de producción sin evaluar sus efectos.

## 5. Leer datos por bloques

```python
for bloque in pd.read_sql(
    "SELECT * FROM pedidos",
    con=engine,
    chunksize=10000,
):
    print(bloque.shape)
```

Procesa consultas grandes por bloques para reducir el consumo de memoria.

## Regla 80/20

Domina `read_sql()`, `to_sql()`, `create_engine()`, `params`, `if_exists` y `chunksize`. Usa SQL para filtrar los datos en origen y Pandas para analizarlos.

**Versión de referencia:** Pandas 3.1.0.