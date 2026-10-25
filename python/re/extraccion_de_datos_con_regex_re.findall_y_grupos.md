# Extracción de datos con Regex — `re.findall()` y grupos

Extrae números, códigos, fechas y campos de textos semiestructurados mediante patrones y grupos de captura.

## Ejemplo funcional

```python
import re

texto = "Pedido ABC-12345 registrado el 24/10/2026"

patron = re.compile(
    r"(?P<codigo>[A-Z]{3}-\d{5}).*?"
    r"(?P<fecha>\d{2}/\d{2}/\d{4})"
)

coincidencia = patron.search(texto)

if coincidencia:
    print(coincidencia.groupdict())
    # {'codigo': 'ABC-12345', 'fecha': '24/10/2026'}
```

## Técnicas esenciales

- Usa `re.search()` para extraer el primer registro coincidente.
    
- Usa `re.findall()` para obtener todas las coincidencias.
    
- Usa `re.finditer()` para recuperar datos y posiciones.
    
- Usa grupos con nombre `(?P<nombre>...)` para identificar campos.
    
- Valida las fechas con `datetime.strptime()` después de extraerlas; una expresión regular por sí sola no garantiza que una fecha exista.
    

**Regla:** utiliza Regex para identificar y extraer estructuras textuales; valida después los tipos, rangos y reglas de negocio con herramientas específicas.

**Referencia:** [Documentación oficial de](https://docs.python.org/3.14/library/re.html) `[re](https://docs.python.org/3.14/library/re.html)` [— Python 3.14](https://docs.python.org/3.14/library/re.html).