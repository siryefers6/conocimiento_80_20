# Pruebas unitarias con Pytest

Comprueba funciones y servicios de forma aislada, verificando sus resultados y reglas de negocio sin depender de bases de datos reales, redes u otros sistemas externos.

## Ejemplo: probar una función

```python
def calcular_total(precio: float, cantidad: int) -> float:
    if precio < 0 or cantidad < 0:
        raise ValueError("Valores negativos")
    return precio * cantidad
```

```python
import pytest
from app.services.pedido import calcular_total

def test_calcular_total():
    assert calcular_total(100, 3) == 300

def test_rechazar_precio_negativo():
    with pytest.raises(ValueError, match="Valores negativos"):
        calcular_total(-100, 2)
```

## Principios esenciales

- **Aislamiento:** evita conexiones reales a servicios externos.
    
- **Determinismo:** usa entradas controladas y resultados predecibles.
    
- **Rapidez:** ejecuta pruebas sin inicializar infraestructura innecesaria.
    
- **Cobertura de reglas:** comprueba casos normales, límites y entradas inválidas.
    
- **Independencia:** cada prueba debe poder ejecutarse por separado.
    

## Dobles de prueba

Usa `Mock` o `monkeypatch` para sustituir dependencias externas cuando sea necesario. Comprueba el comportamiento del servicio, no los detalles internos irrelevantes.

**Prioriza:** probar las reglas de negocio en funciones y servicios puros. Reserva las pruebas de integración para verificar la interacción real con la base de datos, la API u otros componentes.