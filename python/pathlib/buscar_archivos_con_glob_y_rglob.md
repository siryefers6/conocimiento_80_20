# `pathlib` — Buscar archivos con `glob()` y `rglob()`

Utiliza `glob()` para buscar rutas que coincidan con un patrón en un directorio y `rglob()` para incluir sus subdirectorios recursivamente.

```python
from pathlib import Path

directorio = Path("datos")

# Buscar archivos CSV en el nivel inmediato
for archivo in directorio.glob("*.csv"):
    print(archivo)

# Buscar archivos CSV en todos los subdirectorios
for archivo in directorio.rglob("*.csv"):
    print(archivo)
```

- `glob("*.csv")`: busca coincidencias en el nivel inmediato.
    
- `rglob("*.csv")`: busca coincidencias recursivamente.
    
- `*.csv`: coincide con nombres que terminan en `.csv`.
    
- `**/*.csv`: patrón recursivo equivalente al uso habitual de `rglob("*.csv")`.
    
- Filtra por tipo con `archivo.is_file()` si necesitas excluir directorios coincidentes.
    

**Referencia:** Python 3.14 · `pathlib.Path.glob()` y `pathlib.Path.rglob()`.