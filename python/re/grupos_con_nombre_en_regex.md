# Grupos con nombre en Regex — `(?P<nombre>...)`

Asigna nombres descriptivos a las capturas para acceder a cada campo sin depender de su posición.

```python
import re

patron = re.compile(
    r"(?P<codigo>[A-Z]+)-(?P<numero>\d+)"
)

coincidencia = patron.search("Pedido ABC-12345")

if coincidencia:
    print(coincidencia.group("codigo"))  # ABC
    print(coincidencia.group("numero"))  # 12345
    print(coincidencia.groupdict())
    # {'codigo': 'ABC', 'numero': '12345'}
```

**Usa estos métodos:**

- `group("nombre")`: recupera un campo específico.
    
- `groupdict()`: devuelve todas las capturas con nombre en un diccionario.
    
- `(?P=nombre)`: vuelve a coincidir con el texto capturado por ese grupo.
    

**Regla:** asigna nombres descriptivos al extraer datos estructurados, como códigos, fechas, identificadores o campos de registros.

**Referencia:** [Documentación oficial de Regex — Python 3.14](https://docs.python.org/3.14/library/re.html#regular-expression-syntax).