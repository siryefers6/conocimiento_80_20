# `pathlib` — Eliminar, renombrar y reemplazar rutas

Utiliza `unlink()` para eliminar archivos, `rmdir()` para eliminar directorios vacíos, `rename()` para renombrar o mover rutas y `replace()` para reemplazar el destino si existe.

```python
from pathlib import Path

archivo = Path("datos.txt")
archivo.unlink(missing_ok=True)  # Elimina el archivo si existe

directorio = Path("temporal")
if directorio.is_dir():
    directorio.rmdir()  # Solo si está vacío

origen = Path("informe.txt")
origen.rename(Path("informe_antiguo.txt"))

origen = Path("nuevo.txt")
origen.replace(Path("informe.txt"))
```

- `unlink()`: elimina un archivo o enlace simbólico; `missing_ok=True` tolera que no exista.
    
- `rmdir()`: elimina un directorio únicamente si está vacío.
    
- `rename(destino)`: cambia el nombre o la ubicación; el tratamiento de un destino existente depende del sistema operativo.
    
- `replace(destino)`: reemplaza el destino si existe, sujeto a las restricciones del sistema de archivos.
    

**Precaución:** comprueba las rutas antes de eliminar o reemplazar archivos. Estas operaciones pueden provocar pérdida de datos.

**Referencia:** Python 3.14 · `pathlib.Path.unlink()`, `rmdir()`, `rename()` y `replace()`.