# Validación de formatos con Regex — `re.fullmatch()`

Valida identificadores, códigos y números de pedido mediante patrones que definan el formato permitido. Usa `fullmatch()` para exigir que toda la entrada cumpla el patrón.

## Ejemplo funcional

```python
import re

PATRON_PEDIDO = re.compile(r"\d{8}")

def validar_pedido(valor: str) -> bool:
    return PATRON_PEDIDO.fullmatch(valor) is not None

print(validar_pedido("12345678"))  # True
print(validar_pedido("1234567"))   # False
print(validar_pedido("12345678A")) # False
```

## Patrones frecuentes

|Formato|Regex|
|---|---|
|Pedido de 8 dígitos|`\d{8}`|
|Código alfanumérico ASCII|`[A-Z0-9]{6,12}`|
|Identificador con guion|`[A-Z]{3}-\d{5}`|
|Número entero positivo|`[1-9]\d*`|

**Reglas:**

- Define primero la longitud y los caracteres permitidos.
    
- Usa `fullmatch()` para rechazar prefijos y sufijos adicionales.
    
- Normaliza mayúsculas o espacios solo si las reglas del negocio lo permiten.
    
- Recuerda que `\d` admite dígitos Unicode; usa `[0-9]` si necesitas exclusivamente dígitos ASCII.
    

**Referencia:** [Documentación oficial de](https://docs.python.org/3.14/library/re.html#re.fullmatch) `[re.fullmatch()](https://docs.python.org/3.14/library/re.html#re.fullmatch)` [— Python 3.14](https://docs.python.org/3.14/library/re.html#re.fullmatch).