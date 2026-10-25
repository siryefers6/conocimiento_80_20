# Alternancia y agrupación en Regex con Python (`re`)

Combina alternativas con `|` y agrupa expresiones para controlar su evaluación y repetición.

## Sintaxis esencial

|Sintaxis|Función|
|---|---|
|`a|b`|Coincide con `a` o `b`.|
|`(abc)`|Agrupa y captura el texto coincidente.|
|`(?:abc)`|Agrupa sin capturar.|
|`(a|b)+`|Repite una de las alternativas.|

## Ejemplo funcional

```python
import re

texto = "Pedido 123, Orden 456, Factura 789"

# Alternativas y grupo capturador
patron = r"(Pedido|Orden)\s+(\d+)"
print(re.findall(patron, texto))
# [('Pedido', '123'), ('Orden', '456')]

# Agrupación sin captura
patron = r"(?:Pedido|Orden)\s+\d+"
print(re.findall(patron, texto))
# ['Pedido 123', 'Orden 456']

# Alternativas agrupadas y repetidas
print(re.fullmatch(r"(?:ab|cd)+", "abcdab").group())
# 'abcdab'
```

**Reglas:**

- Agrupa alternativas cuando compartan cuantificadores: `(?:gato|perro)+`.
    
- Usa `(...)` cuando necesites recuperar el contenido mediante grupos.
    
- Usa `(?:...)` cuando solo necesites agrupar, sin crear grupos capturadores.
    
- Recuerda que `|` tiene una precedencia baja; agrupa las alternativas para delimitar su alcance.
    

**Referencia:** [Documentación oficial de](https://docs.python.org/3.14/library/re.html#regular-expression-syntax) `[re](https://docs.python.org/3.14/library/re.html#regular-expression-syntax)` [— Python 3.14](https://docs.python.org/3.14/library/re.html#regular-expression-syntax).