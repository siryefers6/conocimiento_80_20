# Clases de caracteres en Regex con Python (`re`)

Identifica caracteres mediante clases predefinidas y conjuntos personalizados.

|Patrón|Coincidencia|
|---|---|
|`\d`|Dígito decimal Unicode|
|`\w`|Carácter alfanumérico Unicode o `_`|
|`\s`|Espacio en blanco, tabulación o salto de línea|
|`.`|Cualquier carácter, excepto salto de línea por defecto|
|`[A-Z]`|Letra mayúscula ASCII|
|`[a-z]`|Letra minúscula ASCII|
|`[0-9]`|Dígito ASCII|
|`[^0-9]`|Cualquier carácter excepto un dígito ASCII|
|`\D`, `\W`, `\S`|Complementos de `\d`, `\w` y `\s`|

## Ejemplo funcional

```python
import re

texto = "Pedido ABC-12345"

print(re.findall(r"\d+", texto))       # ['12345']
print(re.findall(r"[A-Z]+", texto))    # ['A', 'B', 'C']
print(re.findall(r"\w+", texto))       # ['Pedido', 'ABC', '12345']
print(re.findall(r"[A-Z]{3}-\d{5}", texto))  # ['ABC-12345']
```

**Reglas:**

- Combina clases con cuantificadores, como `\d{5}`.
    
- Usa `[^...]` para negar un conjunto.
    
- Recuerda que `\d`, `\w` y `\s` admiten caracteres Unicode por defecto. Usa `re.ASCII` si necesitas restringirlos al comportamiento ASCII.
    
- Dentro de `[]`, varios metacaracteres cambian de significado; por ejemplo, `.` representa un punto literal.
    

**Referencia:** [Documentación oficial de](https://docs.python.org/3.14/library/re.html#regular-expression-syntax) `[re](https://docs.python.org/3.14/library/re.html#regular-expression-syntax)` [— Python 3.14](https://docs.python.org/3.14/library/re.html#regular-expression-syntax).