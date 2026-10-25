# Crear archivos comprimidos con `shutil.make_archive()`

Usa `shutil.make_archive()` para comprimir directorios en formatos como ZIP o TAR.

```python
from shutil import make_archive

make_archive(
    "respaldo",
    "zip",
    root_dir="proyecto",
)
```

- `base_name`: nombre y ruta del archivo de salida, sin extensión.
    
- `format`: formato de compresión, como `"zip"`, `"tar"`, `"gztar"` o `"bztar"`.
    
- `root_dir`: directorio cuyo contenido se incluirá.
    
- Devuelve la ruta del archivo creado.
    

**Prioriza:** genera archivos comprimidos para respaldar, transferir o distribuir directorios.