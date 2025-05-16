# Activida 2

## 1.  Pausa activa: ¿Qué pasaría si se corta tu acceso a Internet?

En casa me conecto a Internet principalmente a través de Wi-Fi, y en la universidad también suele ser inalámbrico. Esta conexión es mi "rampa de acceso" a la red de redes.  
Si esa rampa se corta (por ejemplo, si el router deja de funcionar o hay un corte de energía), simplemente no podría acceder a Internet. No se podrían cargar páginas web, enviar correos ni usar aplicaciones que dependen de la nube. Estaría como un coche varado, sin carretera por la que circular.

---

## 2.  Pausa activa: Ejemplos de relaciones Cliente-Servidor en la vida diaria

Un ejemplo claro es un *restaurante*:
- El *cliente* es la persona que se sienta a la mesa y pide comida.
- El *servidor* (mesero o camarero) toma la orden, la transmite a la cocina y luego entrega la comida al cliente.
- Se pide un plato específico (como si fuera un recurso en la web), y se entrega una respuesta (el plato servido).

Otro ejemplo sería una *biblioteca física*:
- El usuario (cliente) solicita un libro.
- El bibliotecario (servidor) localiza y entrega el libro solicitado.

---

## 3.  Pausa activa: Analizando una URL

Tome como ejemplo la URL: https://www.youtube.com/watch?v=dQw4w9WgXcQ

- *Protocolo*: https://
- *Nombre de dominio*: www.youtube.com
- *Ruta*: /watch?v=dQw4w9WgXcQ

Si solo escribo www.youtube.com, el servidor probablemente me envíe una *página por defecto*, como la página principal o de inicio personalizada del sitio, usualmente algo como index.html.

---

## 4.  Pausa activa: Comparando HTTP con protocolos seriales

### Similitudes:
- Ambos protocolos definen *reglas de comunicación* entre dos entidades.
- Son necesarios para que los dispositivos "hablen el mismo idioma".
- Incluyen estructuras específicas de petición y respuesta.

### Diferencias:
- HTTP es mucho más complejo y tiene más capas (cabeceras, códigos de estado, tipos de contenido).
- Los protocolos seriales suelen ser más directos y simples, adecuados para dispositivos con recursos limitados.

### ¿Por qué HTTP es más complejo?
Porque debe gestionar múltiples tipos de datos, seguridad, estados de sesión, tipos de archivos, errores, etc. Además, opera a través de una infraestructura mucho más variada y abierta que una simple conexión serial.

---

## 5.  Pausa activa: HTML, CSS y JavaScript en un formulario de login

- *HTML*: Define los elementos como los campos de usuario y contraseña, y el botón de "Iniciar sesión".
- *CSS*: Aplica estilos como el color del botón, las fuentes, el diseño visual del formulario.
- *JavaScript*: Verifica que los campos no estén vacíos, muestra mensajes de error sin recargar la página, o realiza la conexión con el servidor para validar los datos.

---

## 6.  Pausa activa: ¿Cuándo y cómo se ejecuta JavaScript?

El JavaScript se ejecuta:
- *Durante la carga*, si el script está en la parte superior del HTML.
- *Después del DOM*, si se marca con defer o se ubica al final del <body>.

Esto permite que el JS tenga acceso completo al DOM y pueda manipularlo correctamente. El navegador gestiona los tiempos de ejecución para que todo fluya sin errores de referencia a elementos aún no cargados.

---

## 7.  Pausa activa: Ventajas del modelo basado en eventos

- *Ventajas*:
  - Es *más eficiente*, ya que solo ejecuta código cuando es necesario (cuando ocurre un evento).
  - Mejora la *experiencia del usuario*, porque las interacciones son inmediatas y específicas.
  - Permite un *mejor rendimiento* en dispositivos con recursos limitados.

- ¿Tendría sentido un bucle draw() para toda una web?
  No. Redibujar toda la página 60 veces por segundo sin cambios sería un desperdicio de recursos. Las interfaces modernas solo reaccionan cuando algo cambia, lo cual es mucho más eficiente.

---

## 8.  Pausa activa: Ventajas de usar JavaScript en cliente y servidor

- *Facilidad de aprendizaje*: Un solo lenguaje para front-end y back-end.
- *Reutilización de código*: Se pueden compartir funciones y utilidades entre ambos entornos.
- *Mayor coherencia*: Menos errores por cambio de paradigma entre lenguajes.
- *Más rápido desarrollo*: Equipos de desarrollo más integrados y menos fragmentados.

---

## 9.  Pausa activa final: Comparación HTTP vs WebSockets/Socket.IO

### Diferencia fundamental:
- *HTTP*: Comunicación unidireccional, basada en petición/respuesta. El cliente siempre inicia la conversación.
- *WebSockets/Socket.IO: Comunicación **bidireccional y persistente*. Ambos lados pueden enviar mensajes en cualquier momento, sin necesidad de una nueva petición.

### ¿Dónde se usa esto?
- *Chats en tiempo real* (como WhatsApp Web).
- *Juegos multijugador en línea*.
- *Aplicaciones colaborativas* (como Google Docs).
- *Monitoreo en tiempo real* (sensores, precios de acciones, mapas en vivo).

---
