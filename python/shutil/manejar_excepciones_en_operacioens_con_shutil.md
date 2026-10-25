# Manejar excepciones en operaciones con `shutil`

Controla los errores de `shutil` para detectar fallos al copiar, mover o eliminar archivos y directorios.

```python
import shutil

try:
    shutil.copyfile("origen.txt", "destino.txt")
except shutil.SameFileError:
    print("El origen y el destino son el mismo archivo.")
except shutil.Error as exc:
    print(f"Error de copia: {exc}")
except OSError as exc:
    print(f"Error del sistema de archivos: {exc}")
```

- `shutil.SameFileError`: intenta copiar un archivo sobre sí mismo.
    
- `shutil.Error`: agrupa errores de operaciones de alto nivel, como los errores de copia acumulados por `copytree()`.
    
- `OSError`: permisos insuficientes, rutas inexistentes u otros fallos del sistema de archivos.
    
- **Ordena las excepciones de específicas a generales**, porque `SameFileError` deriva de `Error`, y `Error` deriva de `OSError`.
    

**Prioriza:** captura únicamente los errores que puedas manejar; evita ocultarlos con un `except Exception` genérico.