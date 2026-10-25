# Validación de claves — cardinalidad y duplicados en `merge()`

**Pandas 3.1.0**

Valida las claves antes de combinar DataFrames para detectar relaciones inesperadas y evitar que una unión multiplique filas.

## 1. Detectar claves duplicadas

```python
# Mostrar filas con claves duplicadas
df[df.duplicated("pedido_id", keep=False)]

# Comprobar si existen duplicados
df["pedido_id"].duplicated().any()

# Exigir claves únicas
assert df["pedido_id"].is_unique
```

- `duplicated(keep=False)`: marca todas las apariciones de cada clave repetida.
    
- `is_unique`: comprueba que cada valor sea único.
    

## 2. Validar la cardinalidad de una unión

Usa `validate` en `merge()` para comprobar la relación esperada entre las claves.

```python
resultado = pd.merge(
    pedidos,
    despachos,
    on="pedido_id",
    how="left",
    validate="one_to_one"
)
```

| `validate`                 | Relación        | Condición                           |
| -------------------------- | --------------- | ----------------------------------- |
| `"one_to_one"` o `"1:1"`   | Uno a uno       | Claves únicas en ambas tablas       |
| `"one_to_many"` o `"1:m"`  | Uno a muchos    | Claves únicas en la tabla izquierda |
| `"many_to_one"` o `"m:1"`  | Muchos a uno    | Claves únicas en la tabla derecha   |
| `"many_to_many"` o `"m:m"` | Muchos a muchos | No exige unicidad                   |

Si la relación no cumple la validación, Pandas lanza `MergeError`.

## 3. Detectar registros sin correspondencia

```python
resultado = pd.merge(
    pedidos,
    despachos,
    on="pedido_id",
    how="outer",
    indicator=True
)

# Claves presentes solo en pedidos
resultado[resultado["_merge"] == "left_only"]

# Claves presentes solo en despachos
resultado[resultado["_merge"] == "right_only"]

# Claves coincidentes
resultado[resultado["_merge"] == "both"]
```

`indicator=True` añade `_merge`, que identifica el origen de cada registro.

## Reglas prácticas

- Comprueba los duplicados antes de unir.
    
- Usa `validate` para hacer cumplir la cardinalidad esperada.
    
- Usa `indicator=True` para detectar registros sin correspondencia.
    
- Revisa las claves nulas: Pandas puede emparejar claves nulas entre sí, a diferencia del comportamiento habitual de SQL.
    
- No elimines duplicados automáticamente: primero determina si son errores o registros legítimos.
    

**Prioriza:** `duplicated()`, `is_unique`, `validate` e `indicator=True` para garantizar uniones confiables.