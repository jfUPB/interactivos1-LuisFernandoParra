``` JS
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
    square(200, 200, 55);
}

function draw() {

    if(port.availableBytes() > 0){
        let dataRx = port.read(1);
        if(dataRx == 'A'){
            fill('red');
        }
        background(220);
        noStroke();
        square(200, 200, 55);
        fill('white');
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
Para completar este ejercicio, tomé el código de la actividad anterior que permitía la conexión con micro:bit y reutilicé el mismo código de p5.js. Realicé algunas modificaciones, como sustituir el círculo por un cuadrado con relleno blanco, eliminar las funciones que manejaban otras interacciones y conservar solo la que responde al botón ‘A’. Además, cambié el color del cuadrado utilizando la función fill.
