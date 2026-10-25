# Sintaxis de patrones Regex en Python (`re`)

Combina caracteres literales y metacaracteres para definir qué texto buscar, validar o extraer.

## Metacaracteres esenciales

|Patrón|Función|Ejemplo|
|---|---|---|
|`.`|Cualquier carácter, excepto salto de línea por defecto|`a.c`|
|`^`|Inicio de cadena o línea|`^Hola`|
|`$`|Final de cadena o línea|`fin$`|
|`*`|Cero o más repeticiones|`ab*`|
|`+`|Una o más repeticiones|`ab+`|
|`?`|Cero o una repetición|`colou?r`|
|`{n,m}`|Entre `n` y `m` repeticiones|`\d{2,4}`|
|`[]`|Conjunto de caracteres permitidos|`[A-Z]`|
|`()`|Agrupa una expresión y captura su contenido|`(ab)+`|
|`\|`|Alternativa entre patrones|`gato\|perro`|
|`\`|Escapa metacaracteres o introduce secuencias especiales|`\.`|

## Secuencias frecuentes

- `\d`: dígito decimal.
    
- `\w`: carácter alfanumérico Unicode o guion bajo.
    
- `\s`: espacio en blanco, incluidos tabuladores y saltos de línea.
    
- `\D`, `\W`, `\S`: complementos de las secuencias anteriores.
    

## Ejemplo funcional

Usa cadenas raw (`r"..."`) para evitar conflictos entre las barras invertidas de Python y las de Regex.

```python
import re

texto = "Pedido: 12345, Pedido: 67890"
patron = r"\d{5}"

pedidos = re.findall(patron, texto)
print(pedidos)  # ['12345', '67890']
```

**Regla:** combina literales, cuantificadores y clases de caracteres. Usa `re.fullmatch()` para validar que toda la cadena cumpla el patrón y `re.search()` para localizar coincidencias parciales.

**Referencia:** [Documentación oficial de](https://docs.python.org/3.14/library/re.html) `[re](https://docs.python.org/3.14/library/re.html)` [— Python 3.14](https://docs.python.org/3.14/library/re.html).