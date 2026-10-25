# SQLite3 — INSERT INTO: insertar registros

Inserta nuevas filas en una tabla indicando las columnas y sus valores.

Insertar un registro:

```sql
INSERT INTO pedidos (numero_pedido, cantidad_bultos, zona)
VALUES (1001, 12, 'Norte');
```

Insertar varios registros:

```sql
INSERT INTO pedidos (numero_pedido, cantidad_bultos, zona)
VALUES
    (1002, 8, 'Sur'),
    (1003, 15, 'Centro');
```

Insertar resultados de una consulta:

```sql
INSERT INTO pedidos_archivo (numero_pedido, zona)
SELECT numero_pedido, zona
FROM pedidos
WHERE cantidad_bultos = 0;
```

- `INSERT INTO`: indica la tabla de destino.
    
- Las columnas determinan el orden de los valores.
    
- `VALUES`: define los datos que insertarás.
    

Regla: especifica siempre las columnas. Respeta los tipos de datos, las restricciones y las columnas obligatorias.