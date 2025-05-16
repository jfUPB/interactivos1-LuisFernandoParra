### ¿Por qué es necesario Dev Tunnels en este escenario y cómo funciona conceptualmente?

Dev Tunnels es esencial en este caso porque el servidor está ejecutándose localmente en el computador, y queremos que un dispositivo externo —como un celular— pueda conectarse a él. Como el celular no puede acceder directamente al servidor local, necesitamos una herramienta como Dev Tunnels que haga de puente entre ambos. Una opción alternativa sería usar la IP del PC, pero eso requiere que ambos dispositivos estén en la misma red Wi-Fi, y aun así podrían existir cortafuegos que bloqueen el acceso.

En cuanto a su funcionamiento, Dev Tunnels actúa como un intermediario: recibe las peticiones del celular y las redirige al servidor local, luego toma la respuesta del servidor y se la devuelve al celular. Todo esto se realiza de forma segura, ya que implica comunicación desde un dispositivo externo hacia una máquina local.

---

### ¿Por qué usamos JSON.stringify() en el emisor (móvil) y JSON.parse() en el receptor (escritorio)? ¿Qué problema resuelve JSON aquí?

Esto se hace porque la mayoría de los datos enviados por red deben ir en formato de texto. No se pueden transmitir variables tal cual, sin estructura. Por eso, en el celular usamos JSON.stringify() para convertir los datos a una cadena. Cuando el servidor los recibe, utiliza JSON.parse() para reconstruir esa cadena en un objeto que se pueda manipular como los datos originales.

---

### Describe la función de touchMoved() y por qué se usa la variable threshold en el cliente móvil.

La función touchMoved() detecta el movimiento del dedo en la pantalla táctil dentro del lienzo de p5.js, registrando su posición en coordenadas (x, y). A medida que el dedo se desplaza, esta función va capturando ese trayecto, lo cual permite actualizar la posición del círculo en el otro cliente.

El threshold actúa como filtro para evitar enviar información innecesaria. Si el movimiento del dedo es muy leve —por ejemplo, un pequeño temblor— no se envía nada al servidor. Solo si el cambio de posición supera ese umbral, se considera un movimiento válido y se comunica al servidor.

---

### ¿Qué otros eventos táctiles existen en p5.js y para qué tipo de interacciones podrían ser útiles?

p5.js incluye varios eventos táctiles que pueden aprovecharse según la interacción que se quiera diseñar. Por ejemplo, touchStarted() puede servir para detectar cuando alguien toca la pantalla, como en un botón virtual. touchEnded() puede indicar que se dejó de tocar. Un simple tap podría cambiar el color de un objeto o activar una función. Todo depende de la creatividad y del tipo de experiencia que se busque.

---

### Compara brevemente Dev Tunnels con simplemente usar la IP local. ¿Cuáles son las ventajas y desventajas de cada uno?

*Dev Tunnels*

- *Ventajas:*
  - Funciona incluso si el celular está usando datos móviles o está en otra red.
  - Proporciona una URL pública accesible desde cualquier navegador moderno.
  - Es ideal para probar en múltiples dispositivos o compartir con otras personas fácilmente.

- *Desventajas:*
  - Puede haber algo de latencia.
  - La URL cambia cada vez que se abre un nuevo túnel.

*IP local*

- *Ventajas:*
  - Es una conexión directa sin intermediarios.
  - No requiere herramientas externas.

- *Desventajas:*
  - Solo es útil si ambos dispositivos están conectados a la misma red Wi-Fi.
  - Puede ser menos seguro si no se controla el acceso a esa IP.
