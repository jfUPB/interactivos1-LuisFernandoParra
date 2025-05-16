# 1. Reflexión sobre el uso de módulos o librerías  
*¿Por qué crees que es útil usar módulos o librerías en lugar de escribir todo desde cero? ¿Qué ventajas aporta?*

El uso de módulos o librerías representa un ahorro significativo de tiempo y esfuerzo, ya que muchas funciones ya vienen desarrolladas, comprobadas y optimizadas. Esto incrementa la eficiencia en el desarrollo, facilita la gestión del código y disminuye la posibilidad de errores. Además, permite concentrarse en la lógica específica del proyecto sin tener que desarrollar todo desde cero.

# 4. Experimentación con express.static  
*¿Qué ocurre al cambiar la línea a una carpeta que no existe (archivos_cliente)? ¿Qué mensaje ves?*

Al modificar la ruta de views a archivos_cliente, que no es una carpeta existente, el servidor no puede localizar los archivos estáticos como page1.html. Como resultado, al acceder a http://localhost:3000/page1.html en el navegador, aparece un error 404 (No encontrado). En la consola del navegador se muestra un mensaje del tipo “Failed to load resource”. Al volver a configurar correctamente la carpeta como views, todo vuelve a funcionar sin problemas.

# 5. Experimentación con rutas  
*¿Qué ocurre al cambiar la ruta /page1 por /pagina_uno?*

Después de cambiar la ruta, al intentar acceder a http://localhost:3000/page1 se produce un error 404, ya que esa dirección ya no está disponible. En cambio, si se accede a http://localhost:3000/pagina_uno, el navegador carga correctamente el archivo page1.html.  
*Conclusión:* Es fundamental que las rutas coincidan exactamente con las que se definieron en el código, ya que el servidor solo responde a las URLs programadas explícitamente.

# 6. Observación de conexión y desconexión  
*Preguntas:*

- ¿Qué mensaje ves al abrir page1?  
- ¿Y al abrir page2?  
- ¿Los IDs son diferentes?  
- ¿Qué mensaje aparece al cerrar page1?  
- ¿Y al cerrar page2?

*Respuestas:*

Al abrir http://localhost:3000/page1, en la terminal se muestra el mensaje: "A user connected - ID: <id_unico>".  
Cuando se abre http://localhost:3000/page2, aparece un mensaje similar pero con un ID diferente.  
Sí, los identificadores son distintos para cada usuario conectado.  
Al cerrar la pestaña de page1, se muestra: "User disconnected - ID: <id_unico_de_page1>".  
Y al cerrar page2, se genera un mensaje similar con el ID correspondiente a esa conexión.

# 7. Prueba de recepción de eventos y broadcast  
*Preguntas:*

- ¿Qué evento se registra al mover page1?  
- ¿Y al mover page2?  
- ¿Qué ocurre si se cambia broadcast.emit por emit?  
- ¿Se actualiza page2 al mover page1?

*Respuestas:*

Cuando se mueve la ventana de page1, en la terminal se registra el evento win1update junto con los datos enviados.  
Al mover page2, aparece el evento win2update con su respectiva información.  
Si se reemplaza socket.broadcast.emit('getdata', page1) por socket.emit('getdata', page1), el mensaje solo se envía al cliente que originó la acción, sin propagarse a los demás.  
Por lo tanto, page2 no recibe la información actualizada de page1, ya que emit no transmite datos a otros sockets conectados. Por eso es crucial utilizar broadcast.emit.

# 8. Cambio de puerto  
*¿Qué ocurre al cambiar el puerto a 3001?*

En la consola se muestra el mensaje: "Server is listening on http://localhost:3001", lo que indica que el servidor ahora opera en el nuevo puerto. Si se intenta acceder mediante http://localhost:3000, la aplicación no funcionará. Es necesario ingresar a http://localhost:3001 para poder visualizarla.
