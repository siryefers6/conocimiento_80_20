# Directorios temporales (`tmp_path`) — pytest

**Objetivo:** crea archivos y directorios temporales aislados para cada prueba, sin modificar archivos reales del proyecto.

## Uso básico

```python
def test_crear_archivo(tmp_path):
    archivo = tmp_path / "datos.txt"
    archivo.write_text("contenido", encoding="utf-8")

    assert archivo.exists()
    assert archivo.read_text(encoding="utf-8") == "contenido"
```

- `tmp_path`: fixture integrada de pytest que proporciona un objeto `pathlib.Path`.
    
- `/`: construye rutas de forma portable.
    
- `write_text()` y `read_text()`: escriben y leen archivos de texto.
    

## Crear estructuras de directorios

```python
def test_crear_directorio(tmp_path):
    carpeta = tmp_path / "informes"
    carpeta.mkdir()

    archivo = carpeta / "reporte.csv"
    archivo.write_text("id,nombre\n1,Ana\n", encoding="utf-8")

    assert archivo.is_file()
    assert archivo.parent == carpeta
```

## Reglas esenciales

- **Usa** `**tmp_path**` para probar archivos, exportaciones, configuraciones y procesamiento de datos.
    
- **Crea las rutas dentro de** `**tmp_path**` para mantener el aislamiento.
    
- **Evita rutas absolutas fijas** y archivos compartidos entre pruebas.
    
- **Prefiere** `**tmp_path**` **sobre** `**tmpdir**` en código nuevo: devuelve un `Path` moderno de Python.
    
- **No elimines manualmente el directorio temporal**; pytest administra su ciclo de vida.
    

**Referencia:** [pytest — The](https://docs.pytest.org/en/stable/how-to/tmp_path.html) `[tmp_path](https://docs.pytest.org/en/stable/how-to/tmp_path.html)` [fixture](https://docs.pytest.org/en/stable/how-to/tmp_path.html)