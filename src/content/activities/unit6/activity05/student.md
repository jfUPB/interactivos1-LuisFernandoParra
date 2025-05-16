# Actividad 05: Proyecto de Modificación Creativa – EmotiChat

##  Descripción de la idea

*EmotiChat* es una aplicación web interactiva en tiempo real que permite a dos usuarios comunicarse mediante emojis acompañados de mensajes cortos. Cada usuario puede seleccionar un emoji que represente su emoción actual y escribir un mensaje opcional. Esta información se transmite al otro usuario, quien la verá en su pantalla como una burbuja flotante que aparece aleatoriamente y se desvanece automáticamente después de unos segundos. 

Esta aplicación promueve una comunicación emocional rápida, lúdica y visual, distinta al típico sistema de chat basado en texto plano.

---

##  Plan de Implementación

### 1. Conceptualización

- *Interacción principal:* intercambio de emociones representadas por emojis y mensajes cortos.
- *Información compartida:* objeto con emoji (string) y message (string).
- *Visualización:* burbuja flotante con diseño animado que muestra el emoji y el mensaje.

### 2. Cambios realizados al código base

#### server.js
- Se agregó un evento send-emotion que recibe un objeto con los datos de la emoción y el mensaje.
- El servidor luego emite receive-emotion a todos los clientes excepto el emisor.

#### page1.js y page2.js
- Ambos scripts:
  - Detectan clics sobre los botones de emojis.
  - Envían el emoji y el contenido del input a través de send-emotion.
  - Escuchan receive-emotion y renderizan la emoción en pantalla con animación y eliminación automática.

#### page1.html y page2.html
- Estructura básica con:
  - Botones de emojis
  - Campo de texto para mensaje
  - Inclusión del script correspondiente (page1.js o page2.js)

#### style.css
- Se añadió la clase .emotion-bubble con diseño atractivo, posición absoluta, animación de entrada y temporizador de salida.

---

## 🛠️ Desafíos y soluciones

| Desafío | Solución |
|--------|----------|
| Asegurar que el mensaje no se replique en el emisor | Se usó socket.broadcast.emit en lugar de io.emit |
| Mostrar la emoción en un lugar aleatorio sin salir de pantalla | Se calculó la posición usando Math.random() dentro de los márgenes de la ventana (window.innerWidth y window.innerHeight) |
| Manejo de múltiples emociones sin saturar la vista | Cada burbuja se elimina después de 5 segundos con setTimeout(() => bubble.remove(), 5000) |
| Duplicación de lógica en dos archivos JS distintos | Se mantuvo la estructura duplicada (page1.js y page2.js) con la misma lógica, cumpliendo con la estructura original del caso de estudio |

---
# codigo

## page1.js
``` cpp
const socket = io();

// Emitir emoción al hacer clic
document.querySelectorAll(".emoji-button").forEach((button) => {
  button.addEventListener("click", () => {
    const emoji = button.textContent;
    const message = document.getElementById("message-input").value;
    socket.emit("send-emotion", { emoji, message });
  });
});

// Recibir emoción enviada por el otro cliente
socket.on("receive-emotion", (data) => {
  showEmotion(data.emoji, data.message);
});

function showEmotion(emoji, message) {
  const bubble = document.createElement("div");
  bubble.classList.add("emotion-bubble");
  bubble.innerHTML = <span>${emoji}</span><p>${message}</p>;
  document.body.appendChild(bubble);

  // Posición aleatoria
  bubble.style.position = "absolute";
  bubble.style.top = Math.random() * (window.innerHeight - 100) + "px";
  bubble.style.left = Math.random() * (window.innerWidth - 100) + "px";

  // Quitar el mensaje después de 5 segundos
  setTimeout(() => bubble.remove(), 5000);
}
```
## page2.js
``` cpp
const socket = io();

// Emitir emoción al hacer clic
document.querySelectorAll(".emoji-button").forEach((button) => {
  button.addEventListener("click", () => {
    const emoji = button.textContent;
    const message = document.getElementById("message-input").value;
    socket.emit("send-emotion", { emoji, message });
  });
});

// Recibir emoción enviada por el otro cliente
socket.on("receive-emotion", (data) => {
  showEmotion(data.emoji, data.message);
});

function showEmotion(emoji, message) {
  const bubble = document.createElement("div");
  bubble.classList.add("emotion-bubble");
  bubble.innerHTML = <span>${emoji}</span><p>${message}</p>;
  document.body.appendChild(bubble);

  // Posición aleatoria
  bubble.style.position = "absolute";
  bubble.style.top = Math.random() * (window.innerHeight - 100) + "px";
  bubble.style.left = Math.random() * (window.innerWidth - 100) + "px";

  // Quitar el mensaje después de 5 segundos
  setTimeout(() => bubble.remove(), 5000);
}
```
## page1.html
``` cpp
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8" />
  <title>EmotiChat - Page 1</title>
  <link rel="stylesheet" href="style.css" />
</head>
<body>
  <h1>EmotiChat - Usuario 1</h1>

  <div id="chat-interface">
    <button class="emoji-button">😊</button>
    <button class="emoji-button">😡</button>
    <button class="emoji-button">😢</button>
    <button class="emoji-button">🎉</button>
    <input type="text" id="message-input" placeholder="Mensaje opcional..." />
  </div>

  <script src="/socket.io/socket.io.js"></script>
  <script src="page1.js"></script>
</body>
```
## page2.html
``` cpp
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8" />
  <title>EmotiChat - Page 2</title>
  <link rel="stylesheet" href="style.css" />
</head>
<body>
  <h1>EmotiChat - Usuario 2</h1>

  <div id="chat-interface">
    <button class="emoji-button">😊</button>
    <button class="emoji-button">😡</button>
    <button class="emoji-button">😢</button>
    <button class="emoji-button">🎉</button>
    <input type="text" id="message-input" placeholder="Mensaje opcional..." />
  </div>

  <script src="/socket.io/socket.io.js"></script>
  <script src="page2.js"></script>
</body>
</html>
```
## server.js
``` cpp
const express = require('express');
const http = require('http');
const socketIO = require('socket.io');
const path = require('path');
const app = express();
const server = http.createServer(app); 
const io = socketIO(server); 
const port = 3000;

let page1 = { x: 0, y: 0, width: 100, height: 100 };
let page2 = { x: 0, y: 0, width: 100, height: 100 };

app.use(express.static(path.join(__dirname, 'views')));

app.get('/page1', (req, res) => {
    res.sendFile(path.join(__dirname, 'views', 'page1.html'));
});

app.get('/page2', (req, res) => {
    res.sendFile(path.join(__dirname, 'views', 'page2.html'));
});

io.on("connection", (socket) => {
    console.log("Usuario conectado:", socket.id);
  
    socket.on("send-emotion", (data) => {
      socket.broadcast.emit("receive-emotion", data);
    });
});

server.listen(port, () => {
    console.log(`Server is listening on http://localhost:${port}`);
});


  <script src="/socket.io/socket.io.js"></script>
  <script src="page2.js"></script>
</body>
</html>
```


##  Reflexión Final

###  ¿Qué fue fácil?
- Aprovechar la estructura base de socket.io.
- Detectar eventos de botón y enviar mensajes entre clientes.

###  ¿Qué fue difícil?
- Controlar que los eventos no regresaran al emisor.
- Posicionar las burbujas correctamente para que sean visibles pero no molestas.

###  ¿Qué aprendí?
- Cómo reutilizar la infraestructura de comunicación en tiempo real para construir una aplicación distinta.
- Que las emociones también se pueden comunicar visualmente de manera efectiva.
- Que una interacción sencilla como enviar un emoji puede volverse una experiencia significativa con pequeños toques de diseño y animación.

---
