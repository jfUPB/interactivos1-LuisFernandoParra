# Actividad 7:

---

##  Cambios realizados en mobile/sketch.js

###  ¿Qué datos se envían y cómo?

Ahora se envían tres datos:  
- x y y (posición del toque)  
- color (color actual de la partícula)  
- useStroke (booleano que indica si se debe usar stroke() o no)

###  Fragmentos de código

```javascript
let socket;
let useStroke = true;
let particleColor;

function setup() {
  createCanvas(windowWidth, windowHeight);
  socket = io();
  particleColor = color(random(255), random(255), random(255));

  socket.on('connect', () => {
    console.log("Conectado al servidor");
  });
}

function touchMoved() {
  if (socket && socket.connected) {
    let touchData = {
      type: 'particle',
      x: mouseX,
      y: mouseY,
      color: [red(particleColor), green(particleColor), blue(particleColor)],
      useStroke: useStroke
    };
    socket.emit('message', JSON.stringify(touchData));
  }
  return false;
}

function keyPressed() {
  // Cambia color aleatoriamente y alterna stroke
  particleColor = color(random(255), random(255), random(255));
  useStroke = !useStroke;
}
```
## Cambios realizados endesktop/sketch.js
### ¿Cómo se reciben e interpretan los datos?
para recibir el JSON desdepara extraUsamos socket.on('message', callback)para recibir el JSON desde el móvil, luego usamos JSON.parse()para extraer los valores y crear una nueva partícula con esas propiedades.

## Codigo:
```
let socket;
let particles = [];

function setup() {
  createCanvas(windowWidth, windowHeight);
  background(0);
  socket = io();

  socket.on('connect', () => console.log("Conectado"));

  socket.on('message', (data) => {
    try {
      let parsed = JSON.parse(data);
      if (parsed.type === 'particle') {
        particles.push(new Particle(
          parsed.x, parsed.y, parsed.color, parsed.useStroke
        ));
      }
    } catch (e) {
      console.error("Error al parsear JSON:", e);
    }
  });
}

function draw() {
  background(0, 20);

  for (let i = particles.length - 1; i >= 0; i--) {
    particles[i].update();
    particles[i].display();
    if (particles[i].isDead()) {
      particles.splice(i, 1);
    }
  }
}

class Particle {
  constructor(x, y, rgb = [255, 0, 0], useStroke = true) {
    this.pos = createVector(x, y);
    this.vel = p5.Vector.random2D();
    this.lifespan = 255;
    this.size = random(5, 20);
    this.noiseOffset = random(1000);
    this.color = color(...rgb);
    this.useStroke = useStroke;
  }

  update() {
    let angle = noise(this.pos.x * 0.005, this.pos.y * 0.005, this.noiseOffset) * TWO_PI * 2;
    let force = p5.Vector.fromAngle(angle);
    force.mult(0.5);
    this.vel.add(force);
    this.vel.limit(4);
    this.pos.add(this.vel);
    this.lifespan -= 3;
  }

  display() {
    if (this.useStroke) {
      stroke(255, this.lifespan);
    } else {
      noStroke();
    }
    fill(this.color.levels[0], this.color.levels[1], this.color.levels[2], this.lifespan);
    ellipse(this.pos.x, this.pos.y, this.size);
  }

  isDead() {
    return this.lifespan < 0;
  }
}
```
## no se realizaron cambios en el server.js
