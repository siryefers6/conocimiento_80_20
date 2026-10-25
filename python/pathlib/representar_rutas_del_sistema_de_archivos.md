# `Path` — Representar rutas del sistema de archivos

Utiliza `Path` de `pathlib` para representar rutas de archivos y directorios mediante objetos. Evita concatenar cadenas manualmente y prefiere esta API a `os.path` en código nuevo.

```python
from pathlib import Path

archivo = Path("datos") / "informe.csv"
directorio = Path.home() / "Documentos"

print(archivo)
print(directorio)
```

- Combina rutas con `/`.
    
- Obtén la ruta del directorio personal con `Path.home()`.
    
- Usa rutas relativas o absolutas según el contexto.
    
- No instales paquetes: `pathlib` pertenece a la biblioteca estándar de Python 3.14.