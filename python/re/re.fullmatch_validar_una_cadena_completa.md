# `re.fullmatch()` — Validar una cadena completa

Exige que toda la cadena coincida con el patrón. Devuelve un objeto `Match` si cumple la expresión o `None` si no la cumple.

```python
import re

patron = r"\d{5}"

print(bool(re.fullmatch(patron, "12345")))   # True
print(bool(re.fullmatch(patron, "123")))     # False
print(bool(re.fullmatch(patron, "12345abc"))) # False
```

**Usa** `**re.fullmatch()**` **para:**

- Validar códigos, identificadores y formatos de entrada.
    
- Rechazar caracteres adicionales al inicio o al final.
    
- Comprobar formatos sin construir manualmente anclas `^` y `$`.
    

**Regla:** valida entradas completas con `re.fullmatch()`; no uses `re.search()` cuando necesites validar todo el texto.

**Referencia:** [Documentación oficial de](https://docs.python.org/3.14/library/re.html#re.fullmatch) `[re.fullmatch()](https://docs.python.org/3.14/library/re.html#re.fullmatch)` [— Python 3.14](https://docs.python.org/3.14/library/re.html#re.fullmatch).