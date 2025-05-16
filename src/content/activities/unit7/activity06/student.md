### Mi solución a la actividad 6

```
┌────────────┐         ┌────────────┐         ┌────────────┐         ┌────────────┐         ┌──────────────┐
│   Usuario  │ ─────▶ │  Móvil     │ ─────▶ │ Dev Tunnel  │ ─────▶ │  Servidor   │ ─────▶ │ Escritorio    │
│ (Toque)    │        │ (sketch.js)│         │ (Proxy)     │         │ (server.js) │         │ (sketch.js)   │
└────────────┘         └────────────┘         └────────────┘         └────────────┘         └──────────────┘
       │                       │                      │                      │                        │
       └── Coordenadas         └─ socket.emit(...)    └─ Redirecciona        └─ socket.broadcast      └─ Dibuja partícula
            color, stroke                                                       a escritorio
```
Usuario: toca la pantalla del móvil para generar eventos. Controla posición, color y stroke de la partícula.

Móvil (sketch.js): captura el evento touch, genera un objeto JSON con x, y, color, y stroke, y lo envía al servidor mediante socket.emit('message', json).

Dev Tunnel: permite que el servidor local sea accesible públicamente. Actúa como puente entre internet y tu máquina local.

Servidor (server.js + Socket.IO): recibe el mensaje del móvil, y lo reenvía al cliente de escritorio usando socket.broadcast.emit('message', data).

Escritorio (sketch.js): recibe el JSON, interpreta los datos, y crea una nueva partícula con las características recibidas. Dibuja las partículas en el canvas.
