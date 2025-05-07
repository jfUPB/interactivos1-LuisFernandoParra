#  Bitácora - Actividad 06: Consolidación y Metacognición 🤔

## 1. ¿Qué es un protocolo de comunicación y por qué es importante en la comunicación serial?

Un **protocolo de comunicación** define las reglas y estructura para intercambiar datos entre dispositivos. En comunicación serial, es fundamental porque permite que ambos extremos (por ejemplo, un micro:bit y una computadora) entiendan y procesen correctamente los datos enviados y recibidos.

###  Fragmento de código:
```python
serial.writeString(x + "," + y + "," + aState + "," + bState + "\n")
```
Este formato estructurado es parte del protocolo: datos separados por comas, terminados con salto de línea.

---

## 2. ¿Por qué se separan los datos con comas en el protocolo ASCII que exploramos?

Separar los datos con **comas** permite segmentar la información fácilmente en el receptor. Es una forma simple de crear una estructura de mensaje tipo CSV que puede ser procesada con funciones como `split()`.

###  Fragmento de código:
```javascript
let values = data.split(",");
```

---

## 3. ¿Por qué es necesario terminar los datos con un carácter que marque el fin del mensaje?

El carácter de fin de mensaje, como `\n`, le indica al receptor cuándo ha terminado un paquete de datos. Sin él, no se puede saber si el mensaje está completo o si se deben esperar más bytes.

###  Fragmento de código:
```javascript
let data = port.readUntil("\n");
```

---

## 4. ¿Por qué fue necesario usar una máquina de estados en la aplicación modificada de p5.js?

La **máquina de estados** ayuda a controlar el flujo de la aplicación según los datos recibidos, por ejemplo, cambiar la animación o la lógica según el botón `A` o `B` del micro:bit.

###  Fragmento de código:
```javascript
if (aState == 1) {
  currentState = "MOVING";
} else if (bState == 1) {
  currentState = "JUMPING";
}
```

---

## 5. ¿Cómo se formatean los datos en el micro:bit para ser enviados por el puerto serial?

Los datos se formatean como una cadena de texto ASCII con campos separados por comas y terminados en salto de línea.

###  Fragmento de código en MakeCode:
```python
serial.writeLine(x + "," + y + "," + aState + "," + bState)
```

---

## 6. ¿Qué significa que los datos enviados por el micro:bit están codificados en ASCII?

Significa que los datos se envían como caracteres legibles (texto), no como binarios. Cada número o letra se convierte en un byte que representa un símbolo ASCII.

---

## 7. ¿Por qué es necesario en la aplicación de p5.js preguntar si hay bytes disponibles en el puerto serial antes de leerlos?

Porque si no hay datos disponibles, `readUntil()` podría fallar o devolver un valor incompleto. Se necesita asegurar que el mensaje esté listo para ser leído.

###  Fragmento de código:
```javascript
if (port.availableBytes() > 0) {
  let data = port.readUntil("\n");
}
```

###  ¿Qué pasa si no se hace?
Podríamos leer datos vacíos o incompletos, lo cual causaría errores al convertir o dividir la cadena.

---

## 8. ¿Cómo se elimina el retorno de carro o salto de línea de un string en p5.js?

Usando la función `trim()`, que elimina espacios, saltos de línea y otros caracteres no imprimibles.

###  Fragmento de código:
```javascript
data = trim(data);
```

---

## 9. Si una cadena tiene información separada por espacios y quieres dividir dicha información en varias cadenas individuales ¿Qué función de p5.js usarías?

La función `split()`.

###  Ejemplo:
```javascript
let parts = split("123 456 789", " ");
```

---

## 10. ¿Por qué es necesario en la aplicación del caso de estudio convertir las cadenas a números enteros antes de usarlas en el sketch de p5.js?

Porque los datos se reciben como texto (string) y para operar con ellos (dibujar, mover, comparar), se necesitan como números.

###  Fragmento de código:
```javascript
let x = int(values[0]);
let y = int(values[1]);
```

---

## 11. Si el micro:bit tiene los siguientes datos: xValue: 123, yValue: 756, aState: False, bState: True. ¿Qué bytes se enviarían por el puerto serial?

Se enviaría la siguiente cadena en ASCII:
```
"123,756,0,1\n"
```
Cada carácter de la cadena es un byte ASCII. El "0" y "1" representan los estados
