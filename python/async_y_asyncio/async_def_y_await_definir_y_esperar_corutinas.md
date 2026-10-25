# `async def` y `await` — Definir y esperar corutinas en Python

## Declarar una corutina

Usa `async def` para definir una función asíncrona. Al llamarla, obtén un objeto corutina; no ejecutes su cuerpo inmediatamente.

```python
async def obtener_dato():
    return 42
```

## Esperar una corutina

Usa `await` dentro de una función asíncrona para esperar el resultado de otra operación asíncrona.

```python
async def main():
    resultado = await obtener_dato()
    print(resultado)  # 42
```

Ejecuta la corutina principal con `asyncio.run()`:

```python
import asyncio

async def main():
    resultado = await obtener_dato()
    print(resultado)

asyncio.run(main())
```

## Reglas esenciales

- `**async def**`**:** define una función que produce una corutina.
    
- `**await**`**:** espera a que una operación asíncrona termine y devuelve su resultado.
    
- `**asyncio.run()**`**:** inicia y gestiona el bucle de eventos para ejecutar la corutina principal.
    
- **Concurrencia:** permite intercalar operaciones cuando esperan E/S; no implica paralelismo de CPU.
    
- **Restricción:** no uses `await` directamente en una función convencional con `def`.
    

## Regla 80/20

Aprende a declarar corutinas con `async def`, esperar resultados con `await` y ejecutar el punto de entrada con `asyncio.run()`.