# Optimizar la copia de archivos con `shutil`

Usa las funciones de alto nivel de `shutil` para aprovechar las operaciones de copia optimizadas que Python puede utilizar según el sistema operativo.

```python
from shutil import copy2, copytree

copy2("datos.bin", "respaldo/datos.bin")
copytree("documentos", "respaldo/documentos")
```

- Prefiere `copy2()` para copiar archivos conservando metadatos cuando sea posible.
    
- Usa `copytree()` para copiar directorios recursivamente.
    
- Python puede utilizar operaciones de copia rápidas del sistema operativo, como `copy_file_range()` o `sendfile()`, cuando estén disponibles y sean compatibles.
    
- Evita implementar bucles manuales de lectura y escritura sin una necesidad específica.
    
- Mide el rendimiento con archivos representativos antes de optimizar.
    

**Prioriza:** utiliza `shutil` como primera opción y deja que Python seleccione el mecanismo de copia compatible.