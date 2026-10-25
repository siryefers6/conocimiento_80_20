# Cadenas raw en Python — `r"..."` para Regex

Usa el prefijo `r` para conservar las barras invertidas (`\`) en una cadena sin interpretarlas como escapes de Python. Simplifica los patrones de expresiones regulares.

```python
import re

texto = "Pedido: 12345"

# Cadena raw: recomendada para Regex
patron = r"\d+"
print(re.findall(patron, texto))  # ['12345']

# Sin cadena raw: requiere escapar la barra invertida
patron = "\\d+"
print(re.findall(patron, texto))  # ['12345']
```

**Regla:** escribe los patrones Regex con `r"..."` o `r'...'`. Recuerda que las cadenas raw no permiten terminar con una cantidad impar de barras invertidas y que no desactivan la interpretación de comillas delimitadoras.

**Referencia:** [Documentación oficial — Cadenas literales de Python 3.14](https://docs.python.org/3.14/reference/lexical_analysis.html#string-and-bytes-literals).