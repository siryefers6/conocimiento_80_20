# Copiar sobre directorios existentes con `dirs_exist_ok`

Usa `dirs_exist_ok=True` en `shutil.copytree()` para copiar un árbol de directorios sobre un destino existente.

```python
from shutil import copytree

copytree("proyecto", "respaldo/proyecto", dirs_exist_ok=True)
```

- Si el destino no existe, lo crea.
    
- Si existe, permite copiar y sobrescribir archivos coincidentes.
    
- Conserva los archivos del destino que no coincidan con los archivos copiados.
    
- No elimina archivos sobrantes del destino.
    

**Precaución:** revisa el contenido del destino antes de copiar para evitar sobrescribir archivos importantes.