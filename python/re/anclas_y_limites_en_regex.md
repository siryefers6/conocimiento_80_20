# Anclas y límites en Regex con Python (`re`)

Controla dónde debe comenzar o terminar una coincidencia sin consumir caracteres.

|Patrón|Función|
|---|---|
|`^`|Inicio de la cadena; también de cada línea con `re.MULTILINE`.|
|`$`|Final de la cadena o posición anterior al salto de línea final; con `re.MULTILINE`, también al final de cada línea.|
|`\A`|Inicio absoluto de la cadena.|
|`\Z`|Final absoluto de la cadena en Python 3.14.|
|`\b`|Límite entre un carácter `\w` y uno `\W`, o el borde de la cadena.|
|`\B`|Posición que no es un límite de palabra.|

## Ejemplo funcional

```python
import re

texto = "Pedido 12345\nPedido 67890"

print(re.findall(r"^Pedido", texto))
# ['Pedido']

print(re.findall(r"^Pedido", texto, re.MULTILINE))
# ['Pedido', 'Pedido']

print(re.findall(r"\b\d+\b", texto))
# ['12345', '67890']

print(bool(re.fullmatch(r"\d{5}", "12345")))
# True
```

**Reglas:**

- Usa `^` y `$` para patrones sensibles a líneas.
    
- Usa `\A` y `\Z` para exigir posiciones absolutas.
    
- Usa `\b` para localizar palabras completas o valores delimitados.
    
- Prefiere `re.fullmatch()` para validar toda una entrada, en lugar de combinar manualmente anclas.
    

**Referencia:** [Documentación oficial de](https://docs.python.org/3.14/library/re.html#regular-expression-syntax) `[re](https://docs.python.org/3.14/library/re.html#regular-expression-syntax)` [— Python 3.14](https://docs.python.org/3.14/library/re.html#regular-expression-syntax).