## Actividad 04

### Análisis del código
### 1. Cliente Móvil: mobile/sketch.js– El Emisor
#### Explicación del funcionamiento
``` js

Copiar

Editar
let socket;
let lastTouchX = null;
let lastTouchY = null;
const threshold = 5;
```
threshold: evita que se envíen datos constantemente por mínimos movimientos. Solo se envía si el toque supera los 5 píxeles de cambio.

lastTouchXylastTouchY : recuerdan la última posición enviada, para comparar con la nueva.

```js

Copiar

Editar
function setup() {
    createCanvas(windowWidth, windowHeight);
    background(255);
    socket = io();
    socket.on('connect', () => console.log('Connected to server'));
}
```
Establece la conexión con el servidor usando Socket.IO.

Muestra en consola si la conexión fue exitosa.

```js

Copiar

Editar
function touchMoved() {
    if (socket && socket.connected) {
        let dx = abs(mouseX - lastTouchX);
        let dy = abs(mouseY - lastTouchY);
        
        if (dx > threshold || dy > threshold || lastTouchX === null) {
            let touchData = {
                type: 'touch',
                x: mouseX,
                y: mouseY
            };
            socket.emit('message', JSON.stringify(touchData));
            lastTouchX = mouseX;
            lastTouchY = mouseY;
        }
    }
    return false;
}
```
La función touchMoved()de p5.js se llama automáticamente cuando el usuario desliza el dedo sobre el lienzo.

, se envSi el movimiento supera el threshold, se envía un mensaje al servidor con las coordenadas del toque en formato JSON.

Reflexión (Cliente móvil)
¿Por qué es útil enviar los datos como un objeto JSON ( {type: ‘touch’, x: …, y: …})?

→ Porque el formato JSON es estructurado, flexible y extensible. Permite incluir más información en el futuro, como tipo de interacción, ID de usuario, color, etc. Facilita el manejo de datos tanto en el cliente como en el servidor.

¿Qué pasaría si quitaras la comprobación del threshold?

→ El sistema enviaría demasiados mensajes por cada mínimo movimiento. Esto podría saturar la red, consumir más recursos del cliente y servidor, y generar una experiencia menos fluida.

¿Cómo adaptarías este código para también responder al clic del ratón (para pruebas en computadora)?

→ Se podría agregar un oyente mouseMoved()con lógica similar a touchMoved():

``` js

Copiar

Editar
function mouseMoved() {
    if (socket && socket.connected) {
        let dx = abs(mouseX - lastTouchX);
        let dy = abs(mouseY - lastTouchY);

        if (dx > threshold || dy > threshold || lastTouchX === null) {
            let mouseData = {
                type: 'touch',
                x: mouseX,
                y: mouseY
            };
            socket.emit('message', JSON.stringify(mouseData));
            lastTouchX = mouseX;
            lastTouchY = mouseY;
        }
    }
}
```
## 2. Cliente de Escritorio: desktop/sketch.js– El Receptor
### Explicación del funcionamiento
```js

Copiar

Editar
let socket;
let circleX = 200;
let circleY = 200;
```
Variables para la posición del círculo en el lienzo de escritorio.

```js

Copiar

Editar
function setup() {
    createCanvas(windowWidth, windowHeight);
    background(220);
    socket = io();

    socket.on('connect', () => console.log('Connected to server'));

    socket.on('message', (data) => {
        console.log(Received message: ${data});
        try {
            let parsedData = JSON.parse(data);
            if (parsedData && parsedData.type === 'touch') {
                circleX = parsedData.x;
                circleY = parsedData.y;
            }
        } catch (e) {
            console.error("Error parsing received JSON:", e);
        }
    });
}
```
Se conecta al servidor con Socket.IO.

Escucha eventos message.

Convierte el mensaje recibido de cadena JSON en objeto.

Verifica que sea de tipo 'touch'antes de actualizar la posición del círculo.

```js

Copiar

Editar
function draw() {
    background(220);
    fill(255, 0, 0);
    ellipse(circleX, circleY, 50, 50);
}
```
Dibuja continuamente un círculo rojo en la última posición recibida desde el móvil.

## Reflexión (Cliente de escritorio)
¿Por qué es importante usar JSON.parse()dentro de un bloque try…catch?

→ Porque si el mensaje recibido no es un JSON válido, el programa lanzaría un error y se detendría. El bloque try…catchpermite capturar ese error y seguir ejecutando el código normalmente.

Si los lienzos tuvieran tamaños diferentes, ¿cómo adaptarías la posición del círculo?

→ Usando la función map()de p5.js para reescalar las coordenadas del móvil al tamaño del lienzo del escritorio:

``` js

Copiar

Editar
circleX = map(parsedData.x, 0, 300, 0, width);
circleY = map(parsedData.y, 0, 300, 0, height);
```
Esto supone que el lienzo del móvil mide 300x300 y el escritorio tiene widthy heightvariables.

¿Qué cambiar si el servidor usará el evento 'updateDesktop' en vez de 'message'?

→ Cambiar el nombre del oyente en el cliente de escritorio:

```js

Copiar

Editar
socket.on('updateDesktop', (data) => {
    // misma lógica interna
});
```
 Reporte en Bitácora
 ¿Cuál es el propósito principal de cada guión?
mobile/sketch.js : Captura interacciones táctiles y envía sus coordenadas al servidor usando Socket.IO.

desktop/sketch.js : Recibe esos datos desde el servidor, los interpreta, y dibuja un círculo en la posición indicada, reflejando en tiempo real el movimiento del usuario en el móvil.

 ¿Qué hace la función touchMovedy por qué se usa thresholdy JSON.stringify?
La función touchMoveddetecta movimientos táctiles en el lienzo y, si el cambio es significativo, crea un objeto con las coordenadas y lo convierte a una cadena JSON ( JSON.stringify) para enviarlo al servidor.

Se usa thresholdpara evitar el envío excesivo de datos por mínimos movimientos, lo cual mejora el rendimiento.

 ¿Cómo procesa el escritorio los datos recibidos?
Usa socket.on('message')para escuchar los datos.

Convierta la cadena JSON en un objeto con JSON.parse.

Verifica que el tipo sea 'touch'.

Actualiza las coordenadas del círculo.

Dibuja el círculo continuamente con la función draw().

