``` 
let secuencia = ["A", "B", "A", "SHAKE"];
let secuenciaUsuario = [];
let letra = "";
let bomba1;

function setup() {
  createCanvas(400, 200);
  textAlign(CENTER, CENTER);
  textSize(32);
  bomba1 = new Bomba(20);

  // Botones en pantalla
  createButton('A').position(20, height + 10).mousePressed(() => letra = "A");
  createButton('B').position(70, height + 10).mousePressed(() => letra = "B");
  createButton('Shake').position(120, height + 10).mousePressed(() => letra = "S");
  createButton('Reset').position(190, height + 10).mousePressed(() => letra = "T");
}

function draw() {
  background(220);
  bomba1.update();
  text("Tiempo: " + bomba1.time, width / 2, height / 2);
  if (bomba1.state === "bombaexplotada") {
    text("💥 ¡BOMBA!", width / 2, height / 2 + 50);
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
      if (letra === "A") {
        this.time += 1;
      } else if (letra === "B") {
        this.time -= 1;
      } else if (letra === "S") {
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
