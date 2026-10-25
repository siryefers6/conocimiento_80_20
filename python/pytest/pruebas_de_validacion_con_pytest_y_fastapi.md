# Pruebas de validación con Pytest y FastAPI

Comprueba que la API rechace datos inválidos, exija los campos obligatorios y devuelva los códigos HTTP y mensajes de error esperados.

## 1. Validar campos obligatorios

```python
def test_crear_pedido_sin_datos(client):
    response = client.post("/api/pedidos", json={})

    assert response.status_code == 422
```

FastAPI utiliza Pydantic para validar automáticamente los datos de entrada.

## 2. Validar tipos y restricciones

```python
def test_cantidad_negativa(client):
    response = client.post(
        "/api/pedidos",
        json={"numero_pedido": "123", "cantidad_bultos": -1},
    )

    assert response.status_code == 422
```

Este resultado presupone que el esquema de entrada prohíbe cantidades negativas.

## 3. Comprobar errores de negocio

```python
def test_pedido_inexistente(client):
    response = client.get("/api/pedidos/999999")

    assert response.status_code == 404
    assert response.json()["detail"] == "Pedido no encontrado"
```

## 4. Casos que debes cubrir

- **Campos obligatorios:** ausentes o nulos.
    
- **Tipos incorrectos:** texto donde se espera un entero.
    
- **Restricciones:** valores negativos, longitudes o rangos inválidos.
    
- **Formato:** identificadores, fechas y códigos mal formados.
    
- **Reglas de negocio:** duplicados, recursos inexistentes o estados incompatibles.
    
- **Errores HTTP:** comprobar código y estructura del cuerpo de respuesta.
    

## Reglas

- Utiliza `pytest.mark.parametrize` para probar múltiples entradas inválidas.
    
- Comprueba tanto el rechazo como la respuesta esperada.
    
- Distingue los errores de validación (`422`) de los errores de negocio (`400`, `404` o `409`, según el contrato).
    
- No asumas que un campo es inválido sin verificar las restricciones definidas en el esquema.
    

**Prioriza:** campos obligatorios, tipos incorrectos, límites y reglas de negocio. Cada prueba debe verificar el código HTTP y, cuando corresponda, los detalles del error.