# SQLModel: insertar, eliminar y buscar registros

**Referencia:** SQLModel 0.0.48 · 24-10-2026

## 1. Insertar con `add()`

Añade un objeto a la sesión y confirma la transacción para persistirlo.

```python
with Session(engine) as session:
    producto = Producto(nombre="Teclado", precio=25000)
    session.add(producto)
    session.commit()
    session.refresh(producto)
```

## 2. Recuperar por clave primaria con `get()`

Obtén un registro mediante su clave primaria. Devuelve `None` si no existe.

```python
with Session(engine) as session:
    producto = session.get(Producto, 1)
```

- Primer argumento: clase del modelo.
    
- Segundo argumento: valor de la clave primaria.
    

## 3. Eliminar con `delete()`

Marca un objeto persistido para su eliminación y confirma la transacción.

```python
with Session(engine) as session:
    producto = session.get(Producto, 1)

    if producto is not None:
        session.delete(producto)
        session.commit()
```

## Reglas prácticas

- Ejecuta `commit()` para confirmar inserciones y eliminaciones.
    
- Comprueba si `get()` devuelve `None` antes de utilizar el objeto.
    
- Usa `select()` y `session.exec()` para buscar por campos distintos de la clave primaria.
    
- Gestiona los errores de integridad, como las restricciones de claves foráneas.
    

**Prioriza:** domina estas tres operaciones antes de implementar endpoints CRUD en FastAPI.