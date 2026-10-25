# SQLite3 — Operadores de comparación

Filtra registros mediante comparaciones entre valores.

|Operador|Función|
|---|---|
|`=`|Igual|
|`!=`|Distinto|
|`>`|Mayor que|
|`<`|Menor que|
|`>=`|Mayor o igual|
|`<=`|Menor o igual|

Ejemplo funcional:

```sql
SELECT numero_pedido, cantidad_bultos
FROM pedidos
WHERE cantidad_bultos >= 10;
```

Devuelve pedidos con 10 o más bultos.

Combina condiciones:

```sql
SELECT *
FROM pedidos
WHERE cantidad_bultos >= 10
  AND cantidad_bultos <= 20;
```

Reglas:

- Usa comillas simples para comparar texto: `zona = 'Norte'`.
    
- No uses `= NULL`; utiliza `IS NULL` o `IS NOT NULL`.
    
- Considera que las comparaciones con `NULL` no devuelven verdadero ni falso, sino un resultado desconocido.