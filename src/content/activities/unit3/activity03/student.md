```
from microbit import *
import utime
import music
secuencia = ["A","B","A","SHAKE"]
secuenciaUsuario = []
letra = "c"
class bomba:
     def __init__(self,time):
         self.state = "config"
         self.time = time
         self.startTime = 0

     def update(self):
         global letra
         if self.state == "config":
             if True:
                 display.scroll(self.time)
             if letra == "A":
                 self.time = self.time +1
                 display.scroll(self.time)
                 

             if letra == "B":
                 self.time = self.time -1
                 display.scroll(self.time)

             if letra == "S":
                 self.startTime = utime.ticks_ms()
                 self.state = "LETS GO"
              

         elif self.state == "LETS GO":
            if utime.ticks_diff(utime.ticks_ms(),self.startTime) > 1000:
                self.startTime = utime.ticks_ms()
                self.time -=1
                display.scroll(self.time)
                if letra == "A":
                    secuenciaUsuario.append("A")
                if  letra == "B":
                    secuenciaUsuario.append("B")
                if letra == "s":
                    secuenciaUsuario.append("SHAKE")
                if len(secuenciaUsuario) >= len(secuencia): 
                    if secuenciaUsuario == secuencia:
                        self.state = "config"
                    if secuenciaUsuario != secuencia:
                        secuenciaUsuario.clear()
                if self.time == 0:
                    music.play(music.WAWAWAWAA)
                    self.state = "bombaexplotada"

         elif self.state == "bombaexplotada":
                if letra == "T":
                    self.time = 20
                    self.state = "config"
         letra = ""
def tareaEventos():
    global letra
    if uart.any():
        data = uart.read(1)
        if data:
            letra = data[0]
    if button_a.was_pressed():
        letra = "A"
    if button_b.was_pressed():
        letra = "B"
    if pin_logo.is_touched():
        letra = "T"
    if accelerometer.was_gesture('shake'):
        letra = "S"
    


bomba1 = bomba(20)

while True:
    bomba1.update()
    tareaEventos()
```
