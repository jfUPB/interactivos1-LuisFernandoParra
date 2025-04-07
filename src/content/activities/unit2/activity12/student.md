```
from microbit import *
import utime
import music

class bomba:
     def __init__(self,time):
         self.state = "config"
         self.time = time
         self.startTime = 0


     def update(self):
         if self.state == "config":
             if True:
                 display.scroll(self.time)
             if button_a.was_pressed():
                 self.time = self.time +1
                 display.scroll(self.time)
                 
             if button_b.was_pressed():
                 self.time = self.time -1
                 display.scroll(self.time)
                 
             if accelerometer.was_gesture('shake'):
                 
                 self.startTime = utime.ticks_ms()
                 self.state = "LETS GO"

         elif self.state == "LETS GO":
            if utime.ticks_diff(utime.ticks_ms(),self.startTime) > 1000:
                self.startTime = utime.ticks_ms()
                self.time -=1
                display.scroll(self.time)
                if self.time == 0:
                    music.play(music.WAWAWAWAA)
                    self.state = "bombaexplotada"
         elif self.state == "bombaexplotada":
                if pin_logo.is_touched():
                    self.time = 20
                    self.state = "config"
                     
                             
                            
                     
bomba1 = bomba(20)

while True:
    bomba1.update()
```
