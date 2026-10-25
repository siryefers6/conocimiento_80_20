# Mover archivos y directorios con `shutil.move()`

Usa `shutil.move(origen, destino)` para mover archivos o directorios. Si el movimiento entre sistemas de archivos no es posible directamente, puede copiar el contenido y eliminar el origen.

```python
from shutil import move

move("datos.txt", "respaldo/datos.txt")
move("documentos", "respaldo/documentos")
```

- Acepta archivos y directorios.
    
- Si el destino es un directorio existente, mueve el elemento dentro de él.
    
- Entre sistemas de archivos, puede recurrir a copiar y eliminar.
    
- Comprueba el destino antes de mover: el comportamiento depende de si existe y del tipo de elemento.
    
- Puede generar `OSError` si la operación falla.