# Banderas Regex en Python — `IGNORECASE`, `MULTILINE` y `DOTALL`

Modifica el comportamiento de una expresión regular mediante banderas (`flags`) sin cambiar el patrón.

|Bandera|Efecto|
|---|---|
|`re.IGNORECASE` (`re.I`)|Ignora diferencias entre mayúsculas y minúsculas.|
|`re.MULTILINE` (`re.M`)|Hace que `^` y `$` coincidan con los límites de cada línea.|
|`re.DOTALL` (`re.S`)|Permite que `.` coincida también con saltos de línea.|
|`re.ASCII` (`re.A`)|Restringe `\d`, `\w`, `\s` y sus complementos a la semántica ASCII.|

## Ejemplo funcional

```python
import re

texto = "PEDIDO 123\npedido 456"

# Ignora mayúsculas y minúsculas
print(re.findall(r"pedido", texto, re.IGNORECASE))
# ['PEDIDO', 'pedido']

# Busca al inicio de cada línea
print(re.findall(r"^pedido \d+", texto, re.IGNORECASE | re.MULTILINE))
# ['PEDIDO 123', 'pedido 456']

# Permite que el punto abarque varias líneas
print(re.search(r"PEDIDO.*456", texto, re.IGNORECASE | re.DOTALL).group())
# PEDIDO 123
# pedido 456
```

**Reglas:**

- Combina banderas con `|`.
    
- Usa `MULTILINE` para procesar registros línea por línea.
    
- Usa `DOTALL` cuando el patrón deba abarcar varias líneas.
    
- Pasa `flags=` explícitamente cuando llames a `re.compile()`, `re.search()` u otras funciones compatibles.
    

**Referencia:** [Documentación oficial de banderas Regex — Python 3.14](https://docs.python.org/3.14/library/re.html#flags).