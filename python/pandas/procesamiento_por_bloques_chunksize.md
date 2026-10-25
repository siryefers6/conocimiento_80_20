# Procesamiento por bloques — `chunksize`

**Pandas 3.1.0**

Usa `chunksize` para leer archivos grandes en bloques y procesarlos sin cargar todo el contenido en memoria.

## 1. Leer un CSV por bloques

```python
import pandas as pd

for bloque in pd.read_csv("pedidos.csv", chunksize=10_000):
    print(bloque.shape)
```

Cada iteración devuelve un `DataFrame` con hasta 10.000 filas.

## 2. Procesar y acumular resultados

Ejemplo: sumar el importe de todos los pedidos sin cargar el archivo completo.

```python
total = 0

for bloque in pd.read_csv("pedidos.csv", chunksize=10_000):
    total += bloque["importe"].sum()

print(total)
```

## 3. Filtrar y guardar resultados

```python
primera_escritura = True

for bloque in pd.read_csv("pedidos.csv", chunksize=10_000):
    filtrado = bloque[bloque["estado"] == "Pendiente"]

    if not filtrado.empty:
        filtrado.to_csv(
            "pendientes.csv",
            mode="w" if primera_escritura else "a",
            header=primera_escritura,
            index=False,
        )
        primera_escritura = False
```

El modo `w` crea o sobrescribe el archivo; `a` añade filas. `header` evita repetir los encabezados.

## Reglas esenciales

- `chunksize` devuelve un iterador de bloques, no un único `DataFrame`.
    
- Ajusta el tamaño del bloque según la memoria disponible.
    
- Acumula únicamente los resultados necesarios; evita guardar todos los bloques en una lista.
    
- Las operaciones entre bloques, como eliminar duplicados globales o calcular una mediana exacta, requieren tratamiento adicional.
    
- `read_csv()` admite `chunksize`; otros formatos tienen distintas limitaciones.
    

**Prioriza:** leer, filtrar, agregar y guardar cada bloque antes de procesar el siguiente.