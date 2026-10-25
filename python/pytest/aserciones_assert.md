# Aserciones (`assert`) — pytest

**Objetivo:** verifica que los resultados reales coincidan con los esperados mediante `assert`.

## Uso básico

```python
def sumar(a: int, b: int) -> int:
    return a + b


def test_sumar():
    assert sumar(2, 3) == 5
    assert sumar(0, 0) == 0
    assert sumar(-1, 1) == 0
```

## Comparaciones habituales

```python
assert resultado == esperado
assert resultado != valor
assert resultado is None
assert elemento in coleccion
assert condicion is True
assert len(elementos) == 3
```

## Mensajes de error detallados

```python
def test_total():
    total = 10
    assert total == 15
```

Si la prueba falla, `pytest` muestra los valores comparados y la expresión que no se cumplió.

Añade un mensaje solo cuando aporte contexto adicional:

```python
assert total == 15, "El total debe ser 15"
```

## Reglas esenciales

- **Usa** `**assert**` **directamente** para comprobar condiciones.
    
- **Escribe una condición clara por aserción** cuando mejore la legibilidad.
    
- **Evita** `**assert condicion, mensaje**` **innecesarios**: pytest ya genera información detallada.
    
- **No uses** `**assert**` **para validar datos en producción**; Python puede desactivarlo con `-O`.
    

**Referencia:** [pytest — Assertions](https://docs.pytest.org/en/stable/how-to/assert.html)