# `pathlib` — Recorrer árboles con `Path.walk()`

Utiliza `Path.walk()` para recorrer recursivamente un directorio y obtener las carpetas y los archivos de cada nivel. Está disponible desde Python 3.12.

```python
from pathlib import Path

directorio = Path("datos")

for raiz, directorios, archivos in directorio.walk():
    print(f"Directorio: {raiz}")

    for nombre in archivos:
        print(raiz / nombre)
```

- `raiz`: directorio actual del recorrido.
    
- `directorios`: nombres de los subdirectorios encontrados.
    
- `archivos`: nombres de los archivos encontrados.
    
- `raiz / nombre`: construye la ruta completa de cada entrada.
    
- Usa `top_down=False` para recorrer primero los subdirectorios y después sus directorios superiores.
    

**Referencia:** Python 3.14 · `pathlib.Path.walk()`.