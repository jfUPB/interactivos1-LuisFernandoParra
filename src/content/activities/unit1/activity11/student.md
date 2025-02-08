``` JS
let port;
let connectBtn;

let x;

function setup() {
    createCanvas(400, 400);
    background(220);
    port = createSerial();
    connectBtn = createButton('Connect to micro:bit');
    connectBtn.position(80, 300);
    connectBtn.mousePressed(connectBtnClick);
    fill('white');
    circle(50,50,50);

    x = 20
}

function draw() {

    if(port.availableBytes() > 0){
        let dataRx = port.read(1);
        if(dataRx == 'A'){
            x = x+20
            translate(x, 0);
        }
        else if(dataRx == 'B'){
            x = x - 20
            translate(x, 0);
        }
      background(220);
      fill('white');
      circle(50,50,50);
    }


    if (!port.opened()) {
        connectBtn.html('Connect to micro:bit');
    }
    else {
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
### Como funciona?
#### El micro:bit detecta cuando se presionan los botones:
- Si se presiona el botón A, envía la letra ‘A’
- Si se presiona el botón B, envía la letra ‘B’
p5.js recibe estos datos y actualiza la posición del círculo en la pantalla. La función port.read(1) captura la información enviada por el micro:bit y, según el valor recibido (‘A’ o ‘B’), se modifica la ubicación del círculo
### Resultados observados:
- Al presionar el botón B, el círculo se desplaza hacia la izquierda, mientras que al presionar el botón A, se mueve hacia la derecha
- La interfaz de p5.js incluye un botón para conectar y desconectar el micro:bit
- La comunicación en serie entre el micro:bit y p5.js permite actualizar la posición del círculo en tiempo real
