# Grupos de captura en Regex — Extraer datos con `group()`

Usa paréntesis `(...)` para capturar partes concretas de una coincidencia y recuperarlas mediante `group()`.

```python
import re

patron = re.compile(r"([A-Z]+)-(\d+)")
coincidencia = patron.search("Pedido ABC-12345")

if coincidencia:
    print(coincidencia.group(0))  # ABC-12345: coincidencia completa
    print(coincidencia.group(1))  # ABC: primer grupo
    print(coincidencia.group(2))  # 12345: segundo grupo
    print(coincidencia.groups())  # ('ABC', '12345')
```

## Grupos con nombre

Usa `(?P<nombre>...)` para identificar cada captura y acceder a ella mediante `group("nombre")`.

```python
patron = re.compile(r"(?P<codigo>[A-Z]+)-(?P<numero>\d+)")
coincidencia = patron.search("ABC-12345")

print(coincidencia.group("codigo"))  # ABC
print(coincidencia.group("numero")) # 12345
print(coincidencia.groupdict())
# {'codigo': 'ABC', 'numero': '12345'}
```

**Reglas:**

- Usa `group(0)` para recuperar la coincidencia completa.
    
- Usa `group(1)`, `group(2)`, etc., para recuperar capturas por posición.
    
- Prefiere grupos con nombre cuando extraigas varios campos.
    
- Usa `(?:...)` para agrupar sin crear una captura.
    

**Referencia:** [Documentación oficial de](https://docs.python.org/3.14/library/re.html#match-objects) `[re](https://docs.python.org/3.14/library/re.html#match-objects)` [— Python 3.14](https://docs.python.org/3.14/library/re.html#match-objects).