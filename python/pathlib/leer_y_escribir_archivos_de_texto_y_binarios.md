# `pathlib` — Leer y escribir archivos de texto y binarios

Utiliza `read_text()` y `write_text()` para archivos de texto; utiliza `read_bytes()` y `write_bytes()` para contenido binario. Evita gestionar manualmente objetos de archivo cuando estas operaciones sean suficientes.

```python
from pathlib import Path

# Texto
archivo = Path("datos.txt")
archivo.write_text("Hola, Python", encoding="utf-8")
contenido = archivo.read_text(encoding="utf-8")

# Binario
binario = Path("imagen.bin")
binario.write_bytes(b"\x00\x01\x02")
datos = binario.read_bytes()
```

- `read_text(encoding="utf-8")`: lee el contenido como `str`.
    
- `write_text(texto, encoding="utf-8")`: escribe texto y devuelve el número de caracteres escritos.
    
- `read_bytes()`: lee el contenido como `bytes`.
    
- `write_bytes(datos)`: escribe bytes y devuelve el número de bytes escritos.
    
- **Crea primero los directorios padre** si no existen.
    
- **Evita sobrescribir datos accidentalmente**: los métodos de escritura reemplazan el contenido existente y crean el archivo si no existe.
    

**Referencia:** Python 3.14 · Biblioteca estándar `pathlib`.