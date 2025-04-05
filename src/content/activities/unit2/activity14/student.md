# Documentación del Proceso de Prueba y Código Final Corregido

## Introducción

Este documento presenta la metodología de pruebas exhaustivas aplicada al código de la "bomba" para micro:bit. Se han definido varios vectores de prueba para evaluar el comportamiento del programa en diferentes situaciones, se han identificado errores y se han implementado las correcciones necesarias para mejorar la robustez y la experiencia de usuario.

## Casos de Prueba

A continuación se describen 5 vectores de prueba, sus acciones, resultados esperados y observaciones:

1. **Caso de Prueba 1: Configuración Inicial**  
   - **Acción:**  
     - Iniciar el programa con el temporizador en 20 segundos.  
   - **Resultado Esperado:**  
     - La pantalla muestra "20" y el estado inicial es `config`.  
   - **Observación:**  
     - Se verifica que el estado inicial y el valor del temporizador sean correctos.

2. **Caso de Prueba 2: Incremento del Temporizador (Botón A)**  
   - **Acción:**  
     - Presionar el botón A para aumentar el tiempo.  
   - **Resultado Esperado:**  
     - El valor del temporizador incrementa en 1 (ej. de 20 a 21) y se actualiza la visualización.  
   - **Error Detectado:**  
     - La visualización se actualizaba de forma excesiva, saturando la pantalla.  
   - **Corrección:**  
     - Se implementó un mecanismo que actualiza la pantalla únicamente cuando el valor cambia.

3. **Caso de Prueba 3: Decremento del Temporizador (Botón B)**  
   - **Acción:**  
     - Presionar el botón B para disminuir el tiempo.  
   - **Resultado Esperado:**  
     - El valor del temporizador disminuye en 1 (ej. de 20 a 19) y se actualiza la visualización.  
   - **Error Detectado:**  
     - No se controlaba que el valor no bajara de 0, pudiendo generar números negativos.  
   - **Corrección:**  
     - Se agregó una condición para evitar que el tiempo se decremente por debajo de 0.

4. **Caso de Prueba 4: Inicio de la Cuenta Regresiva (Gesto "shake")**  
   - **Acción:**  
     - Agitar el micro:bit para iniciar la cuenta regresiva.  
   - **Resultado Esperado:**  
     - El estado cambia a `LETS GO` y el temporizador comienza a decrementar en 1 cada segundo, actualizando la visualización.  
   - **Error Detectado:**  
     - La pantalla se actualizaba de forma repetitiva en cada iteración, lo que podía distraer al usuario.  
   - **Corrección:**  
     - Se optimizó la actualización de la visualización para que se realice solo cuando hay un cambio significativo.

5. **Caso de Prueba 5: Detonación y Reinicio (Logo Touch)**  
   - **Acción:**  
     - Permitir que la cuenta regresiva llegue a 0 y, tras escuchar el sonido de detonación, tocar el logo para reiniciar la bomba.  
   - **Resultado Esperado:**  
     - Al llegar a 0, se reproduce `music.WAWAWAWAA` y el estado cambia a `bombaexplotada`.  
     - Al tocar el logo, el temporizador se reinicia a 20 y el estado vuelve a `config`.  
   - **Observación:**  
     - Se comprueba que el reinicio funcione correctamente y se muestra un ícono (por ejemplo, una calavera) para indicar que la bomba explotó antes de reiniciar.

## Descripción de Errores y Correcciones Implementadas

- **Error 1: Decremento sin Validación**  
  - *Problema:* El valor del tiempo podía volverse negativo al presionar el botón B sin restricciones.  
  - *Corrección:* Se añadió una condición para asegurarse de que el tiempo sea mayor que 0 antes de decrementar.

- **Error 2: Visualización Excesiva**  
  - *Problema:* Se utilizaba `display.scroll(self.time)` de forma continua, saturando la pantalla y haciendo difícil la lectura.  
  - *Corrección:* Se introdujo una variable de control (`last_displayed_time`) para actualizar la pantalla únicamente cuando el valor del tiempo cambia.

- **Error 3: Falta de Debouncing para los Botones**  
  - *Problema:* Las pulsaciones rápidas de los botones podían provocar múltiples incrementos o decrementos no deseados.  
  - *Corrección:* Se implementó un retardo corto (`utime.sleep_ms(200)`) después de detectar la pulsación de cada botón para evitar lecturas múltiples.

## Código Final Corregido

```python
from microbit import *
import utime
import music

class Bomba:
    def __init__(self, tiempo_inicial):
        self.state = "config"
        self.time = tiempo_inicial
        self.startTime = 0
        self.last_displayed_time = None  # Para evitar actualizaciones innecesarias

    def update(self):
        if self.state == "config":
            # Actualiza la visualización solo si el tiempo ha cambiado
            if self.last_displayed_time != self.time:
                display.clear()
                display.scroll(str(self.time))
                self.last_displayed_time = self.time

            if button_a.was_pressed():
                self.time += 1
                self.last_displayed_time = None  # Forzar actualización
                utime.sleep_ms(200)  # Debouncing
            if button_b.was_pressed():
                if self.time > 0:
                    self.time -= 1
                    self.last_displayed_time = None  # Forzar actualización
                utime.sleep_ms(200)  # Debouncing

            if accelerometer.was_gesture('shake'):
                self.startTime = utime.ticks_ms()
                self.state = "LETS GO"
                display.clear()

        elif self.state == "LETS GO":
            if utime.ticks_diff(utime.ticks_ms(), self.startTime) > 1000:
                self.startTime = utime.ticks_ms()
                self.time -= 1
                display.clear()
                display.scroll(str(self.time))
                if self.time == 0:
                    music.play(music.WAWAWAWAA)
                    self.state = "bombaexplotada"
                    display.clear()
                utime.sleep_ms(200)  # Debouncing para evitar actualizaciones demasiado rápidas

        elif self.state == "bombaexplotada":
            # Mostrar imagen de bomba explotada
            display.show(Image.SKULL)
            if pin_logo.is_touched():
                self.time = 20
                self.last_displayed_time = None
                self.state = "config"
                display.clear()
                utime.sleep_ms(200)  # Debouncing

# Instanciar la bomba con un tiempo inicial de 20 segundos
bomba1 = Bomba(20)

while True:
    bomba1.update()
