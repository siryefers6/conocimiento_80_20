# Despliegue WebSocket con Nginx, HTTPS y WSS

Configura el proxy inverso para mantener conexiones WebSocket persistentes, habilitar TLS y evitar cierres prematuros por timeouts.

## 1. Configuración de Nginx

```
map $http_upgrade $connection_upgrade {
    default upgrade;
    ''      close;
}

server {
    listen 443 ssl;
    server_name ejemplo.com;

    ssl_certificate     /etc/letsencrypt/live/ejemplo.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/ejemplo.com/privkey.pem;

    location /ws/ {
        proxy_pass http://127.0.0.1:8000;

        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection $connection_upgrade;

        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;

        proxy_read_timeout 3600s;
        proxy_send_timeout 3600s;
        proxy_buffering off;
    }
}
```

Ajusta `proxy_pass` para conservar o eliminar el prefijo `/ws/` según las rutas definidas en FastAPI.

## 2. Ejecuta FastAPI con Uvicorn

```sh
uv run uvicorn app.main:app \
  --host 127.0.0.1 \
  --port 8000 \
  --proxy-headers \
  --forwarded-allow-ips=127.0.0.1
```

Confía únicamente en los proxies conocidos. No expongas directamente el puerto de Uvicorn a Internet si Nginx debe controlar el acceso.

## 3. Conecta desde JavaScript

```js
const ws = new WebSocket("wss://ejemplo.com/ws/");

ws.onopen = () => console.log("Conectado");
ws.onmessage = (event) => console.log(event.data);
ws.onclose = (event) => console.log("Cierre:", event.code);
```

Usa `wss://` cuando la aplicación web se sirva mediante HTTPS. Los navegadores bloquean normalmente las conexiones `ws://` desde páginas HTTPS por considerarlas contenido mixto.

## 4. Ajusta los timeouts

- Configura `proxy_read_timeout` por encima del intervalo máximo entre datos recibidos desde el servidor.
    
- Configura `proxy_send_timeout` según el comportamiento de envío y las necesidades de la aplicación.
    
- Implementa heartbeat para mantener activa la conexión cuando no haya tráfico.
    
- Ajusta los timeouts del balanceador, firewall y proveedor de infraestructura.
    
- Recuerda que estos timeouts miden períodos de inactividad entre operaciones de E/S, no la duración total de la conexión.
    

## 5. Verifica el despliegue

```sh
sudo nginx -t
sudo systemctl reload nginx
```

Comprueba la conexión desde el navegador, verifica que el handshake responda con `101 Switching Protocols` y revisa los registros de Nginx y Uvicorn ante errores `502`, `504` o cierres inesperados.

## Reglas prácticas

- Gestiona los certificados TLS y su renovación.
    
- Limita conexiones y recursos por usuario.
    
- Implementa reconexión y recuperación del estado del cliente.
    
- Usa Redis Pub/Sub u otro intermediario si despliegas varios procesos que deben compartir eventos.
    
- Planifica el cierre controlado de conexiones durante actualizaciones del servidor.
    

**Referencia:** [Nginx — WebSocket proxying](https://nginx.org/en/docs/http/websocket.html) · [Uvicorn — Settings](https://www.uvicorn.org/settings/).