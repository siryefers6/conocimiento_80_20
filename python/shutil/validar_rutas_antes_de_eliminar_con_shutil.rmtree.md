# Validar rutas antes de eliminar con `shutil.rmtree()`

Restringe la eliminación a un directorio permitido y verifica que la ruta resuelta permanezca dentro de él antes de ejecutar `rmtree()`.

```python
from pathlib import Path
from shutil import rmtree

base = Path("temporales").resolve()
destino = (base / "sesion").resolve()

if destino != base and destino.is_relative_to(base):
    if destino.is_dir() and not destino.is_symlink():
        rmtree(destino)
```

- Usa `resolve()` para normalizar la ruta.
    
- Usa `is_relative_to()` para comprobar que el destino esté dentro del directorio permitido.
    
- Rechaza el directorio base y las rutas fuera de él.
    
- Comprueba que el destino sea un directorio real y no un enlace simbólico.
    
- **Precaución:** estas comprobaciones no eliminan todos los riesgos de condiciones de carrera. Evita que otros procesos puedan modificar las rutas durante la operación.