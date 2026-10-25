# `pathlib` — Consultar metadatos con `stat()`

Utiliza `stat()` para obtener los metadatos de un archivo o directorio. Accede al tamaño, las fechas y los permisos mediante los atributos de `os.stat_result`.

```python
from pathlib import Path
from datetime import datetime

ruta = Path("informe.csv")
datos = ruta.stat()

print(f"Tamaño: {datos.st_size} bytes")
print(f"Modificado: {datetime.fromtimestamp(datos.st_mtime)}")
print(f"Creado en el sistema: {datetime.fromtimestamp(datos.st_ctime)}")
print(f"Permisos: {oct(datos.st_mode)}")
```

- `st_size`: tamaño en bytes.
    
- `st_mtime`: fecha de última modificación, como marca de tiempo Unix.
    
- `st_ctime`: fecha de cambio de metadatos en Unix; en Windows representa normalmente la fecha de creación.
    
- `st_mode`: tipo de archivo y bits de permisos.
    
- `st_atime`: fecha del último acceso.
    

**Considera:** `stat()` puede generar `FileNotFoundError` si la ruta no existe. Para evitar seguir enlaces simbólicos, utiliza `lstat()`.

**Referencia:** Python 3.14 · `pathlib.Path.stat()`.