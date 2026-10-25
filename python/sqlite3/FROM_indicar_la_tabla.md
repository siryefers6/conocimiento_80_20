# SQLite3 — FROM: indicar la tabla

Especifica la tabla desde la que obtendrás los registros.

```sql
SELECT numero_pedido, zona
FROM pedidos;
```

- `FROM`: indica el origen de los datos.
    
- `pedidos`: nombre de la tabla consultada.
    

Consultar otra tabla:

```sql
SELECT codigo, peso
FROM embalajes;
```

Regla: identifica primero la tabla que contiene los datos necesarios.