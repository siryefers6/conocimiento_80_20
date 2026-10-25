# Copiar archivos conservando metadatos con `shutil.copy2()`

Usa `shutil.copy2(origen, destino)` para copiar un archivo y conservar sus permisos y metadatos, incluidas las fechas de acceso y modificación cuando el sistema lo permita.

```python
from shutil import copy2

copy2("datos.txt", "respaldo/datos.txt")
```

- Copia el contenido y los permisos.
    
- Intenta preservar las fechas de acceso y modificación y otros metadatos.
    
- Sobrescribe el archivo de destino si existe.
    
- No garantiza la conservación de todos los metadatos en todos los sistemas operativos.