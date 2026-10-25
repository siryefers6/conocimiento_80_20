# `pathlib` — Extraer componentes de una ruta

Utiliza las propiedades de `Path` para obtener el nombre del archivo, su extensión y los directorios superiores sin manipular cadenas manualmente.

```python
from pathlib import Path

ruta = Path("datos/informes/ventas.csv")

print(ruta.name)       # ventas.csv
print(ruta.stem)       # ventas
print(ruta.suffix)     # .csv
print(ruta.parent)     # datos/informes
print(list(ruta.parents))
# [Path('datos/informes'), Path('datos'), Path('.')]
```

- `name`: nombre completo del archivo o directorio.
    
- `stem`: nombre sin la última extensión.
    
- `suffix`: última extensión, incluido el punto.
    
- `parent`: directorio inmediato superior.
    
- `parents`: secuencia de todos los directorios superiores.
    

**Considera:** para `archivo.tar.gz`, `stem` devuelve `archivo.tar` y `suffix` devuelve `.gz`.