#### Que querias comprobar?
queria mirar que sucede cuando se presionan los botones A Y B tambien el logo
#### Mi hipotesis:
Al usar los botones estos seran leidos por el microbit por medio del puerto usb y dependiendo de lo que se presione se realizara una accion diferente
#### Codigos usados:
```
let port;
let connectBtn;

function setup() {
    createCanvas(400, 400);
    background(220);
    port = createSerial();

    connectBtn = createButton('Connect to micro:bit');
    connectBtn.position(80, 300);
    connectBtn.mousePressed(connectBtnClick);

    let sendBtn = createButton('Send Love');
    sendBtn.position(220, 300);
    sendBtn.mousePressed(sendBtnClick);

    fill('white');
    ellipse(width / 2, height / 2, 100, 100);
}

function draw() {
    if (port.availableBytes() > 0) {
        let dataRx = port.read(1);

        if (dataRx == 'A') {
            fill('red');  // Si se presiona el botón A
        }
        else if (dataRx == 'B') {
            fill('yellow');  // Si se presiona el botón B
        }
        else if (dataRx == 'L') {
            fill('blue');  // Si se toca el logo
        }
        else {
            fill('green');  // Estado por defecto
        }

        background(220);
        ellipse(width / 2, height / 2, 100, 100);
        fill('black');
        textSize(20);
        textAlign(CENTER, CENTER);
        text(dataRx, width / 2, height / 2);
    }

    if (!port.opened()) {
        connectBtn.html('Connect to micro:bit');
    } else {
        connectBtn.html('Disconnect');
    }
}

function connectBtnClick() {
    if (!port.opened()) {
        port.open('MicroPython', 115200);
    } else {
        port.close();
    }
}

function sendBtnClick() {
    port.write('h');
}
```
```
from microbit import *

while True:
    if button_a.is_pressed():
        display.show("A")
    elif button_b.is_pressed():
        display.show("B")
    elif pin_logo.is_touched():
        display.show("L")  # L de "logo"
    else:
        display.clear()
```
#### Resultados:
se supone que cuando se presione el boton A Y B o el logo se muestra un circulo en pantalla que tambien cambiara de color
#### Analisis:
En p5.js, el micro:bit envía datos a la computadora a través de un puerto que este conectado a la computadoraa, permitiendo actualizar la interfaz.
El MicroPython usa is_pressed() y pin_logo.is_touched() para detectar los eventos.
#### Mis conclusiones:
 observé que los botones A y B funcionan de manera similar, mientras que el logo capacitivo ofrece una alternativa táctil interesante. Estos resultados me ayudan a entender mejor cómo integrar entradas físicas en proyectos de entretenimiento digital, permitiéndome crear experiencias más dinámicas e intuitivas.



