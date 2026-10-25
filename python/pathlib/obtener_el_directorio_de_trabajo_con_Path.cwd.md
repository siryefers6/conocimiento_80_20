# `pathlib` — Obtener el directorio de trabajo con `Path.cwd()`

Utiliza `Path.cwd()` para obtener el directorio de trabajo actual del proceso. No lo confundas con el directorio donde está guardado el archivo Python.

```python
from pathlib import Path

directorio_trabajo = Path.cwd()
directorio_script = Path(__file__).resolve().parent

print(directorio_trabajo)
print(directorio_script)
```

- `Path.cwd()`: devuelve el directorio desde el que se ejecuta el proceso.
    
- `Path(__file__).resolve().parent`: obtiene el directorio del script; `__file__` no está disponible en todos los entornos, como algunas consolas interactivas.
    
- Resuelve las rutas relativas respecto al directorio de trabajo, no respecto al archivo Python.
    
- No cambies el directorio de trabajo para corregir rutas mal construidas; define explícitamente la base que necesitas.
    

**Referencia:** Python 3.14 · `pathlib.Path.cwd()`.