# `pathlib` — Gestionar errores del sistema de archivos

Captura las excepciones de operaciones con archivos para controlar rutas inexistentes, permisos insuficientes y otros fallos del sistema operativo.

```python
from pathlib import Path

ruta = Path("datos/informe.txt")

try:
    contenido = ruta.read_text(encoding="utf-8")
except FileNotFoundError:
    print("El archivo no existe.")
except PermissionError:
    print("Permisos insuficientes.")
except OSError as error:
    print(f"Error del sistema: {error}")
```

- `FileNotFoundError`: la ruta requerida no existe.
    
- `PermissionError`: el sistema deniega el acceso por permisos.
    
- `OSError`: captura otros errores de operaciones del sistema de archivos.
    
- Respeta el orden de las excepciones: captura primero las específicas y después `OSError`.
    
- Evita usar `except Exception` para ocultar errores inesperados.
    

**Referencia:** Python 3.14 · Biblioteca estándar `pathlib` y excepciones de `OSError`.