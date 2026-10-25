# `datetime` — Convertir marcas temporales en fechas

Convierte `st_mtime`, expresado como segundos desde la época Unix, en un objeto `datetime` mediante `datetime.fromtimestamp()` para mostrar fechas legibles en informes.

```python
from pathlib import Path
from datetime import datetime

ruta = Path("informe.csv")
modificado = datetime.fromtimestamp(ruta.stat().st_mtime)

print(modificado.strftime("%d-%m-%Y %H:%M:%S"))
```

- `st_mtime`: obtiene la marca temporal de la última modificación.
    
- `datetime.fromtimestamp()`: convierte la marca temporal a la hora local del sistema.
    
- `strftime()`: aplica el formato de presentación deseado.
    
- Usa `datetime.fromtimestamp(timestamp, tz=timezone.utc)` si necesitas una fecha explícita en UTC.
    

**Referencia:** Python 3.14 · `datetime` y `pathlib`.