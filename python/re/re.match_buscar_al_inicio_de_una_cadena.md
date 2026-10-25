# `re.match()` — Buscar al inicio de una cadena

Comprueba si el patrón coincide desde la posición inicial de la cadena. Devuelve un objeto `Match` o `None`.

```python
import re

texto = "Pedido 12345"

resultado = re.match(r"Pedido\s+\d+", texto)

print(resultado.group() if resultado else "Sin coincidencia")
# Pedido 12345

print(re.match(r"\d+", texto))
# None
```

**Usa** `**re.match()**` **para:**

- Comprobar prefijos y formatos que comiencen al inicio del texto.
    
- Recuperar la coincidencia con `.group()`.
    
- Obtener posiciones con `.start()` y `.end()`.
    

**Distingue:** `re.match()` busca desde el inicio; `re.search()` busca en cualquier posición; `re.fullmatch()` exige que coincida toda la cadena.

**Referencia:** [Documentación oficial de](https://docs.python.org/3.14/library/re.html#re.match) `[re.match()](https://docs.python.org/3.14/library/re.html#re.match)` [— Python 3.14](https://docs.python.org/3.14/library/re.html#re.match).