# Consultar espacio en disco con `shutil.disk_usage()`

Usa `shutil.disk_usage(ruta)` para consultar el espacio total, usado y libre del sistema de archivos que contiene la ruta. Los valores se expresan en bytes.

```python
from shutil import disk_usage

uso = disk_usage(".")

print(f"Total: {uso.total / 1024**3:.2f} GB")
print(f"Usado: {uso.used / 1024**3:.2f} GB")
print(f"Libre: {uso.free / 1024**3:.2f} GB")
```

- Devuelve una tupla con los campos `total`, `used` y `free`.
    
- Acepta una ruta de archivo o directorio existente.
    
- Convierte los bytes a GB dividiendo entre (1024^3).
    
- Úsala para comprobar el espacio disponible antes de generar copias de seguridad o procesar archivos grandes.