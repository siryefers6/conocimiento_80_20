# `re.findall()` — Extraer todas las coincidencias

Encuentra todas las coincidencias no superpuestas de un patrón y devuelve una lista. Si no encuentra coincidencias, devuelve una lista vacía.

```python
import re

texto = "Pedidos: 12345, 67890 y 54321"

print(re.findall(r"\d+", texto))
# ['12345', '67890', '54321']
```

**Captura grupos:**

- Sin grupos: devuelve las coincidencias completas.
    
- Un grupo capturador: devuelve una lista con sus contenidos.
    
- Varios grupos capturadores: devuelve una lista de tuplas.
    

```python
texto = "ABC-123 DEF-456"

print(re.findall(r"([A-Z]+)-(\d+)", texto))
# [('ABC', '123'), ('DEF', '456')]
```

**Regla:** usa `re.findall()` para extraer todos los resultados; usa `re.search()` cuando solo necesites la primera coincidencia.

**Referencia:** [Documentación oficial de](https://docs.python.org/3.14/library/re.html#re.findall) `[re.findall()](https://docs.python.org/3.14/library/re.html#re.findall)` [— Python 3.14](https://docs.python.org/3.14/library/re.html#re.findall).