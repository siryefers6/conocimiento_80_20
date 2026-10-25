# SQLite3 — Consultas parametrizadas con Python

Usa parámetros para insertar valores en las consultas sin concatenar cadenas. Evita la inyección SQL y separa los datos del código SQL.

Consultar con parámetros:

```python
import sqlite3

with sqlite3.connect("app.db") as conn:
    pedidos = conn.execute(
        """
        SELECT *
        FROM pedidos
        WHERE numero_pedido = ?
        """,
        (1001,),
    ).fetchall()
```

- `?`: marcador de posición del parámetro.
    
- `(1001,)`: tupla con un único valor. La coma es obligatoria.
    
- `fetchall()`: recupera todas las filas del resultado.
    
- `with`: administra el cierre de la conexión y confirma o revierte la transacción según corresponda.
    

Usar varios parámetros:

```python
conn.execute(
    """
    SELECT *
    FROM pedidos
    WHERE zona = ? AND cantidad_bultos > ?
    """,
    ("Norte", 10),
)
```

Insertar datos de forma segura:

```python
conn.execute(
    """
    INSERT INTO pedidos (numero_pedido, zona)
    VALUES (?, ?)
    """,
    (1002, "Sur"),
)
```

Regla: utiliza siempre parámetros para los valores. No uses f-strings ni concatenación para construir consultas con datos externos.

Importante: los parámetros representan valores, no nombres de tablas ni columnas. Para estos últimos, utiliza una lista explícita de opciones permitidas.