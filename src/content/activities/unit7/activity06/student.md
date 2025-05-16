``` plaintext
+---------------------+        +---------------------+        +----------------------+        +---------------------------+
|     Usuario         |        |     Cliente         |        |        Dev Tunnel     |        |     Cliente               |
| (Toque en pantalla, |        |     Móvil           |        |  (Proxy público entre |        |     Escritorio            |
|  color y stroke)    |        |  (mobile/sketch.js) |        |   internet y servidor)|        | (desktop/sketch.js)       |
+---------------------+        +---------------------+        +----------------------+        +---------------------------+
           |                            |                                 |                                 |
           |   Toca la pantalla         |                                 |                                 |
           |--------------------------->|                                 |                                 |
           |                            |                                 |                                 |
           |                            | Crea objeto JSON:               |                                 |
           |                            | { x, y, color, stroke }         |                                 |
           |                            | socket.emit('message', JSON)   |                                 |
           |                            |-------------------------------> |                                 |
           |                            |                                 |                                 |
           |                            |                                 | socket transmite al servidor   |
           |                            |                                 |------------------------------->|
           |                            |                                 |                                 |
           |                            |                                 |                                 |
           |                            |                                 | socket.broadcast.emit('message', JSON)
           |                            |                                 |<-------------------------------|
           |                            |                                 |                                 |
           |                            |                                 |                                 |
           |                            |                                 |  JSON recibido por cliente     |
           |                            |                                 |  de escritorio                 |
           |                            |                                 |                                 |
           |                            |                                 |   JSON.parse(data)             |
           |                            |                                 |   Extrae x, y, color, stroke   |
           |                            |                                 |   Crea nueva partícula         |
           |                            |                                 |   Dibuja en el canvas          |
           |                            |                                 |                                 |
           |                            |                                 |   p5.js -> draw()              |
           |                            |                                 |   ellipse(x, y, size, color)   |
           |                            |                                 |                                 |

```
###  Descripción de los componentes

- *Usuario*: Interactúa con la pantalla del móvil, elige un color y si quiere usar stroke.
- *Cliente Móvil*: Usa p5.js y Socket.IO para capturar los datos del toque y enviarlos al servidor como JSON.
- *Dev Tunnel*: Hace visible el servidor local en Internet para permitir la comunicación desde el cliente móvil.
- *Servidor Node.js*: Recibe los datos y los retransmite a los clientes conectados (en este caso, el escritorio).
- *Cliente de Escritorio*: Recibe los datos, crea una partícula en la posición indicada y la dibuja con las características recibidas usando p5.js.
