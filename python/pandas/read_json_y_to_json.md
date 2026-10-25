# JSON — `read_json()` y `to_json()` en Pandas

Importa datos JSON con `pd.read_json()` y exporta estructuras tabulares con `DataFrame.to_json()`. Utiliza JSON para intercambiar datos entre aplicaciones, APIs y servicios web.

## 1. Leer un archivo JSON

```python
import pandas as pd

df = pd.read_json("datos.json")
```

## 2. Controlar la lectura

```python
df = pd.read_json(
    "datos.json",
    orient="records",
)
```

- `orient="records"`: cada objeto representa una fila.
    
- `orient="split"`: separa índices, columnas y datos.
    
- `lines=True`: lee un objeto JSON por línea (JSON Lines).
    
- `convert_dates=True`: convierte automáticamente determinadas columnas de fechas.
    

Ejemplo de JSON compatible con `orient="records"`:

```python
[
    {"nombre": "Ana", "edad": 25},
    {"nombre": "Luis", "edad": 30}
]
```

## 3. Exportar datos JSON

```python
df.to_json(
    "resultado.json",
    orient="records",
    indent=2,
    force_ascii=False,
)
```

- `orient="records"`: genera una lista de objetos.
    
- `indent=2`: indenta el documento para facilitar su lectura.
    
- `force_ascii=False`: conserva caracteres como las tildes.
    
- `date_format="iso"`: representa fechas en formato ISO 8601.
    

## 4. Intercambiar datos con una API

Convierte un `DataFrame` en registros Python para integrarlos con aplicaciones FastAPI.

```python
registros = df.to_dict(orient="records")
```

Para obtener una cadena JSON directamente:

```python
json_texto = df.to_json(
    orient="records",
    force_ascii=False,
)
```

## Regla 80/20

Domina `read_json()`, `to_json()`, `orient="records"`, `lines=True` y `to_dict(orient="records")`. Verifica la estructura JSON esperada antes de importar o exportar datos.

**Versión de referencia:** Pandas 3.1.0.