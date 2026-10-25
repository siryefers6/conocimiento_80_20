# Extraer archivos comprimidos con `shutil.unpack_archive()`

Usa `shutil.unpack_archive()` para extraer el contenido de un archivo comprimido en un directorio de destino.

```python
from shutil import unpack_archive

unpack_archive("respaldo.zip", "extraido/")
```

- Detecta automáticamente el formato compatible según la extensión si no especificas `format`.
    
- Crea el directorio de destino si no existe.
    
- Admite formatos como ZIP, TAR, GZIP, BZIP2 y XZ, según los formatos registrados y disponibles.
    
- Puede sobrescribir archivos existentes.
    
- **Seguridad:** valida el archivo y el destino antes de extraer archivos de fuentes no confiables; comprueba los riesgos de rutas maliciosas y sobrescrituras.