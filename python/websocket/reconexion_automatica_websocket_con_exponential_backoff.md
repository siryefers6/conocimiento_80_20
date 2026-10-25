# Reconexión automática WebSocket con exponential backoff

Implementa reintentos automáticos cuando una conexión WebSocket se interrumpa. Aumenta progresivamente el tiempo de espera y añade variación aleatoria (_jitter_) para evitar que muchos clientes se reconecten simultáneamente.

## Estrategia de reintentos

Calcula la espera con crecimiento exponencial y un límite máximo:

[  
t_n = \min(t_{\max}, t_0 \cdot 2^n)  
]

Añade _jitter_ aleatorio para distribuir los reintentos. Reinicia el contador después de establecer una conexión estable.

## Implementación en JavaScript

```js
const URL_WS = "wss://ejemplo.com/ws";
const ESPERA_INICIAL = 1000;
const ESPERA_MAXIMA = 30000;

let ws;
let intento = 0;
let temporizador;
let detenido = false;

function conectar() {
  if (detenido) return;

  ws = new WebSocket(URL_WS);

  ws.addEventListener("open", () => {
    intento = 0;
    console.log("Conexión establecida");
  });

  ws.addEventListener("message", (event) => {
    console.log("Mensaje:", event.data);
  });

  ws.addEventListener("error", () => {
    // El evento close gestionará la reconexión.
    console.error("Error WebSocket");
  });

  ws.addEventListener("close", () => {
    if (detenido) return;

    const base = Math.min(
      ESPERA_MAXIMA,
      ESPERA_INICIAL * 2 ** intento
    );

    const espera = Math.random() * base;
    intento++;

    temporizador = setTimeout(conectar, espera);
  });
}

function desconectar() {
  detenido = true;
  clearTimeout(temporizador);
  ws?.close(1000, "Cierre solicitado");
}

conectar();
```

## Reglas prácticas

- Usa _exponential backoff_ con _jitter_ y un límite máximo.
    
- Gestiona la reconexión desde `close` para evitar programar reintentos duplicados desde `error`.
    
- Distingue entre cierre voluntario y desconexión inesperada.
    
- Limita los reintentos o aplica una política de espera prolongada ante fallos persistentes.
    
- Renueva credenciales vencidas antes de volver a conectar.
    
- Restablece las suscripciones y recupera el estado después de reconectar.
    
- No supongas que los mensajes enviados antes de la desconexión fueron recibidos: implementa confirmaciones si necesitas entrega fiable.
    

**Referencia:** [MDN — WebSocket](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket).