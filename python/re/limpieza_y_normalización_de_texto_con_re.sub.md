# Limpieza y normalización de texto con `re.sub()`

Usa `re.sub()` para reemplazar patrones y normalizar cadenas. Conserva el texto original y asigna el resultado a una nueva variable.

## Ejemplo funcional

```python
import re

texto = "  Pedido   ABC-12345 \n"

# Elimina espacios al inicio y al final
texto = texto.strip()

# Reduce espacios consecutivos a uno
texto = re.sub(r"\s+", " ", texto)

# Normaliza el código a mayúsculas
texto = re.sub(
    r"[a-z]+-\d+",
    lambda m: m.group().upper(),
    texto,
    flags=re.IGNORECASE,
)

print(texto)  # Pedido ABC-12345
```

## Operaciones frecuentes

|Objetivo|Patrón o técnica|
|---|---|
|Reducir espacios consecutivos|`re.sub(r"\s+", " ", texto)`|
|Eliminar caracteres no alfanuméricos ASCII|`re.sub(r"[^A-Za-z0-9]", "", texto)`|
|Sustituir varios separadores por guion|`re.sub(r"[,;\s]+", "-", texto)`|
|Limitar sustituciones|`re.sub(patron, reemplazo, texto, count=1)`|

**Reglas:**

- Usa `strip()` para recortar los extremos y `re.sub()` para transformar patrones internos.
    
- Define explícitamente los caracteres permitidos; no elimines símbolos sin considerar su significado.
    
- Usa `re.IGNORECASE` para ignorar diferencias entre mayúsculas y minúsculas.
    

**Referencia:** [Documentación oficial de](https://docs.python.org/3.14/library/re.html#re.sub) `[re.sub()](https://docs.python.org/3.14/library/re.html#re.sub)` [— Python 3.14](https://docs.python.org/3.14/library/re.html#re.sub).