https://openprocessing.org/sketch/create
```
let port;
let microBitConnected = false;
let microBitX = windowWidth / 2;
let microBitY = windowHeight / 2;

function setup() {
  createCanvas(windowWidth, windowHeight);
  background(100);

  port = createSerial();

  let connectBtn = createButton("Connect to micro:bit");
  connectBtn.position(10, 10);
  connectBtn.mousePressed(() => {
    if (!port.opened()) {
      port.open("MicroPython", 115200);
    } else {
      port.close();
    }
  });
}

function draw() {
  background(100);

  if (!port.opened()) {
    microBitConnected = false;
  } else {
    microBitConnected = true;

    if (port.availableBytes() > 0) {
      let data = port.readUntil("\n");
      if (data) {
        let values = data.trim().split(",");
        if (values.length == 4) {
          microBitX = int(values[0]) + width / 2;
          microBitY = int(values[1]) + height / 2;
        } else {
          print("No se están recibiendo 4 datos del micro:bit");
        }
      }
    }
  }

  fill(255);
  circle(microBitX, microBitY, 20);
}
```
https://editor.p5js.org/simonburgosb/sketches/A1IS5yL-5
