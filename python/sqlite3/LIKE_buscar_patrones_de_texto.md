# SQLite3 — LIKE: buscar patrones de texto

Filtra texto según un patrón utilizando comodines.

- `%`: representa cero o más caracteres.
    
- `_`: representa exactamente un carácter.
    

Buscar texto al inicio:

```sql
SELECT *
FROM pedidos
WHERE zona LIKE 'Nor%';
```

Encuentra valores como `Norte` y `Norte Chico`.

Buscar texto en cualquier posición:

```sql
SELECT *
FROM pedidos
WHERE zona LIKE '%sur%';
```

Buscar un carácter específico:

```sql
SELECT *
FROM pedidos
WHERE zona LIKE '_ur';
```

Encuentra valores como `Sur` si coinciden con el patrón.

Reglas:

- Usa `%texto%` para buscar coincidencias parciales.
    
- En SQLite, `LIKE` normalmente ignora diferencias entre mayúsculas y minúsculas en caracteres ASCII; para Unicode, el comportamiento predeterminado es limitado.
    
- Los patrones que comienzan con `%` normalmente impiden aprovechar un índice para acelerar la búsqueda.