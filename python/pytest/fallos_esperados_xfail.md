# Fallos esperados en Pytest (`xfail`)

Marca pruebas que se espera que fallen debido a defectos conocidos o funcionalidades pendientes, sin confundir esos fallos con regresiones inesperadas.

## 1. `pytest.mark.xfail`

```python
import pytest

@pytest.mark.xfail(reason="Pendiente corregir el cálculo")
def test_calcular_total():
    assert calcular_total() == 100
```

Si la prueba falla, Pytest la registra como `XFAIL` en lugar de considerarla un fallo normal.

## 2. `strict=True`

Convierte una prueba que pasa inesperadamente en un fallo (`XPASS`).

```python
@pytest.mark.xfail(
    reason="Error conocido #123",
    strict=True,
)
def test_calcular_total():
    assert calcular_total() == 100
```

Usa `strict=True` para detectar cuándo se corrige el defecto y retirar el marcador.

## 3. `pytest.xfail()`

Marca dinámicamente una prueba como fallo esperado.

```python
def test_funcionalidad():
    if not funcionalidad_implementada():
        pytest.xfail("Funcionalidad pendiente")

    assert ejecutar_funcionalidad() == "OK"
```

## Reglas

- Documenta el motivo y, cuando sea posible, la incidencia asociada.
    
- Usa `xfail` temporalmente; elimina el marcador cuando corrijas el defecto.
    
- Prefiere `strict=True` para detectar pruebas que pasan inesperadamente.
    
- Usa `skip` cuando no puedas ejecutar una prueba; usa `xfail` cuando puedas ejecutarla, pero esperes que falle.
    

**Prioriza:** `@pytest.mark.xfail(reason="...", strict=True)` para defectos conocidos que deben seguir bajo vigilancia.