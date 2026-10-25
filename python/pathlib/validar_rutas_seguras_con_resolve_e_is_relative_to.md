# `pathlib` — Validar rutas seguras con `resolve()` e `is_relative_to()`

Resuelve la ruta y comprueba que permanezca dentro del directorio permitido antes de leer o modificar archivos. Evita validar rutas mediante concatenaciones de cadenas.

```python
from pathlib import Path

base = Path("archivos_permitidos").resolve()
nombre = "../secreto.txt"
ruta = (base / nombre).resolve()

if ruta.is_relative_to(base):
    print("Ruta permitida")
else:
    print("Ruta fuera del directorio permitido")
```

- `resolve()`: normaliza la ruta, resuelve enlaces simbólicos y elimina componentes `..`.
    
- `is_relative_to(base)`: comprueba si la ruta está dentro de `base`, incluida la propia base.
    
- Valida la ruta **después** de resolverla.
    
- No uses `str.startswith()` para validar rutas: puede aceptar directorios con nombres similares.
    
- Considera que una validación previa no elimina las condiciones de carrera si otro proceso modifica enlaces o directorios antes de operar.
    

**Referencia:** Python 3.14 · `Path.resolve()` e `is_relative_to()`.