# Compatibilidad entre Windows y Linux al manipular archivos

Usa `pathlib` y las funciones de `shutil` para reducir diferencias entre sistemas operativos. Verifica especialmente las rutas, los permisos y los enlaces simbólicos.

```python
from pathlib import Path
from shutil import copy2

origen = Path("datos") / "archivo.txt"
destino = Path("respaldo") / "archivo.txt"

destino.parent.mkdir(parents=True, exist_ok=True)
copy2(origen, destino, follow_symlinks=False)
```

- **Rutas:** construye rutas con `Path` en lugar de concatenar separadores manualmente.
    
- **Permisos:** Windows y Linux interpretan y aplican los permisos de forma diferente; no asumas que `chmod()` tiene el mismo efecto.
    
- **Enlaces simbólicos:** comprueba `follow_symlinks`; su comportamiento depende de la función y del soporte del sistema.
    
- **Archivos abiertos:** Windows suele restringir más ciertas operaciones sobre archivos abiertos que Linux.
    
- **Pruebas:** ejecuta pruebas en ambos sistemas si la aplicación debe funcionar en ambos.
    

**Prioriza:** evita asumir que una operación de archivos tendrá idénticos efectos en Windows y Linux.