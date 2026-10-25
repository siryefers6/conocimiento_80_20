# Copiar contenido de archivos con `shutil.copyfile()`

Usa `shutil.copyfile(origen, destino)` para copiar únicamente el contenido de un archivo, sin conservar sus permisos ni otros metadatos.

```python
from shutil import copyfile

copyfile("datos.txt", "respaldo/datos.txt")
```

- Sobrescribe el destino si existe.
    
- Exige que el directorio de destino exista.
    
- Genera `SameFileError` si el origen y el destino representan el mismo archivo.
    
- No copia archivos especiales ni directorios.