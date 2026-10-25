# `re.split()` — Dividir texto mediante Regex

Divide una cadena cada vez que encuentra una coincidencia del patrón y devuelve una lista de fragmentos.

```python
import re

texto = "Python,FastAPI;SQLModel HTMX"

resultado = re.split(r"[,;\s]+", texto)
print(resultado)
# ['Python', 'FastAPI', 'SQLModel', 'HTMX']
```

## Parámetros clave

- `pattern`: patrón que identifica los separadores.
    
- `string`: texto que debes dividir.
    
- `maxsplit`: número máximo de divisiones; `0` significa sin límite.
    

```python
print(re.split(r"\d+", "ABC123DEF456GHI", maxsplit=1))
# ['ABC', 'DEF456GHI']
```

**Reglas:**

- Combina separadores con `|` o clases de caracteres.
    
- Usa `+` para agrupar separadores consecutivos y evitar fragmentos vacíos.
    
- Recuerda que los grupos capturadores del patrón incluyen los separadores coincidentes en el resultado.
    

**Referencia:** [Documentación oficial de](https://docs.python.org/3.14/library/re.html#re.split) `[re.split()](https://docs.python.org/3.14/library/re.html#re.split)` [— Python 3.14](https://docs.python.org/3.14/library/re.html#re.split).