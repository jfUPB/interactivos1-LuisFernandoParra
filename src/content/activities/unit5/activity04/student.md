### Enlace:
https://openprocessing.org/sketch/create
### Codigo modificado:
```
let serial;
let deviceIsConnected = false;
let posX = 100 / 2;
let posY = 100 / 2;
let dataBuffer = [];

function setup() {
  createCanvas(500, 500);
  background(100);

  serial = createSerial();

  let btnConectar = createButton("Connect to device");
  btnConectar.position(10, 10);
  btnConectar.mousePressed(() => {
    if (!serial.opened()) {
      serial.open("MicroPython", 115200);
    } else {
      serial.close();
    }
  });
}

function draw() {
  background(100);

  if (!serial.opened()) {
    deviceIsConnected = false;
  } else {
    deviceIsConnected = true;
    recibirDatosSerial();
  }

  fill(255);
  circle(posX, posY, 20);
}

function recibirDatosSerial() {
  let cantidadDisponible = serial.availableBytes();
  if (cantidadDisponible > 0) {
    let bytesNuevos = serial.readBytes(cantidadDisponible);
    dataBuffer = dataBuffer.concat(bytesNuevos);
  }

  while (dataBuffer.length >= 8) {
    if (dataBuffer[0] !== 0xAA) {
      dataBuffer.shift(); // Descartar byte hasta hallar encabezado
      continue;
    }

    if (dataBuffer.length < 8) break;

    let paquete = dataBuffer.slice(0, 8);
    dataBuffer.splice(0, 8); // Eliminar paquete del buffer

    let datos = paquete.slice(1, 7);
    let checksumRecibido = paquete[7];
    let checksumCalculado = datos.reduce((acc, val) => acc + val, 0) % 256;

    if (checksumCalculado !== checksumRecibido) {
      console.log("Error de checksum");
      continue;
    }

    let arreglo = new Uint8Array(datos).buffer;
    let visor = new DataView(arreglo);
    posX = visor.getInt16(0) + width / 2;
    posY = visor.getInt16(2) + height / 2;
  }
}

```
Link:
https://editor.p5js.org/LuisFernandoParra/sketches/1gz3vy0is
