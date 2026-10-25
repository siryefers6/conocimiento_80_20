# Límites de recursos en WebSocket con FastAPI

Establece límites de conexiones simultáneas, tamaño de mensajes, frecuencia de envío y memoria para evitar sobrecargas y ataques de denegación de servicio.

## 1. Limita conexiones simultáneas

Controla el número de conexiones activas por proceso y por usuario. Para varios trabajadores, aplica límites compartidos en un intermediario o en la infraestructura.

```python
MAX_CONEXIONES = 1000

if len(manager.active) >= MAX_CONEXIONES:
    await websocket.close(code=1013, reason="Servidor ocupado")
    return
```

Comprueba el límite antes de registrar la conexión y coordina la reserva si varias tareas pueden aceptar conexiones simultáneamente.

## 2. Limita el tamaño de los mensajes

Configura límites en Uvicorn y valida el contenido en la aplicación.

```sh
uv run uvicorn app.main:app \
  --ws websockets \
  --ws-max-size 65536 \
  --ws-max-queue 16
```

- `--ws-max-size`: tamaño máximo del mensaje recibido, en bytes.
    
- `--ws-max-queue`: límite de la cola de recepción, según la implementación WebSocket utilizada.
    

Verifica las opciones disponibles en la versión y el backend de Uvicorn instalados.

## 3. Limita la frecuencia de mensajes

Aplica _rate limiting_ por usuario o conexión. Por ejemplo, permite un máximo de 10 mensajes por segundo y rechaza o limita los excesos.

```python
import time

class RateLimiter:
    def __init__(self, limite: int = 10):
        self.limite = limite
        self.instantes = []

    def permitir(self) -> bool:
        ahora = time.monotonic()
        self.instantes = [
            t for t in self.instantes if ahora - t < 1
        ]

        if len(self.instantes) >= self.limite:
            return False

        self.instantes.append(ahora)
        return True
```

Crea un limitador por conexión o identidad autenticada. Para cargas elevadas, utiliza un algoritmo más eficiente, como _token bucket_, y limita también el número de conexiones por usuario.

## 4. Controla la memoria y el envío

- Utiliza colas acotadas para mensajes pendientes.
    
- Descarta actualizaciones obsoletas cuando solo importa el estado más reciente.
    
- Aplica contrapresión (_backpressure_) a clientes lentos.
    
- Evita acumular mensajes ilimitados en memoria.
    
- Cancela tareas y libera suscripciones al desconectarse el cliente.
    
- Define límites de tiempo para operaciones externas y consultas a bases de datos.
    

## Reglas prácticas

- Dimensiona los límites según la carga real y la memoria disponible.
    
- Aplica límites tanto en el servidor como en el proxy o balanceador.
    
- Supervisa conexiones activas, mensajes por segundo, latencia y memoria.
    
- Devuelve `1013` cuando rechaces una conexión temporalmente por sobrecarga, cuando corresponda.
    
- No consideres los límites en memoria suficientes para una aplicación distribuida.
    

**Referencia:** [Uvicorn — Settings](https://www.uvicorn.org/settings/) · [OWASP — WebSocket Security Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/WebSocket_Security_Cheat_Sheet.html).