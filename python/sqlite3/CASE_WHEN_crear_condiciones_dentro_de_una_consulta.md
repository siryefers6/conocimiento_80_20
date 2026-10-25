# SQLite3 — CASE WHEN: crear condiciones dentro de una consulta

Evalúa condiciones y devuelve un valor según el primer caso que se cumpla.

Clasificar registros:

```sql
SELECT
    numero_pedido,
    cantidad_bultos,
    CASE
        WHEN cantidad_bultos = 0 THEN 'Vacío'
        WHEN cantidad_bultos <= 10 THEN 'Pequeño'
        ELSE 'Grande'
    END AS categoria
FROM pedidos;
```

- `CASE`: inicia la evaluación condicional.
    
- `WHEN`: define una condición.
    
- `THEN`: establece el resultado si se cumple.
    
- `ELSE`: define el resultado predeterminado.
    
- `END`: finaliza la expresión.
    

Contar registros por categoría calculada:

```sql
SELECT
    CASE
        WHEN cantidad_bultos <= 10 THEN 'Pequeño'
        ELSE 'Grande'
    END AS categoria,
    COUNT(*) AS total
FROM pedidos
GROUP BY categoria;
```

Regla: utiliza `CASE WHEN` para clasificar, etiquetar o calcular valores condicionales sin modificar los registros almacenados. Evalúa los casos en orden; el primer caso verdadero determina el resultado.