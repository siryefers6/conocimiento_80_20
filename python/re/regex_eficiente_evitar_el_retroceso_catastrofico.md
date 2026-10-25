# Regex eficiente — Evitar el retroceso catastrófico

Evita patrones ambiguos con cuantificadores anidados, porque pueden generar un crecimiento exponencial del tiempo de ejecución ante entradas que no coincidan.

## Patrón problemático

```python
import re

# Evita: cuantificadores anidados y alternativas ambiguas
patron = re.compile(r"(a+)+$")

# Una cadena larga de 'a' seguida de '!'
# puede provocar un retroceso excesivo.
```

Patrones como `(a+)+$` obligan al motor a explorar múltiples particiones posibles cuando falla la coincidencia.

## Alternativa segura

```python
import re

patron = re.compile(r"a+$")

print(bool(patron.fullmatch("aaaaa")))  # True
print(bool(patron.fullmatch("aaa!")))   # False
```

## Reglas de rendimiento

- Evita estructuras como `(a+)+`, `(.+)*` y alternativas solapadas con cuantificadores.
    
- Reduce la ambigüedad mediante clases específicas, delimitadores y longitudes acotadas.
    
- Usa `fullmatch()` para validar entradas completas, pero no lo consideres una solución automática al retroceso excesivo.
    
- Limita la longitud de las entradas no confiables.
    
- Para patrones complejos, prueba casos válidos, inválidos y cadenas largas adversariales.
    
- Considera un motor con soporte para límites de tiempo si procesas Regex potencialmente peligrosas de fuentes externas.
    

**Regla:** simplifica los patrones y elimina caminos de coincidencia ambiguos antes de optimizar microdetalles.

**Referencia:** [Documentación oficial de](https://docs.python.org/3.14/library/re.html) `[re](https://docs.python.org/3.14/library/re.html)` [— Python 3.14](https://docs.python.org/3.14/library/re.html).