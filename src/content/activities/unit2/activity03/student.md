

## Características del Micro:bit – Entradas y Salidas  

| **Tipo**  | **Elemento**       | **Descripción** |
|-----------|-------------------|----------------|
| **Entrada** | **Botón A** | Botón físico en la parte frontal izquierda del Micro:bit que permite detectar presiones. |
| **Entrada** | **Botón B** | Botón físico en la parte frontal derecha del Micro:bit que también detecta presiones. |
| **Entrada** | **Acelerómetro** | Sensor que detecta movimiento e inclinación en los ejes X, Y y Z. |
| **Entrada** | **Sensor de luz** | Mide la cantidad de luz ambiental usando la matriz de LEDs. |
| **Salida** | **Matriz de LEDs (5x5)** | Permite mostrar imágenes, texto y animaciones en una cuadrícula de 5x5 LEDs. |
| **Salida** | **Buzzer (v2)** | Reproduce sonidos y tonos en el Micro:bit v2. |
| **Salida** | **Pines GPIO** | Permiten enviar señales a dispositivos externos, como motores o luces. |
| **Salida** | **Radio/Bluetooth** | Comunicación inalámbrica con otros Micro:bits o dispositivos compatibles. |

---

## Funciones de MicroPython para entradas y salidas  

### **Funciones para entradas**  

1. **`button_a.is_pressed()`** – Detecta si el botón A está presionado.  
```python
from microbit import *

while True:
    if button_a.is_pressed():
        display.show("A")

```
2. **`accelerometer.get_x()`** – Obtiene la inclinación en el eje X..  
```python
from microbit import *

while True:
    x = accelerometer.get_x()
    if x > 200:
        display.show("R")  # Muestra "R" si el Micro:bit se inclina a la derecha

```
3. **`display.read_light_level()`** – Obtiene el nivel de luz ambiental. 
```python
from microbit import *

while True:
    light = display.read_light_level()
    if light < 50:
        display.show("🌙")  # Muestra un icono si hay poca luz
```

### **Funciones para Salidas**  
1. **`display.show()`** – Muestra texto o imágenes en la matriz de LEDs.  
```python
from microbit import *

display.show("Hi")

```
2. **`music.play(music.BA_DING)`** – Reproduce un sonido.  
```python
 from microbit import *
 import music
 music.play(music.BA_DING)

```
3. **`pin0.write_digital(1)`** – Activa una señal digital en un pin GPIO (útil para encender un LED externo). 
```python
from microbit import *

pin0.write_digital(1)  # Enciende el dispositivo conectado al pin 0

```
