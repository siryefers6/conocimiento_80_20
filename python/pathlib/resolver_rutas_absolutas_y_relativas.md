# `pathlib` — Resolver rutas absolutas y relativas

Convierte rutas relativas en absolutas con `Path.resolve()` o `Path.absolute()`. Prefiere `resolve()` cuando necesites normalizar componentes `..` y resolver enlaces simbólicos.

```python
from pathlib import Path

ruta = Path("datos") / ".." / "informe.csv"

print(ruta.absolute())  # Ruta absoluta sin resolver necesariamente los enlaces
print(ruta.resolve())   # Ruta absoluta normalizada y enlaces resueltos
```

- Usa `resolve()` para obtener una ruta canónica según el sistema de archivos.
    
- Usa `absolute()` cuando necesites una ruta absoluta sin exigir la resolución de enlaces simbólicos.
    
- Recuerda que `resolve()` puede resolver enlaces simbólicos y eliminar componentes `..`.
    
- No confundas una ruta absoluta con una ruta existente: obtenerla no crea el archivo ni el directorio.
    

**Referencia:** Python 3.14 · `pathlib.Path.resolve()` y `pathlib.Path.absolute()`.