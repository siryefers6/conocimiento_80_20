# Pruebas de regresión — pytest

**Objetivo:** conserva pruebas que detecten la reaparición de errores corregidos después de modificar el código.

## Flujo de trabajo

1. **Reproduce:** identifica el error con un caso de prueba.
    
2. **Verifica:** confirma que la prueba falla antes de corregirlo.
    
3. **Corrige:** modifica el código de producción.
    
4. **Conserva:** incorpora la prueba al conjunto permanente.
    
5. **Ejecuta:** comprueba que la prueba y el resto de la suite pasan.
    

## Ejemplo

```python
# app/descuentos.py
def aplicar_descuento(precio: float, porcentaje: float) -> float:
    if not 0 <= porcentaje <= 100:
        raise ValueError("Porcentaje inválido")
    return precio * (1 - porcentaje / 100)
```

```python
# tests/test_descuentos.py
import pytest
from app.descuentos import aplicar_descuento


def test_descuento_del_cien_por_ciento():
    assert aplicar_descuento(100, 100) == 0


def test_rechaza_descuento_superior_al_cien():
    with pytest.raises(ValueError, match="Porcentaje inválido"):
        aplicar_descuento(100, 101)
```

## Ejecución

```sh
# Ejecutar la prueba relacionada
uv run pytest tests/test_descuentos.py

# Ejecutar toda la suite de regresión
uv run pytest
```

## Reglas esenciales

- **Añade una prueba por cada error corregido** cuando sea posible reproducirlo.
    
- **Comprueba el comportamiento observable**, no detalles internos innecesarios.
    
- **Incluye casos límite** relacionados con el error original.
    
- **Ejecuta toda la suite** antes de integrar cambios.
    

**Referencia:** [pytest — Good Practices](https://docs.pytest.org/en/stable/explanation/goodpractices.html)