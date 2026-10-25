# Manejo seguro de coincidencias Regex — `Match` o `None`

Comprueba que `re.search()` o `re.match()` devuelvan un objeto `Match` antes de acceder a `group()`, `groups()` o `groupdict()`. Si no existe coincidencia, devuelven `None`.

## Ejemplo funcional

```python
import re

texto = "Pedido ABC-12345"
patron = re.compile(r"([A-Z]+)-(\d+)")

coincidencia = patron.search(texto)

if coincidencia:
    codigo, numero = coincidencia.groups()
    print(codigo)  # ABC
    print(numero)  # 12345
else:
    print("Formato no encontrado")
```

## Reglas esenciales

- Comprueba `if coincidencia:` antes de acceder a los grupos.
    
- Usa `groupdict()` para recuperar grupos con nombre.
    
- Recuerda que un grupo opcional puede devolver `None`, aunque exista una coincidencia general.
    
- Usa `fullmatch()` cuando necesites validar toda la entrada.
    

**Regla:** valida la existencia de la coincidencia y contempla los grupos opcionales antes de procesar los datos.

**Referencia:** [Documentación oficial de objetos](https://docs.python.org/3.14/library/re.html#match-objects) `[Match](https://docs.python.org/3.14/library/re.html#match-objects)` [— Python 3.14](https://docs.python.org/3.14/library/re.html#match-objects).