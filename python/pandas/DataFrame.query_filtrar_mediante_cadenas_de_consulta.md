# `DataFrame.query()` — filtrar mediante cadenas de consulta

Usa `query()` para expresar condiciones de filtrado con una cadena legible. Es útil cuando las condiciones se vuelven extensas.

## Ejemplo básico

```python
import pandas as pd

df = pd.DataFrame({
    "pedido": [101, 102, 103, 104],
    "zona": ["Norte", "Sur", "Centro", "Norte"],
    "peso": [10, 25, 15, 30],
})

# Una condición
resultado = df.query("peso > 15")

# Varias condiciones
resultado = df.query("peso > 15 and zona == 'Norte'")

# Operadores lógicos
resultado = df.query("peso >= 15 or zona == 'Sur'")

# Seleccionar valores de una lista
zonas = ["Norte", "Sur"]
resultado = df.query("zona in @zonas")
```

## Variables externas

Usa `@` para referenciar variables definidas fuera de la consulta.

```python
peso_minimo = 20

resultado = df.query("peso >= @peso_minimo")
```

## Reglas 80/20

- `==`, `!=`, `>`, `<`, `>=`, `<=`: comparar valores.
    
- `and`, `or`, `not`: combinar condiciones.
    
- `in`, `not in`: comprobar pertenencia.
    
- `@variable`: incorporar variables externas.
    
- Usa `query()` para filtros legibles; usa `loc[]` o máscaras booleanas cuando necesites mayor flexibilidad.
    

**Precaución:** no construyas consultas con cadenas provenientes de usuarios sin validarlas. `query()` puede evaluar expresiones y no debe tratarse como un mecanismo seguro para consultas arbitrarias.

**Versión de referencia:** Pandas 3.1.0.