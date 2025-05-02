#  Caso de Estudio: Comunicación Serial entre micro:bit y p5.js

##  Objetivo

Analizar el código del micro:bit que permite transmitir datos mediante UART y observar cómo se pueden recibir e interpretar desde otra aplicación como `p5.js` o un `SerialTerminal`.

---

##  Análisis del Código

```python
from microbit import *

uart.init(115200)
display.set_pixel(0,0,9)

while True:
    xValue = accelerometer.get_x()
    yValue = accelerometer.get_y()
    aState = button_a.is_pressed()
    bState = button_b.is_pressed()
    data = "{},{},{},{}\n".format(xValue, yValue, aState, bState)
    uart.write(data)
    sleep(100)
```

---

##  ¿Qué información se está enviando?

- `xValue`: Lectura del acelerómetro en el eje X.
- `yValue`: Lectura del acelerómetro en el eje Y.
- `aState`: Estado del botón A (`True` o `False`).
- `bState`: Estado del botón B (`True` o `False`).

---

##  ¿Cómo se está enviando?

- A través de la interfaz serial UART con velocidad de 115200 baudios.
- En formato **CSV** (valores separados por comas), terminando cada línea con `\n` (salto de línea).
- Ejemplo de una línea de datos enviada:

  ```
  215,-110,True,False
  ```

---

##  Explicación del Formato

```python
"{},{},{},{}\n".format(xValue, yValue, aState, bState)
```

- Usa `format()` para insertar los valores en una cadena.
- Las comas separan los campos para facilitar el análisis posterior.
- `\n` permite que el receptor sepa cuándo termina un conjunto de datos.

---

##  Observaciones en SerialTerminal

### Estructura vista:

```
300,420,False,True
295,419,False,False
```

### Inferencias:

- Cada línea representa un "paquete" de datos.
- Los datos separados por comas facilitan el uso de funciones como `.split(',')` para descomponerlos.

---

##  ¿Qué pasaría si no se separan con comas o no se usa `\n`?

| Situación                     | Consecuencia                                              |
|------------------------------|-----------------------------------------------------------|
| Sin comas                    | Dificultad para separar datos (todo se ve como un número) |
| Sin salto de línea (`\n`)    | El receptor no sabría cuándo termina un conjunto de datos |

---

##  ¿Para qué se usa `sleep(100)`?

- Establece una **frecuencia de envío** de 10 veces por segundo (10 Hz).
- Evita **saturar el canal de comunicación**.
- Si se elimina, los datos llegarán demasiado rápido y podrían no ser interpretados correctamente por el receptor.

---

##  Comportamiento del acelerómetro

| Movimiento         | xValue       | yValue       |
|--------------------|--------------|--------------|
| Inclinación izquierda | Valor negativo | ~estable     |
| Inclinación derecha  | Valor positivo | ~estable     |
| Adelante            | ~estable     | Valor positivo |
| Atrás               | ~estable     | Valor negativo |

*Valores típicos entre -1024 y 1024.*

---

##  Comportamiento de botones

- `button_a.is_pressed()` → `True` si está presionado, `False` si no.
- `button_b.is_pressed()` → igual que el botón A.

---

##  Comparación entre `is_pressed()` y `was_pressed()`

| Función        | Comportamiento                                                        |
|----------------|------------------------------------------------------------------------|
| `is_pressed()` | Devuelve `True` si el botón está presionado **en ese instante**.     |
| `was_pressed()`| Devuelve `True` **una vez**, después de haber sido presionado.        |

---

##  Análisis de Bytes Enviados (Ejemplo)

### Supuestos:

```python
xValue = 969
yValue = 652
aState = True
bState = False
```

Cadena enviada:

```
969,652,True,False\n
```

### Representación en HEX:

| Texto    | ASCII Bytes (HEX)                     |
|----------|----------------------------------------|
| `969`    | `39 36 39`                             |
| `,`      | `2C`                                   |
| `652`    | `36 35 32`                             |
| `,`      | `2C`                                   |
| `True`   | `54 72 75 65`                          |
| `,`      | `2C`                                   |
| `False`  | `46 61 6C 73 65`                       |
| `\n`     | `0A`                                   |

**Cadena total en HEX:**

```
39 36 39 2C 36 35 32 2C 54 72 75 65 2C 46 61 6C 73 65 0A
```

---

## ✅ Conclusiones

- El uso de UART con formato CSV permite una comunicación eficiente y legible.
- `sleep(100)` regula la frecuencia y evita errores en la recepción.
- La estructura con comas y saltos de línea permite descomponer fácilmente los datos.
- Los botones y acelerómetro permiten controlar interactivamente otros sistemas, como visualizaciones en p5.js.
