# Unicode y límites de caracteres en Regex — `\w`, `\d` y `\b`

En Python, las expresiones regulares trabajan con Unicode por defecto para patrones de tipo `str`. Las clases `\w`, `\d` y los límites `\b` pueden comportarse de forma distinta a las reglas ASCII.

## Diferencias esenciales

|Patrón|Comportamiento Unicode predeterminado|Con `re.ASCII`|
|---|---|---|
|`\w`|Letras Unicode, dígitos y `_`|`[a-zA-Z0-9_]`|
|`\d`|Dígitos decimales Unicode|`[0-9]`|
|`\b`|Límite entre `\w` y `\W`|Límite según la definición ASCII de `\w`|

## Ejemplo funcional

```python
import re

texto = "café 123 ١٢٣"

print(re.findall(r"\w+", texto))
# ['café', '123', '١٢٣']

print(re.findall(r"\d+", texto))
# ['123', '١٢٣']

print(re.findall(r"\d+", texto, re.ASCII))
# ['123']

print(re.findall(r"\w+", texto, re.ASCII))
# ['caf', '123']
```

## Aplicación práctica

- Usa `\d` cuando aceptes dígitos decimales Unicode.
    
- Usa `[0-9]` para validar números compuestos exclusivamente por dígitos ASCII.
    
- Usa `re.ASCII` cuando necesites restringir globalmente las clases abreviadas a ASCII.
    
- Recuerda que `\b` depende de `\w`; no representa necesariamente un límite entre palabras lingüísticas.
    

**Regla:** define explícitamente si el formato admite Unicode o solo ASCII, especialmente al validar códigos, identificadores y números de pedido.

**Referencia:** [Documentación oficial de Regex — Python 3.14](https://docs.python.org/3.14/library/re.html#regular-expression-syntax).