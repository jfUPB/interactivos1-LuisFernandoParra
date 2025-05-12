### Enlace:
https://openprocessing.org/sketch/create
### Codigo modificado:
```
let serial;
let x = 0;
let y = 0;
let buttonA = false;
let buttonB = false;

function setup() {
  createCanvas(400, 400);
  serial = new p5.SerialPort();
  
  // Lista de puertos disponibles en consola
  serial.list();
  
  // Abre el puerto una vez lo selecciones desde la consola del navegador
  serial.open('/dev/tty.usbmodemXXXX'); // Sustituye esto por tu puerto correcto

  serial.on('data', serialEvent);
  serial.clear(); // Limpia el buffer
}

function draw() {
  background(220);

  // Representación visual de datos
  fill(0);
  textSize(16);
  text(`x: ${x}`, 20, 40);
  text(`y: ${y}`, 20, 70);
  text(`Botón A: ${buttonA}`, 20, 100);
  text(`Botón B: ${buttonB}`, 20, 130);

  // Dibujar un círculo que se mueve según acelerómetro
  fill(100, 200, 255);
  ellipse(map(x, -2048, 2048, 0, width), map(y, -2048, 2048, 0, height), 30);
}

function serialEvent() {
  while (serial.available() >= 10) {
    let header = serial.read();

    if (header === 0xAA) {
      let buffer = [];

      // Leer los 8 bytes de datos (2h2B)
      for (let i = 0; i < 8; i++) {
        buffer.push(serial.read());
      }

      // Leer el checksum
      let checksum = serial.read();

      // Validar checksum
      let sum = buffer.reduce((acc, val) => acc + val, 0) % 256;
      if (sum === checksum) {
        // Unpack de 2 enteros (16 bits) y 2 bytes
        let xRaw = (buffer[0] << 8) | buffer[1];
        let yRaw = (buffer[2] << 8) | buffer[3];

        // Convertir de entero con signo (16 bits)
        if (xRaw > 32767) xRaw -= 65536;
        if (yRaw > 32767) yRaw -= 65536;

        x = xRaw;
        y = yRaw;
        buttonA = buffer[4] === 1;
        buttonB = buffer[5] === 1;
      }
    }
  }
}
```
