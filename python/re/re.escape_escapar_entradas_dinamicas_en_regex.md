# `re.escape()` — Escapar entradas dinámicas en Regex

Convierte los caracteres especiales de una cadena en representaciones literales para que Regex los interprete como texto, no como metacaracteres.

## Ejemplo funcional

```python
import re

entrada = "pedido.123+"

patron = re.escape(entrada)

print(patron)  # pedido\.123\+

print(bool(re.search(patron, "Código: pedido.123+")))  # True
print(bool(re.search(patron, "Código: pedidoX123")))   # False
```

Sin `re.escape()`, el punto (`.`) coincide con cualquier carácter y el signo `+` modifica la repetición del elemento anterior.

## Buscar texto literal dinámico

```python
import re

busqueda = input("Texto que buscar: ")
patron = re.compile(re.escape(busqueda))

coincidencias = patron.findall("a.b aXb a.b")
print(coincidencias)
```

**Reglas:**

- Usa `re.escape()` cuando incorpores texto externo como parte literal de un patrón.
    
- No lo apliques al patrón Regex completo si necesitas conservar sus metacaracteres.
    
- Para reemplazos con `re.sub()`, recuerda que `re.escape()` está diseñado para patrones de búsqueda, no para cadenas de reemplazo.
    

**Referencia:** [Documentación oficial de](https://docs.python.org/3.14/library/re.html#re.escape) `[re.escape()](https://docs.python.org/3.14/library/re.html#re.escape)` [— Python 3.14](https://docs.python.org/3.14/library/re.html#re.escape).