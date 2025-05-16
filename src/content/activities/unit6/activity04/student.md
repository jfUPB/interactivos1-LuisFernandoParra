# Actividad 04:

## 1. Variables globales y conexión inicial

**Experimento:**
- Al abrir `page2.html` con el servidor corriendo, no hay errores en la consola.
- Al detener el servidor (Ctrl+C) y refrescar `page2.html`, aparece un error en la consola del navegador indicando que no se pudo establecer la conexión con el servidor de WebSockets. Por ejemplo:  
  `"WebSocket connection to 'ws://localhost:3000/socket.io/...' failed"`
- Este error indica que el cliente no pudo conectarse al servidor porque no está disponible.
- Una vez que se reinicia el servidor y se refresca la página, el error desaparece y la conexión se restablece correctamente.


---

## 2. Manejando la conexión establecida

**Experimento:**
- Al comentar la línea `socket.emit('win2update', currentPageData, socket.id);`, el cliente ya no envía su estado inicial al servidor.
- Al abrir `page1.html` y `page2.html`, `page1` no muestra información sobre `page2` hasta que `page2` se mueve.
- Esto sucede porque el servidor no recibió datos de `page2` al momento de la conexión.
- Al descomentar la línea, `page2` envía su información desde el principio, y `page1` la recibe de inmediato.
 
---

## 3. Recibiendo datos del servidor

**Experimento:**
- El `console.log("Page 2 received data...")` se activa en la consola de `page2` cuando movemos la ventana de `page1`.
- Esto indica que `page2` está recibiendo correctamente los datos emitidos por el servidor mediante `broadcast.emit()`.
- Si movemos la ventana de `page2`, ese log **no** aparece, porque `page2` no recibe sus propios datos. Solo se muestran los datos enviados por `page1`.


---

## 4. Detectando cambios y enviando actualizaciones

**Experimento:**
- El mensaje `"Page 2 detected change..."` aparece en la consola cuando la ventana de `page2` se mueve o cambia de tamaño.
- Si la ventana permanece quieta, el mensaje no aparece, lo que indica que no se envían actualizaciones innecesarias.


---

## 5. La visualización (draw)

**Experimento creativo:**
- Al calcular la distancia entre ventanas usando `resultingVector.mag()` y mapearla al valor del fondo (`background(map(distancia, 0, 1000, 255, 0))`), el fondo cambia de color dependiendo de qué tan lejos está la otra ventana.
- Modificar el tamaño del círculo local usando el ancho de la ventana remota (`remotePageData.width`) hace que el círculo se haga más grande o pequeño dependiendo del tamaño de `page1`.
- Cambiar el color de la línea usando una condición basada en `resultingVector.x` permite distinguir si la otra ventana está a la izquierda o derecha, lo cual enriquece la visualización.


---
