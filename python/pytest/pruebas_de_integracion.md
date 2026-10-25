# Pruebas de integración con Pytest

Verifica que varios componentes colaboren correctamente, como servicios, repositorios y bases de datos. A diferencia de las pruebas unitarias, utiliza las dependencias reales que deseas comprobar, aisladas del entorno de producción.

## Ejemplo: servicio y base de datos SQLite

```python
from sqlmodel import SQLModel, Session, create_engine

def test_guardar_pedido():
    engine = create_engine("sqlite://")
    SQLModel.metadata.create_all(engine)

    try:
        with Session(engine) as session:
            pedido = Pedido(numero_pedido="123")
            session.add(pedido)
            session.commit()

            guardado = session.get(Pedido, pedido.id)
            assert guardado.numero_pedido == "123"
    finally:
        engine.dispose()
```

Adapta `Pedido` y sus campos al modelo real del proyecto. Para pruebas más complejas, centraliza la creación de la base de datos y las sesiones en fixtures.

## Qué verificar

- **Servicio + repositorio:** las reglas de negocio se aplican al guardar y consultar datos.
    
- **Repositorio + base de datos:** las operaciones persisten y recuperan registros correctamente.
    
- **API + base de datos:** los endpoints producen las respuestas esperadas y conservan los datos.
    
- **Transacciones:** los cambios de prueba no contaminan otras pruebas.
    

## Reglas

- Utiliza una base de datos exclusiva para pruebas.
    
- Evita mocks en los componentes cuya integración estás verificando.
    
- Usa fixtures para crear y limpiar recursos.
    
- Comprueba resultados observables, no detalles internos.
    
- Mantén las pruebas unitarias separadas de las de integración mediante `@pytest.mark.integration`.
    

**Prioriza:** probar el recorrido completo de una operación importante —por ejemplo, crear un pedido, guardarlo y recuperarlo— con dependencias reales y datos aislados.