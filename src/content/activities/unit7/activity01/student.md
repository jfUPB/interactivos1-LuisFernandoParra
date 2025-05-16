### ¿Por qué usamos esta URL en lugar de localhost?

Se utiliza la URL de Dev Tunnels ya que `localhost:3000` únicamente funciona en el mismo equipo que ejecuta el servidor. Como el celular no está corriendo el servidor de forma local, necesita acceder a él desde la red. Dev Tunnels permite eso al generar una URL pública temporal que comparte el servidor de manera segura hacia el exterior.

### ¿Qué hacen npm install y npm start?

El comando `npm install` se encarga de descargar e instalar las librerías y módulos definidos en el `package.json`, que son esenciales para que el servidor funcione.  
Por su parte, `npm start` lanza el script de arranque configurado en ese archivo, que generalmente ejecuta algo como `node server.js`.

### Mensajes en la terminal del servidor

Al conectar los clientes, se mostraron mensajes en la terminal como:

- Nuevo cliente conectado  
- Mensaje recibido => {"x": 157, "y": 283}  
- Cliente desconectado  

Cada conexión estaba identificada con un `socket.id` distinto, lo cual le permitía al servidor distinguir entre los diferentes clientes aunque enviaran datos similares.

### Observaciones sobre la interacción

La interacción fue exitosa: al tocar la pantalla del móvil, el círculo en el computador se desplazaba de manera fluida.  
La latencia fue muy baja, casi imperceptible, lo que hizo que la comunicación se sintiera en tiempo real.  
En general, la experiencia fue intuitiva y fácil de comprobar.

### ¿Cerré el puerto?

Sí, el puerto fue cerrado una vez concluidas las pruebas.

### ¿Por qué es importante hacerlo?

Porque dejar el puerto abierto puede dejar el servidor expuesto a accesos no deseados desde Internet. Al cerrarlo, se refuerza la seguridad y se evita un uso innecesario de recursos.