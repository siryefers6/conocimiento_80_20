# Coroutines — Diferenciar la función asíncrona del objeto corutina

Una función declarada con `async def` es una **función asíncrona**. Al llamarla, obtienes un **objeto corutina**, que debes esperar con `await` o ejecutar mediante `asyncio`.

```python
import asyncio

async def obtener_dato():  # Función asíncrona
    return 42

corutina = obtener_dato()  # Objeto corutina

async def main():
    resultado = await corutina
    print(resultado)  # 42

asyncio.run(main())
```

- **Función asíncrona:** define el comportamiento.
    
- **Objeto corutina:** representa la ejecución pendiente.
    
- `**await**`**:** espera el resultado de la corutina.
    

**Clave:** definir una función asíncrona no ejecuta su cuerpo; llamarla crea un objeto corutina.