ACTIVIDAD 1:
### ¿Qué ocurrió en la terminal cuando ejecutaste npm install? ¿Cuál crees que es su propósito?
Al ejecutar npm install, la terminal empezó a bajar e instalar varios paquetes y dependencias esenciales para que el proyecto funcione. La función principal de este comando es instalar todas las dependencias especificadas en el archivo package.json, indispensables para el correcto funcionamiento del proyecto.

### ¿Qué mensaje específico apareció en la terminal después de ejecutar npm start? ¿Qué indica este mensaje?
Tras ejecutar npm start, apareció en la terminal el mensaje: Server listening on http://localhost:3000 User connected. Esto significa que el servidor se ha levantado exitosamente y está listo para aceptar conexiones a través del puerto 3000.

### Describe lo que ves inicialmente en page1 y page2 en tu navegador
Al acceder a http://localhost:3000/page1, se mostró una página con un fondo claro y un elemento gráfico (un círculo) que respondía al movimiento de la ventana. Su diseño era simple. En http://localhost:3000/page2, apareció una interfaz relacionada que parecía estar sincronizada con la primera. También presentaba elementos visuales que reflejaban el estado de la página 1, como la posición de la línea que conecta ambos círculos.

### ¿Qué mensajes aparecieron en la terminal del servidor cuando abriste page1 y page2?
En la terminal del servidor aparecieron mensajes como: Client connected.

### Describe qué sucede en ambas páginas del navegador cuando mueves una de las ventanas. ¿Cambia algo visualmente? ¿Qué mensajes aparecen (si los hay) en la consola del navegador y en la terminal del servidor?
Al mover el elemento en la página page1, el cambio se reflejaba automáticamente en la dirección de la línea de la página page2, y viceversa. Esto confirma que ambas páginas están sincronizadas en tiempo real. En la consola del navegador (F12 → Consola) se mostraba: Object height: 671 width: 395 x: -16 y: 55 [[Prototype]]: Object
