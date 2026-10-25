# Copiar directorios recursivamente con `shutil.copytree()`

Usa `shutil.copytree(origen, destino)` para copiar un directorio completo, incluidos sus archivos y subdirectorios.

```python
from shutil import copytree

copytree("documentos", "respaldo/documentos")
```

- Copia los archivos y subdirectorios de forma recursiva.
    
- El directorio de destino no debe existir, salvo que uses `dirs_exist_ok=True`.
    
- Conserva los metadatos de los archivos mediante `copy2()` por defecto.
    
- Usa `ignore=` para excluir archivos o directorios específicos.
    

```python
copytree("documentos", "respaldo/documentos", dirs_exist_ok=True)
```

**Prioriza:** utiliza `copytree()` para respaldar o duplicar estructuras completas de directorios.