# Transformaciones condicionales — `where()`, `mask()` y `replace()`

Modifica valores según condiciones o reemplaza valores específicos sin recorrer las filas manualmente.

## Ejemplo básico

```python
import pandas as pd

df = pd.DataFrame({
    "pedido": [101, 102, 103, 104],
    "peso": [10, 25, 5, 40],
})

# Conserva los valores que cumplen la condición
df["peso_valido"] = df["peso"].where(df["peso"] >= 10)

# Reemplaza los valores que cumplen la condición
df["peso_corregido"] = df["peso"].mask(df["peso"] < 10, 10)

# Reemplaza valores específicos
df["peso"] = df["peso"].replace(25, 30)
```

## Diferencias esenciales

- `where(condición, otro)`: conserva los valores donde la condición es verdadera; reemplaza los demás por `otro` o `NaN` si se omite.
    
- `mask(condición, otro)`: reemplaza los valores donde la condición es verdadera.
    
- `replace(valor_antiguo, valor_nuevo)`: reemplaza valores específicos, sin necesidad de una condición booleana.
    

## Patrones útiles

```python
# Reemplazar varios valores
df["peso"] = df["peso"].replace({
    10: 12,
    30: 32,
})

# Conservar pesos dentro del rango permitido
df["peso_valido"] = df["peso"].where(
    df["peso"].between(10, 35)
)

# Convertir pesos inferiores a 10 en valores ausentes
df["peso"] = df["peso"].mask(df["peso"] < 10)
```

## Reglas 80/20

- Usa `where()` para conservar datos válidos y descartar los que incumplen una condición.
    
- Usa `mask()` para sustituir los datos que incumplen una regla.
    
- Usa `replace()` para corregir códigos, etiquetas o valores concretos.
    
- Asigna el resultado si necesitas conservar la transformación.
    

**Precaución:** `where()` y `mask()` pueden introducir valores ausentes y modificar el tipo de datos resultante.

**Versión de referencia:** Pandas 3.1.0.