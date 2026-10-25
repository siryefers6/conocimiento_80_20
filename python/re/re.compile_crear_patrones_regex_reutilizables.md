# `re.compile()` — Crear patrones Regex reutilizables

Compila una expresión regular en un objeto `Pattern` para reutilizarla en múltiples operaciones sin repetir el patrón.

```python
import re

patron = re.compile(r"\d{5}")

print(patron.findall("Pedidos: 12345, 67890"))
# ['12345', '67890']

print(bool(patron.fullmatch("12345")))
# True

print(bool(patron.fullmatch("123")))
# False
```

## Métodos principales

- `patron.search(texto)`: busca la primera coincidencia.
    
- `patron.findall(texto)`: extrae todas las coincidencias.
    
- `patron.finditer(texto)`: itera sobre las coincidencias.
    
- `patron.sub(reemplazo, texto)`: sustituye coincidencias.
    
- `patron.fullmatch(texto)`: valida toda la cadena.
    

**Regla:** compila el patrón cuando debas reutilizarlo o asociarle opciones, como `re.IGNORECASE`. Para expresiones usadas una sola vez, llama directamente a las funciones de `re`.

**Referencia:** [Documentación oficial de](https://docs.python.org/3.14/library/re.html#re.compile) `[re.compile()](https://docs.python.org/3.14/library/re.html#re.compile)` [— Python 3.14](https://docs.python.org/3.14/library/re.html#re.compile).