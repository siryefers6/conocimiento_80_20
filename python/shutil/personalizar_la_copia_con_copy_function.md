# Personalizar la copia con `copy_function`

Usa `copy_function` en `shutil.copytree()` o `shutil.move()` para definir qué función copiará los archivos. La función debe aceptar las rutas de origen y destino.

```python
from shutil import copy2, copytree

copytree(
    "proyecto",
    "respaldo/proyecto",
    copy_function=copy2,
)
```

- `copy2`: conserva permisos y trata de preservar otros metadatos.
    
- `copy`: conserva permisos, pero no intenta preservar tantos metadatos.
    
- Puedes proporcionar una función propia que cumpla la misma interfaz.
    
- En `move()`, la función se utiliza cuando el movimiento requiere copiar y eliminar, por ejemplo, entre sistemas de archivos.
    

**Prioriza:** usa `copy_function` cuando necesites controlar cómo se copian los archivos durante operaciones recursivas.