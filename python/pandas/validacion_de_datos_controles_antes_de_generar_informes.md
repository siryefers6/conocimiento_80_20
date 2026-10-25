# Validación de datos — controles antes de generar informes

**Pandas 3.1.0**

Valida nulos, duplicados, tipos, rangos y claves antes de calcular métricas o generar informes. Detecta errores al inicio del procesamiento.

## 1. Comprobar valores nulos

```python
nulos = df.isna().sum()

print(nulos[nulos > 0])
```

Si una columna obligatoria contiene nulos, detén el proceso o gestiona los registros afectados explícitamente.

## 2. Detectar duplicados

```python
duplicados = df[df.duplicated(subset=["pedido_id"], keep=False)]

print(duplicados)
```

Usa `subset` para validar la clave que debe ser única. No elimines duplicados automáticamente sin verificar si son errores reales.

## 3. Validar tipos de datos

```python
print(df.dtypes)

df["peso"] = pd.to_numeric(df["peso"], errors="coerce")
```

`errors="coerce"` convierte valores inválidos en `NaN`. Cuenta los nuevos nulos para identificar errores de conversión.

## 4. Validar rangos

```python
invalidos = df.loc[
    (df["peso"] < 0) | (df["peso"] > 1000)
]

if not invalidos.empty:
    raise ValueError("Hay pesos fuera del rango permitido")
```

Define los límites según las reglas reales del negocio.

## 5. Validar claves obligatorias

```python
if df["pedido_id"].isna().any():
    raise ValueError("Hay pedidos sin identificador")

if df["pedido_id"].duplicated().any():
    raise ValueError("Hay identificadores duplicados")
```

Si la clave puede repetirse legítimamente, valida la combinación de columnas que la identifica.

## 6. Crear una validación resumida

```python
resumen = {
    "filas": len(df),
    "nulos": int(df.isna().sum().sum()),
    "filas_duplicadas": int(df.duplicated().sum()),
}

print(resumen)
```

## Reglas esenciales

- Valida antes de agrupar, combinar o calcular métricas.
    
- Distingue errores críticos de advertencias.
    
- No confundas filas idénticas con claves duplicadas.
    
- Registra los registros rechazados y el motivo.
    
- No generes informes si fallan las validaciones críticas.
    

**Prioriza:** claves válidas, tipos correctos, rangos permitidos y ausencia de nulos en campos obligatorios.