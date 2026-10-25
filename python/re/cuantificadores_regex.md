# Cuantificadores Regex en Python (`re`)

Controla cuántas veces puede repetirse un carácter, una clase o un grupo.

|Cuantificador|Significado|Ejemplo|
|---|---|---|
|`*`|Cero o más repeticiones|`a*`|
|`+`|Una o más repeticiones|`a+`|
|`?`|Cero o una repetición|`a?`|
|`{n}`|Exactamente `n` repeticiones|`\d{5}`|
|`{m,n}`|Entre `m` y `n` repeticiones|`\d{2,5}`|
|`{m,}`|Al menos `m` repeticiones|`\d{3,}`|

Por defecto, los cuantificadores son **codiciosos** (_greedy_): capturan la mayor cantidad posible de caracteres sin impedir que el resto del patrón coincida. Añade `?` para hacerlos no codiciosos (_lazy_).

## Ejemplo funcional

```python
import re

texto = "A12 B123 C12345"

print(re.findall(r"\d+", texto))
# ['12', '123', '12345']

print(re.findall(r"\d{2,3}", texto))
# ['12', '123', '123', '45']

print(re.findall(r"<.*>", "<b>Hola</b>"))
# ['<b>Hola</b>']

print(re.findall(r"<.*?>", "<b>Hola</b>"))
# ['<b>', '</b>']
```

**Reglas:**

- Valida longitudes exactas con `{n}`.
    
- Limita entradas con `{m,n}`.
    
- Prefiere cuantificadores acotados cuando conozcas la longitud esperada.
    
- Usa `*?`, `+?` o `??` para reducir la coincidencia cuando corresponda.
    

**Referencia:** [Documentación oficial de](https://docs.python.org/3.14/library/re.html#regular-expression-syntax) `[re](https://docs.python.org/3.14/library/re.html#regular-expression-syntax)` [— Python 3.14](https://docs.python.org/3.14/library/re.html#regular-expression-syntax).