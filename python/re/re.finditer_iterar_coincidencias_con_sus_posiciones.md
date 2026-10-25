# `re.finditer()` — Iterar coincidencias con sus posiciones

Devuelve un iterador de objetos `Match` para cada coincidencia no superpuesta. Obtén el texto, la posición inicial y la final sin crear una lista de resultados.

```python
import re

texto = "Pedido 12345, Pedido 67890"

for coincidencia in re.finditer(r"\d+", texto):
    print(coincidencia.group(), coincidencia.start(), coincidencia.end())

# 12345 7 12
# 67890 21 26
```

**Usa** `**re.finditer()**` **para:**

- Obtener todas las coincidencias junto con sus posiciones.
    
- Procesar resultados de forma iterativa.
    
- Identificar fragmentos de texto que debas reemplazar o analizar.
    

**Distingue:** `re.findall()` devuelve una lista; `re.finditer()` devuelve un iterador de objetos `Match` con métodos como `.group()`, `.start()` y `.end()`.

**Referencia:** [Documentación oficial de](https://docs.python.org/3.14/library/re.html#re.finditer) `[re.finditer()](https://docs.python.org/3.14/library/re.html#re.finditer)` [— Python 3.14](https://docs.python.org/3.14/library/re.html#re.finditer).