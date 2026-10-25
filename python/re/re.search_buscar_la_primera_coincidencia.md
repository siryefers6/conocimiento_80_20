# `re.search()` — Buscar la primera coincidencia en Python

Localiza la primera coincidencia de un patrón en cualquier posición de la cadena. Devuelve un objeto `Match` o `None` si no encuentra resultados.

```python
import re

texto = "Pedido pendiente: 12345"

coincidencia = re.search(r"\d+", texto)

if coincidencia:
    print(coincidencia.group())  # '12345'
    print(coincidencia.start())  # 18
else:
    print("Sin coincidencias")
```

**Usa** `**re.search()**` **para:**

- Encontrar texto dentro de una cadena.
    
- Obtener la posición con `.start()` y `.end()`.
    
- Recuperar el texto coincidente con `.group()`.
    

**Distingue:** `re.search()` busca en cualquier posición; `re.match()` comprueba desde el inicio y `re.fullmatch()` exige que coincida toda la cadena.

**Referencia:** [Documentación oficial de](https://docs.python.org/3.14/library/re.html#re.search) `[re.search()](https://docs.python.org/3.14/library/re.html#re.search)` [— Python 3.14](https://docs.python.org/3.14/library/re.html#re.search).