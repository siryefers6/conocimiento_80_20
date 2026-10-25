# `pathlib` — Crear directorios con `mkdir()`

Utiliza `mkdir()` para crear directorios. Activa `parents=True` para crear los directorios intermedios y `exist_ok=True` para evitar errores cuando el directorio ya existe.

```python
from pathlib import Path

directorio = Path("datos/informes/2026")

directorio.mkdir(parents=True, exist_ok=True)
```

- `parents=True`: crea los directorios superiores que falten.
    
- `exist_ok=True`: no genera un error si el directorio de destino ya existe.
    
- Omite ambos parámetros cuando necesites detectar esas condiciones.
    
- Recuerda que `mkdir()` crea un directorio, no un archivo.
    

**Referencia:** Python 3.14 · `pathlib.Path.mkdir()`.