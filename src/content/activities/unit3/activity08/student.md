## Codigo P5
```
let secuencia = ["A", "B", "A", "SHAKE"];
let secuenciaUsuario = [];
let letra = "";
let bomba1;

let serial;
let puertoAbierto = false;

function setup() {
  createCanvas(400, 200);
  textAlign(CENTER, CENTER);
  textSize(32);
  bomba1 = new Bomba(20);

  // Botones
  createButton('A').position(20, height + 10).mousePressed(() => letra = "A");
  createButton('B').position(70, height + 10).mousePressed(() => letra = "B");
  createButton('Shake').position(120, height + 10).mousePressed(() => letra = "S");
  createButton('Reset').position(190, height + 10).mousePressed(() => letra = "T");

  // Serial
  serial = new p5.SerialPort();
  serial.list(); // lista de puertos disponibles
  serial.openPrompt(); // abrir puerto desde navegador
  serial.on("data", recibirSerial);
}

function draw() {
  background(220);
  bomba1.update();
  text("Tiempo: " + bomba1.time, width / 2, height / 2);
  if (bomba1.state === "bombaexplotada") {
    text("💥 ¡BOMBA!", width / 2, height / 2 + 50);
  }
}

function recibirSerial() {
  let dato = serial.readLine().trim();
  if (["A", "B", "S", "T"].includes(dato)) {
    letra = dato;
  }
}

class Bomba {
  constructor(time) {
    this.state = "config";
    this.time = time;
    this.startTime = 0;
    this.lastTick = millis();
  }

  update() {
    if (this.state === "config") {
      if (letra === "A") this.time += 1;
      else if (letra === "B") this.time -= 1;
      else if (letra === "S") {
        this.startTime = millis();
        this.lastTick = millis();
        this.state = "LETS GO";
      }

    } else if (this.state === "LETS GO") {
      if (millis() - this.lastTick > 1000) {
        this.lastTick = millis();
        this.time -= 1;

        if (letra === "A") secuenciaUsuario.push("A");
        else if (letra === "B") secuenciaUsuario.push("B");
        else if (letra === "S") secuenciaUsuario.push("SHAKE");

        if (secuenciaUsuario.length >= secuencia.length) {
          if (JSON.stringify(secuenciaUsuario) === JSON.stringify(secuencia)) {
            this.state = "config";
            secuenciaUsuario = [];
          } else {
            secuenciaUsuario = [];
          }
        }

        if (this.time <= 0) {
          console.log("WAWAWAWAA!");
          this.state = "bombaexplotada";
        }
      }

    } else if (this.state === "bombaexplotada") {
      if (letra === "T") {
        this.time = 20;
        this.state = "config";
      }
    }

    letra = "";
  }
}
```
codigo microbit
```
from microbit import *
import utime

while True:
    if button_a.was_pressed():
        uart.write("A\n")
    if button_b.was_pressed():
        uart.write("B\n")
    if accelerometer.was_gesture('shake'):
        uart.write("S\n")
    if pin_logo.is_touched():
        uart.write("T\n")
    utime.sleep_ms(100)
```
