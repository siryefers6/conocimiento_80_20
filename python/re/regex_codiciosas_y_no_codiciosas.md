# Regex codiciosas y no codiciosas — `.*` frente a `.*?`

Controla cuánto texto captura un cuantificador. Por defecto, `*`, `+` y `{m,n}` son **codiciosos**: buscan la coincidencia más larga que permita completar el patrón. Añade `?` para hacerlos **no codiciosos**: buscan la más corta que permita completar el patrón.

## Ejemplo funcional

```python
import re

texto = "<b>Uno</b><b>Dos</b>"

print(re.findall(r"<b>.*</b>", texto))
# ['<b>Uno</b><b>Dos</b>']

print(re.findall(r"<b>.*?</b>", texto))
# ['<b>Uno</b>', '<b>Dos</b>']
```

## Diferencias

|Patrón|Comportamiento|
|---|---|
|`.*`|Captura la mayor cantidad posible de texto.|
|`.*?`|Captura la menor cantidad posible que permita completar el patrón.|
|`.+?`|Captura al menos un carácter, intentando minimizar la coincidencia.|
|`\d{2,5}?`|Intenta usar la menor cantidad de dígitos permitida.|

**Reglas:**

- Prefiere patrones específicos, como `[^<]*`, cuando conozcas los delimitadores.
    
- Usa `.*?` cuando necesites coincidencias mínimas entre delimitadores.
    
- No confundas una coincidencia no codiciosa con una garantía de rendimiento: los patrones ambiguos pueden provocar retroceso excesivo.
    
- No uses Regex como analizador general de HTML; utiliza un parser específico.
    

**Referencia:** [Documentación oficial de Regex — Python 3.14](https://docs.python.org/3.14/library/re.html#regular-expression-syntax).