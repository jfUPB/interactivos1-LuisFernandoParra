# Actividad 1  
## Interacción entre micro:bit y p5.js

La comunicación entre el micro:bit y un sketch de p5.js se realiza mediante una conexión serial utilizando la biblioteca `p5.webserial.js`. A través de `uart.write()`, el micro:bit envía datos del acelerómetro y el estado de sus botones, los cuales son leídos en p5.js y empleados para representar elementos visuales en pantalla.

---

## Datos enviados desde el micro:bit

El micro:bit transmite una línea de texto por UART que incluye los siguientes valores, separados por comas:

- **xValue**: valor del eje X del acelerómetro.  
- **yValue**: valor del eje Y del acelerómetro.  
- **aState**: indica si el botón A está presionado (`true`) o no (`false`).  
- **bState**: indica si el botón B está presionado (`true`) o no (`false`).  

---

## Estructura del protocolo ASCII

El protocolo utilizado es bastante básico: los datos se separan con comas y finalizan con un salto de línea (`\n`). Esto simplifica la interpretación en p5.js, ya que se puede usar `split(",")` para separar los valores, y luego convertirlos en números o valores booleanos según sea necesario.

**Formato estándar del mensaje:**  
``<xValue>,<yValue>,<aState>,<bState>\n``

En este formato:  
- *xValue* y *yValue* son números enteros (del acelerómetro).  
- *aState* y *bState* son booleanos (`true` o `false`).  

---

## Lectura e interpretación de los datos en p5.js

La recepción de los datos se realiza dentro de la función `draw()` de p5.js. El proceso es el siguiente:

1. Verificar si hay datos disponibles mediante `port.availableBytes()`.  
2. Leer una línea completa con `port.readUntil("\n")`.  
3. Eliminar espacios en blanco con `trim()`.  
4. Dividir la línea en partes con `split(",")`.  
5. Convertir los valores al tipo adecuado:  
   - *xValue* y *yValue* a enteros.  
   - *aState* y *bState* a booleanos.  
6. Ajustar los valores del acelerómetro para ubicarlos en la pantalla usando `windowWidth` y `windowHeight`.  

### Fragmento de código dentro de draw():
```javascript
if (port.availableBytes() > 0) {
  let data = port.readUntil("\n");
  if (data) {
    data = data.trim();
    let values = data.split(",");
    if (values.length == 4) {
      microBitX = int(values[0]) + windowWidth / 2;
      microBitY = int(values[1]) + windowHeight / 2;
      microBitAState = values[2].toLowerCase() === "true";
      microBitBState = values[3].toLowerCase() === "true";
      updateButtonStates(microBitAState, microBitBState);
    } else {
      print("No se están recibiendo 4 datos del micro:bit");
    }
  }
}
```

Los valores `microBitX` y `microBitY` se emplean para ubicar elementos visuales según la posición detectada por el acelerómetro.

---

## Manejo de eventos: “A presionado” y “B soltado” en p5.js

Los eventos se identifican al comparar el estado actual de cada botón con su estado anterior. Esto permite detectar:

- **“A presionado”**: cuando *microBitAState* cambia de `false` a `true`, lo que indica que el botón A fue presionado recientemente. En ese momento:
  - Se genera un nuevo tamaño aleatorio para el dibujo.
  - Se almacenan las coordenadas actuales del micro:bit.

- **“B soltado”**: cuando *microBitBState* cambia de `true` a `false`, indicando que el botón B fue liberado. En este caso:
  - Se genera un nuevo color aleatorio para el siguiente trazo.

### Código en `updateButtonStates()`:
```javascript
function updateButtonStates(newAState, newBState) {
  if (newAState === true && prevmicroBitAState === false) {
    lineModuleSize = random(50, 160);
    clickPosX = microBitX;
    clickPosY = microBitY;
    print("A pressed");
  }

  if (newBState === false && prevmicroBitBState === true) {
    c = color(random(255), random(255), random(255), random(80, 100));
    print("B released");
  }

  prevmicroBitAState = newAState;
  prevmicroBitBState = newBState;
}
```

Gracias a este mecanismo, los eventos se activan solo cuando realmente hay un cambio en el estado de los botones, evitando repeticiones innecesarias.
