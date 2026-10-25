# `pathlib` — Comprobar existencia y tipo de ruta

Utiliza `exists()`, `is_file()` e `is_dir()` para verificar la existencia de una ruta y distinguir archivos de directorios.

```python
from pathlib import Path

ruta = Path("datos/informe.csv")

if ruta.exists():
    if ruta.is_file():
        print("Es un archivo")
    elif ruta.is_dir():
        print("Es un directorio")
else:
    print("La ruta no existe")
```

- `exists()`: comprueba si la ruta existe.
    
- `is_file()`: comprueba si apunta a un archivo regular.
    
- `is_dir()`: comprueba si apunta a un directorio.
    
- No confundas una ruta inexistente con una ruta inaccesible: los permisos y errores del sistema pueden afectar las comprobaciones.
    

**Referencia:** Python 3.14 · Biblioteca estándar `pathlib`.