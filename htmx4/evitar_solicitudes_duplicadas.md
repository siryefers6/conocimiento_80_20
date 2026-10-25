# HTMX 4: evitar solicitudes duplicadas

Impide que el usuario envíe varias veces la misma operación. Combina controles de interfaz con validaciones en FastAPI.

## 1. Deshabilitar el botón durante la petición

```html
<form hx-post="/pedidos"
      hx-target="#resultado">
    <input name="numero" required>

    <button type="submit" hx-disable>
        Registrar pedido
    </button>
</form>

<div id="resultado"></div>
```

`hx-disable` deshabilita temporalmente el botón durante la petición y lo habilita al finalizar.

## 2. Evitar duplicados en el servidor

La interfaz no garantiza que una operación se ejecute una sola vez. Para impedir registros duplicados, aplica restricciones en la base de datos.

```python
class Pedido(SQLModel, table=True):
    id: int | None = Field(default=None, primary_key=True)
    numero: str = Field(unique=True, index=True)
```

La restricción `unique=True` impide almacenar dos pedidos con el mismo número, incluso si llegan solicitudes simultáneas. Gestiona la excepción de integridad y devuelve un error HTTP apropiado.

## Reglas esenciales

- Usa `hx-disable` para evitar clics repetidos.
    
- Usa restricciones únicas para impedir registros duplicados.
    
- Para operaciones que puedan reintentarse, considera una clave de idempotencia.
    
- No confíes únicamente en el navegador: las peticiones pueden repetirse por red, recargas o clientes externos.
    

**Principio:** protege la interfaz y garantiza la integridad de los datos en el servidor.