# `pathlib` — Construir rutas con el operador `/`

Combina directorios y nombres de archivos mediante `/` para construir rutas legibles y multiplataforma. Evita concatenar cadenas manualmente.

```python
from pathlib import Path

ruta = Path("datos") / "informes" / "ventas.csv"
destino = Path.home() / "Documentos" / "reporte.pdf"

print(ruta)
print(destino)
```

- Encadena `/` para añadir componentes.
    
- Combina objetos `Path` con cadenas.
    
- Deja que Python gestione los separadores del sistema operativo.
    
- No confundas construir una ruta con crear el archivo o directorio: `/` solo compone la ruta.