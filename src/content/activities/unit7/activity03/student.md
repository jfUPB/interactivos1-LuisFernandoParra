## Actividad 3

### Función de express.static('public') en el servidor

La función express.static('public') permite al servidor servir archivos estáticos desde la carpeta public. Esto incluye recursos como HTML, CSS, imágenes y JavaScript. Así, cuando el navegador solicita un archivo, el servidor lo busca directamente en esa carpeta y lo entrega como respuesta.

*Comparación con app.get('/ruta', …'):*  
En la Unidad 6 se utilizaba app.get() para definir rutas específicas, lo que implicaba configurar manualmente cada ruta. En cambio, express.static automatiza este proceso y lo simplifica.

---

### Flujo de un mensaje táctil

*Envío desde el móvil:*  
El evento touchMoved() se encarga de detectar el movimiento del dedo en la pantalla. Cuando esto ocurre, se capturan las coordenadas (x, y) y se envían al servidor usando Socket.IO.

*Recepción en el servidor:*  
El servidor está preparado para recibir esos datos mediante el evento 'message' con socket.on('message', ...).

*Acción del servidor:*  
Una vez que el servidor recibe el mensaje, lo reenvía a los demás clientes utilizando socket.broadcast.emit('message', message).

*Envío al escritorio:*  
El servidor distribuye el mensaje recibido a todos los demás clientes (como los navegadores en escritorio) excepto al que lo envió originalmente.

---

### Uso de socket.broadcast.emit

Este método se utiliza porque envía el mensaje a todos los clientes conectados, menos al emisor. Si se usara socket.emit, el mensaje solo iría de vuelta al mismo cliente que lo envió. En cambio, io.emit lo enviaría a todos, incluyendo al emisor. broadcast.emit es ideal para evitar enviar el mensaje de regreso al dispositivo móvil que lo originó.

---

### ¿Quién recibiría el mensaje retransmitido?

Ambos computadores de escritorio recibirían el mensaje que el servidor reenvía, ya que socket.broadcast.emit lo envía a todos los clientes conectados excepto al que lo originó (en este caso, el móvil).

---

### Mensajes console.log en el servidor

Los mensajes que aparecen en consola, como "Nuevo cliente conectado", "Mensaje recibido" o "Cliente desconectado", ayudan a monitorear el estado del servidor, identificar conexiones activas y verificar que la comunicación entre clientes y servidor esté funcionando correctamente.
