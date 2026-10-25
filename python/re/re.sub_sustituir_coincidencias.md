# `re.sub()` — Sustituir coincidencias en Python

Reemplaza todas las coincidencias no superpuestas de un patrón por un texto o el resultado de una función. Devuelve una nueva cadena sin modificar la original.

```python
import re

texto = "Pedido 12345 y Pedido 67890"

resultado = re.sub(r"\d+", "XXXXX", texto)
print(resultado)
# Pedido XXXXX y Pedido XXXXX
```

## Sustitución con grupos

Usa referencias como `\1` para reutilizar el contenido capturado.

```python
texto = "ABC-12345"

resultado = re.sub(r"([A-Z]+)-(\d+)", r"\2-\1", texto)
print(resultado)  # 12345-ABC
```

**Parámetros clave:**

- `pattern`: patrón que debes buscar.
    
- `repl`: texto o función de reemplazo.
    
- `count`: limita el número de sustituciones; `0` significa todas.
    

**Regla:** usa `re.sub()` para normalizar formatos, ocultar datos o transformar texto mediante patrones.

**Referencia:** [Documentación oficial de](https://docs.python.org/3.14/library/re.html#re.sub) `[re.sub()](https://docs.python.org/3.14/library/re.html#re.sub)` [— Python 3.14](https://docs.python.org/3.14/library/re.html#re.sub).