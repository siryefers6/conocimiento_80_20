# SQLite3 — WHERE: filtrar registros

Filtra las filas según una condición.

```sql
SELECT numero_pedido, zona
FROM pedidos
WHERE zona = 'Norte';
```

- `WHERE`: define la condición de filtrado.
    
- `=`: compara valores.
    
- `'Norte'`: texto entre comillas simples.
    

Operadores esenciales:

| Operador    | Función                       |
| ----------- | ----------------------------- |
| `=`         | Igual                         |
| `!=` o `<>` | Distinto                      |
| `>` / `<`   | Mayor / menor                 |
| `>=` / `<=` | Mayor o igual / menor o igual |
| `AND`       | Ambas condiciones             |
| `OR`        | Al menos una condición        |
| `IN`        | Coincide con una lista        |
| `LIKE`      | Busca patrones de texto       |

Combina condiciones:

```sql
SELECT *
FROM pedidos
WHERE zona = 'Norte'
  AND cantidad_bultos > 10;
```

Regla: utiliza `WHERE` para recuperar únicamente los registros que cumplan las condiciones requeridas.