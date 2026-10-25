# Copiar archivos con `shutil.copy()`

Usa `shutil.copy(origen, destino)` para copiar un archivo y conservar sus permisos. El destino puede ser una ruta de archivo o un directorio existente.

```python
from shutil import copy

copy("datos.txt", "respaldo/datos.txt")
```

- Copia el contenido y los permisos.
    
- No conserva todos los metadatos; usa `shutil.copy2()` si necesitas preservar también las fechas de modificación y acceso.
    
- Sobrescribe el archivo de destino si existe.
    
- Crea previamente el directorio de destino si no existe.