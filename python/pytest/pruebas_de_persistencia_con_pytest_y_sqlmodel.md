# Pruebas de persistencia con Pytest y SQLModel

Verifica que la base de datos guarde, actualice, consulte y rechace registros correctamente. Utiliza una base de prueba aislada y ejecuta operaciones SQL reales.

## 1. Inserción y consulta

```python
def test_insertar_y_consultar(session):
    pedido = Pedido(numero_pedido="123")
    session.add(pedido)
    session.commit()
    session.refresh(pedido)

    guardado = session.get(Pedido, pedido.id)

    assert guardado is not None
    assert guardado.numero_pedido == "123"
```

La fixture `session` debe proporcionar una sesión conectada a la base de datos de prueba.

## 2. Actualización

```python
def test_actualizar_pedido(session, pedido):
    pedido.numero_pedido = "456"
    session.add(pedido)
    session.commit()

    session.refresh(pedido)

    assert pedido.numero_pedido == "456"
```

## 3. Restricciones de integridad

Comprueba que la base de datos rechace registros que incumplan restricciones como `UNIQUE`, `NOT NULL` o `CHECK`.

```python
import pytest
from sqlalchemy.exc import IntegrityError

def test_rechazar_duplicado(session, embalaje):
    duplicado = Embalaje(codigo_barras=embalaje.codigo_barras)
    session.add(duplicado)

    with pytest.raises(IntegrityError):
        session.commit()

    session.rollback()
```

Este ejemplo presupone que `codigo_barras` tiene una restricción `UNIQUE` real en la base de datos.

## 4. Consultas y filtros

Verifica que las consultas devuelvan los registros correctos, respeten filtros y no incluyan datos ajenos a la prueba.

```python
def test_filtrar_pedidos(session):
    session.add_all([
        Pedido(numero_pedido="100"),
        Pedido(numero_pedido="200"),
    ])
    session.commit()

    resultados = session.query(Pedido).filter(
        Pedido.numero_pedido == "100"
    ).all()

    assert len(resultados) == 1
    assert resultados[0].numero_pedido == "100"
```

## Reglas

- Usa una base de datos exclusiva para pruebas.
    
- Verifica las restricciones tanto en el modelo como en la base de datos.
    
- Ejecuta `rollback()` después de un fallo de integridad para restaurar la sesión.
    
- Comprueba el estado persistido, no solamente los atributos del objeto en memoria.
    
- Aísla cada prueba mediante fixtures y transacciones cuando corresponda.
    

**Prioriza:** inserción y lectura, actualización, restricciones de integridad y consultas filtradas. Usa SQLite para pruebas rápidas, pero prueba también contra PostgreSQL si será la base de datos de producción.