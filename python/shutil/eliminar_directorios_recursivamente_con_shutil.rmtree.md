# Eliminar directorios recursivamente con `shutil.rmtree()`

Usa `shutil.rmtree(ruta)` para eliminar un directorio y todo su contenido de forma recursiva.

```python
from pathlib import Path
from shutil import rmtree

ruta = Path("temporal").resolve()

if ruta.is_dir() and not ruta.is_symlink():
    rmtree(ruta)
```

- Elimina archivos y subdirectorios sin enviarlos a la papelera.
    
- No uses la ruta raíz del proyecto ni directorios críticos como destino.
    
- Valida la ruta antes de eliminarla; evita rutas proporcionadas directamente por usuarios.
    
- Puede generar `OSError` si faltan permisos o falla la operación.
    
- **Precaución:** la eliminación es destructiva y no se revierte automáticamente.