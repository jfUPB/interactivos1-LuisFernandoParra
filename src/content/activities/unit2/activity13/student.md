# Explicación del Código y Posibles Mejoras


## Funcionamiento del Código

### Máquina de Estados

El programa se basa en tres estados principales:

1. **Estado "config":**
   - **Configuración del tiempo:**  
     - Se muestra el valor actual del tiempo en la pantalla utilizando `display.scroll(self.time)`.
     - Al presionar el botón A se incrementa el tiempo.
     - Al presionar el botón B se decrementa el tiempo.
   - **Inicio de la cuenta regresiva:**  
     - Al detectar el gesto de "shake" (agitar), se guarda el tiempo actual (`self.startTime = utime.ticks_ms()`) y se cambia el estado a "LETS GO".

2. **Estado "LETS GO":**
   - **Cuenta regresiva:**  
     - Se verifica si ha pasado 1 segundo usando `utime.ticks_diff(utime.ticks_ms(), self.startTime)`.
     - Cada segundo se decrementa el valor de `self.time` y se muestra en la pantalla.
   - **Detonación:**  
     - Cuando el tiempo llega a 0, se reproduce la melodía `music.WAWAWAWAA` y se cambia el estado a "bombaexplotada".

3. **Estado "bombaexplotada":**
   - **Reinicio:**  
     - Al tocar el logo (detectado con `pin_logo.is_touched()`), se reinicia el temporizador a 20 segundos y se regresa al estado "config".

## Dificultades y Consideraciones

- **Manejo de Eventos:**  
  La detección de múltiples pulsaciones rápidas puede causar incrementos o decrementos inesperados del temporizador.

- **Visualización:**  
  Se utiliza `display.scroll(self.time)` de forma continua en el estado "config", lo que puede saturar la pantalla y distraer al usuario.

- **Validación del Tiempo:**  
  No se ha implementado una verificación para evitar que el valor del temporizador se vuelva negativo.

## Posibles Mejoras

- **Control de Límites:**  
  Añadir una condición para evitar que el tiempo sea negativo:
  ```python
  if button_b.was_pressed() and self.time > 0:
      self.time -= 1
      display.scroll(self.time)
