### 1. ¿Para qué se usan estas imágenes? ¿Qué representan?
Estas imágenes sirven para cambiar el tipo de pincel; en esencia, representan las distintas formas de las puntas de pincel.

### 2. ¿Qué ocurre al hacer clic en el botón?
Si el puerto está cerrado, al presionar el botón se abrirá. En cambio, si ya está abierto, se cerrará.

### 3. ¿Por qué? ¿Se pueden leer datos con el puerto cerrado? ¿Qué sucede si el micro:bit envía datos con el puerto cerrado?
Solo es posible leer datos del micro:bit cuando el puerto está abierto, ya que es el único canal de comunicación entre el micro:bit y el sketch, salvo que se use otra vía como Bluetooth o radio.

Cuando `port.opened()` es falso (puerto cerrado), el sketch no recibe los datos del micro:bit, ya que es como si este hablara con una puerta cerrada: nadie lo escucha.

Si el micro:bit envía datos con el puerto cerrado, estos se pierden, ya que no hay nadie que los lea o almacene. En cambio, si el puerto está abierto, los datos se reciben y se guardan en la variable `data`.

### 4. ¿Qué pasa si el micro:bit no envía el salto de línea “\n”?
Si no se envía el salto de línea, el comando `readUntil()` no sabe cuándo detenerse. Entonces, seguirá leyendo datos sin control, lo que resultará en una secuencia ininterrumpida de información. Esto afecta el funcionamiento del código, ya que está diseñado para procesar bloques de 4 datos.

### 5. ¿Por qué se suma `windowWidth/2` y `windowHeight/2` a las coordenadas `x` e `y`?
Porque el micro:bit usa como referencia el centro del lienzo (0,0), mientras que en p5.js esa coordenada está en la esquina superior izquierda. Al sumar la mitad del ancho y alto de la ventana, se corrigen las coordenadas para que coincidan con el sistema de p5.js.

### 6. ¿Cómo verificar si se están generando los eventos `keyPressed` y `keyReleased`?
Una forma de comprobarlo es usando `print()` dentro de las funciones. Por ejemplo:

```python
print("Tecla presionada")
print("Tecla soltada")
```

Esto permite ver si efectivamente se ejecutan. Además, si los efectos esperados se ven reflejados en el lienzo, es una señal de que estos eventos se están generando.

### 7. ¿Qué hace el código? ¿Por qué guardar el estado anterior de los botones? ¿Qué pasa si no se hace?
Cuando el botón A del micro:bit pasa de `false` a `true`, se entiende que fue presionado. Esto cambia aleatoriamente el tamaño del trazo y guarda la posición actual en `clickPosX` y `clickPosY`.

Si el botón B cambia de `true` a `false`, se considera que fue soltado, lo que provoca un cambio aleatorio en el color del trazo.

Luego, los estados actuales de los botones se guardan como los nuevos "estados anteriores", para que el sistema detecte correctamente futuros cambios. Si no se hace, el programa no notaría cambios en los botones y no actualizaría ni el tamaño ni el color del trazo.

### 8. ¿Qué diferencias notas?
La principal diferencia es que ahora se incluye el micro:bit en el flujo de trabajo, lo que requiere usar el puerto serial para comunicarse.

### 9. ¿Qué pasó con algunos eventos del mouse?
Muchos eventos que antes dependían del mouse fueron reemplazados por acciones del micro:bit, como el uso de sus botones o la lectura de su posición.

### 10. ¿Qué ocurrió con la función de la barra espaciadora?
Esa función ya no está disponible. Al parecer, fue sustituida por el botón B del micro:bit, que ahora se encarga de cambiar el color del trazo.

---

### “No se están recibiendo 4 datos del micro:bit”

**1. ¿Qué significa este mensaje? ¿Pasa en varios frames o solo en uno?**  
Este mensaje aparece solo una vez, en el primer frame. Indica que el paquete de datos recibido contiene menos de 4 elementos, por lo que no puede ser procesado.

**2. ¿Por qué sucede esto?**  
Esto ocurre porque el código se ejecuta antes de que el micro:bit haya enviado su primer paquete de datos completo. Es decir, en la primera iteración, todavía no se ha producido una interacción con el dispositivo.

**3. ¿Qué se puede hacer para solucionarlo?**  
No es necesario solucionarlo, ya que es normal al inicio del programa. Aun así, como medida preventiva, se podrían inicializar esos 4 datos con valores por defecto, por ejemplo: `(0, 0, false, false)`, para asegurar que haya un paquete válido desde el principio.
