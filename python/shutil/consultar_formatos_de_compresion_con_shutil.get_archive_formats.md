# Consultar formatos de compresión con `shutil.get_archive_formats()`

Usa `shutil.get_archive_formats()` para obtener los formatos de archivo que `shutil.make_archive()` tiene disponibles.

```python
from shutil import get_archive_formats

for nombre, descripcion in get_archive_formats():
    print(f"{nombre}: {descripcion}")
```

- Devuelve una lista de tuplas con el nombre y la descripción de cada formato.
    
- Los formatos disponibles dependen de la versión de Python y de los módulos compatibles.
    
- Permite comprobar qué formatos puedes utilizar antes de crear un archivo comprimido.
    

**Prioriza:** consulta los formatos disponibles cuando necesites ofrecer opciones de compresión dinámicas.