# Excluir archivos y directorios con `shutil.ignore_patterns()`

Usa `shutil.ignore_patterns(*patrones)` con `shutil.copytree()` para excluir archivos y directorios que coincidan con patrones glob, como `*.tmp` o `__pycache__`.

```python
from shutil import copytree, ignore_patterns

copytree(
    "proyecto",
    "respaldo/proyecto",
    ignore=ignore_patterns("*.tmp", "__pycache__"),
)
```

- `*.tmp`: excluye archivos que terminan en `.tmp`.
    
- `__pycache__`: excluye directorios y entradas con ese nombre.
    
- Admite múltiples patrones.
    
- Aplica los patrones a los nombres de las entradas de cada directorio visitado; no interpreta rutas completas.
    

**Prioriza:** excluye archivos temporales, cachés y artefactos innecesarios al crear copias de seguridad.