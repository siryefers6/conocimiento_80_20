# Diferencias de metadatos entre `copy()`, `copy2()` y `copyfile()`

Selecciona la función de copia según los metadatos que necesites preservar.

| Función      | Contenido | Permisos | Fechas y otros metadatos      |
| ------------ | --------- | -------- | ----------------------------- |
| `copyfile()` | Sí        | No       | No                            |
| `copy()`     | Sí        | Sí       | No intenta preservarlos todos |
| `copy2()`    | Sí        | Sí       | Intenta preservarlos          |

```python
from shutil import copy, copy2, copyfile

copyfile("origen.txt", "contenido.txt")
copy("origen.txt", "permisos.txt")
copy2("origen.txt", "metadatos.txt")
```

- Usa `copyfile()` para copiar únicamente el contenido.
    
- Usa `copy()` para copiar el contenido y los permisos.
    
- Usa `copy2()` para intentar conservar también las fechas de acceso y modificación y otros metadatos compatibles.
    
- No asumas que ninguna función preserva todos los metadatos; el resultado depende del sistema operativo y del sistema de archivos.