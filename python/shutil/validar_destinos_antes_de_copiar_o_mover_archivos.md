# Validar destinos antes de copiar o mover archivos

Comprueba si el destino existe antes de ejecutar `shutil.copy2()` o `shutil.move()` para evitar sobrescrituras accidentales.

```python
from pathlib import Path
from shutil import copy2, move

origen = Path("datos.txt")
destino = Path("respaldo/datos.txt")

if origen.is_file() and not destino.exists():
    destino.parent.mkdir(parents=True, exist_ok=True)
    copy2(origen, destino)
else:
    raise FileExistsError(f"Origen inválido o destino existente: {destino}")
```

- Usa `exists()` para detectar archivos o directorios existentes.
    
- Usa `is_file()` para validar el origen.
    
- Crea el directorio padre con `mkdir()` si es necesario.
    
- Aplica la misma validación antes de mover archivos con `move()`.
    
- **Precaución:** comprobar y copiar son operaciones separadas; otro proceso podría crear el destino entre ambas. Para evitar sobrescrituras de forma robusta, utiliza una estrategia de creación exclusiva.