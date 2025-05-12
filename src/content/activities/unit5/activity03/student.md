## Actividad 3: ¿Por qué antes necesitábamos delimitadores y ahora ya no?

### Formato de datos anterior 
En la unidad anterior, los datos se enviaban como texto en formato ASCII, por ejemplo: `"{},{},{},{}\n"`. Dado que cada valor podía tener un tamaño variable, era necesario:
- Utilizar comas (`,`) para separar los valores y distinguir dónde empieza y termina cada dato.
- Agregar un salto de línea (`\n`) al final para que el receptor supiera cuándo finalizaba un paquete y comenzaba uno nuevo.

### Formato actual 
Ahora los datos se envían en formato binario con `struct.pack('>2h2B')`, lo que da como resultado paquetes de exactamente 6 bytes. Gracias a este tamaño constante:
- Ya no se requieren delimitadores, porque cada parte del paquete ocupa una cantidad fija de bytes.
- El receptor sabe exactamente cuántos bytes leer y cómo interpretarlos, lo que hace más eficiente y confiable la comunicación.

---

## Cambios en el Envío y Recepción de Datos

### En el micro:bit
- **Antes:** Se enviaban datos como texto con separadores.
- **Ahora:** Se usa formato binario con longitud fija.

### En Processing
- **Antes:** Se leía una línea de texto completa con delimitadores.
- **Ahora:** Se leen exactamente 6 bytes por paquete, lo que permite un procesamiento binario más estructurado.

---

## Problemas observados en la consola

Ejemplos de lecturas:
```
microBitX: 1024 microBitY: -500 microBitAState: true microBitBState: false
microBitX: 32767 microBitY: -1 microBitAState: false microBitBState: true
microBitX: -500 microBitY: 256 microBitAState: false microBitBState: false
```

### Posibles errores
- **Lectura desincronizada:** Si la lectura no empieza justo al inicio de un paquete, se pueden mezclar bytes de distintos paquetes.
- **Pérdida de datos:** Si se pierden bytes durante la transmisión, se genera un paquete incompleto e inválido.
- **Lecturas desfasadas:** Si se añade o pierde un byte, la secuencia se desajusta completamente y los datos pierden sentido.

---

## Cambios en el Código

### micro:bit
- **Acelerómetro:** Se reemplazaron valores fijos por `accelerometer.get_x()` y `accelerometer.get_y()`, haciendo que el dibujo en p5.js responda a la inclinación.
- **Botones:** Se reemplazaron valores fijos de `aState` y `bState` por `button_a.is_pressed()` y `button_b.is_pressed()`.

### p5.js
- **Limpieza de consola:** Se eliminó `console.log()` de la función `readSerialData()` para reducir el ruido en la consola.

### Salida esperada
```
Microbit ready to draw
A pressed
B released
Checksum error in packet
```
