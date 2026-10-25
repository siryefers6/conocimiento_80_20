# `pathlib` — Listar contenido con `iterdir()`

Utiliza `iterdir()` para recorrer las entradas inmediatas de un directorio. Devuelve un iterador de objetos `Path`, sin recorrer subdirectorios recursivamente.

```python
from pathlib import Path

directorio = Path("datos")

for ruta in directorio.iterdir():
    print(ruta.name)
```

- Usa `iterdir()` para listar archivos y subdirectorios del primer nivel.
    
- Comprueba `ruta.is_file()` o `ruta.is_dir()` para distinguirlos.
    
- Verifica `directorio.is_dir()` antes de recorrerlo si la ruta puede ser inválida.
    
- No asumas un orden específico: aplica `sorted()` si necesitas ordenar los resultados.
    

```python
for ruta in sorted(directorio.iterdir()):
    print(ruta)
```

**Referencia:** Python 3.14 · `pathlib.Path.iterdir()`.