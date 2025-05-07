# Explicación del Sketch

**Sketch original:** [P_2_1_2_04](http://www.generative-gestaltung.de/2/sketches/?01_P/P_2_1_2_04)

---

## 1. Declaración de variables globales

```javascript
var tileCount = 20;
var actRandomSeed = 0;
var rectSize = 30;
```

En esta sección se definen las variables globales que controlan la lógica del sketch:

- `tileCount`: establece la cantidad de celdas por fila y columna. En este caso, se crean 20 filas y 20 columnas.
- `actRandomSeed`: es la semilla usada para generar patrones seudoaleatorios. Al dejarla en 0, los resultados se mantienen constantes hasta que se cambie.
- `rectSize`: define el tamaño (largo del lado) de cada rectángulo de la cuadrícula, que es de 30 píxeles.

---

## 2. Configuración inicial (`setup`)

```javascript
function setup() {
  createCanvas(600, 600);
  colorMode(HSB, 360, 100, 100, 100);
  noStroke();
  fill(192, 100, 64, 60);
}
```

La función `setup()` se ejecuta una sola vez al principio del sketch y configura el entorno:

- `createCanvas(600, 600)`: crea un lienzo de 600x600 píxeles, que acomoda perfectamente una cuadrícula de 20x20 celdas de 30 píxeles cada una.
- `colorMode(HSB, 360, 100, 100, 100)`: activa el modo de color HSB (Hue, Saturation, Brightness), útil para controlar colores de forma más intuitiva.
- `noStroke()`: desactiva los contornos de las formas.
- `fill(...)`: define el color de relleno de los rectángulos, incluyendo un nivel de transparencia (valor alfa = 60).

---

## 3. Lógica del dibujo (`draw`)

```javascript
function draw() {
  clear();
  randomSeed(actRandomSeed);

  for (var gridY = 0; gridY < tileCount; gridY++) {
    for (var gridX = 0; gridX < tileCount; gridX++) {
      var posX = width / tileCount * gridX;
      var posY = height / tileCount * gridY;

      var shiftX1 = mouseX / 20 * random(-1, 1);
      var shiftY1 = mouseY / 20 * random(-1, 1);
      var shiftX2 = mouseX / 20 * random(-1, 1);
      var shiftY2 = mouseY / 20 * random(-1, 1);
      var shiftX3 = mouseX / 20 * random(-1, 1);
      var shiftY3 = mouseY / 20 * random(-1, 1);
      var shiftX4 = mouseX / 20 * random(-1, 1);
      var shiftY4 = mouseY / 20 * random(-1, 1);

      push();
      translate(posX, posY);
      beginShape();
      vertex(shiftX1, shiftY1);
      vertex(rectSize + shiftX2, shiftY2);
      vertex(rectSize + shiftX3, rectSize + shiftY3);
      vertex(shiftX4, rectSize + shiftY4);
      endShape();
      pop();
    }
  }
}
```

Esta función se ejecuta en bucle, constantemente redibujando el lienzo:

- `clear()`: limpia el lienzo en cada frame para evitar superposiciones.
- `randomSeed(actRandomSeed)`: fija la semilla para que el patrón aleatorio sea reproducible.

Luego, se utilizan dos bucles `for` anidados para recorrer la cuadrícula:
- El bucle externo recorre las filas (`gridY`).
- El bucle interno recorre las columnas (`gridX`).

Para cada celda, se calculan:
- La posición (`posX`, `posY`) donde se ubicará el rectángulo.
- Valores aleatorios de desplazamiento (`shiftX`, `shiftY`) para cada vértice, basados en la posición del mouse.

Finalmente:
- `translate(posX, posY)`: traslada el origen de coordenadas a la posición correspondiente de la celda.
- `beginShape()` y `vertex(...)`: definen los 4 vértices del cuadrado con sus desplazamientos.
- `push()` y `pop()`: mantienen las transformaciones localizadas a cada celda.

---

## 4. Interacción con el mouse (`mousePressed`)

```javascript
function mousePressed() {
  actRandomSeed = random(100000);
}
```

Cuando se hace clic, se cambia aleatoriamente la semilla (`actRandomSeed`), lo que genera un nuevo patrón visual. Esto permite que el sketch produzca diferentes resultados cada vez que se presiona el mouse.

---

## 5. Guardar imagen (`keyReleased`)

```javascript
function keyReleased() {
  if (key == 's' || key == 'S') saveCanvas(gd.timestamp(), 'png');
}
```

Si se presiona la tecla `S`, el canvas actual se guarda como un archivo `.png`, con un nombre que incluye una marca de tiempo. Esto permite capturar fácilmente distintas versiones del arte generativo.

---
