```
rom microbit import *
import utime

class Semaforo:
    def __init__(self,xRed,yRed,intervalRed,intervalYellow,intervalGreen):
        self.state = "Init"
        self.startTime = 0
        self.redX = xRed
        self.redY = yRed
        self.yellowX = self.redX 
        self.yellowY = self.redY + 1
        self.greenX = self.redX 
        self.greenY = self.redY + 2
        self.redInterval = intervalRed
        self.yellowInterval = intervalYellow
        self.greenInterval = intervalGreen

    def update(self):

        if self.state == "Init":
            self.startTime = utime.ticks_ms()
            display.set_pixel(self.redX,self.redY,9)
            self.state = "WaitTimeoutRed"


        elif self.state == "WaitTimeoutRed":
            if utime.ticks_diff(utime.ticks_ms(),self.startTime) > self.redInterval:
                 display.set_pixel(self.redX,self.redY,0)
                 self.startTime = utime.ticks_ms()
                 display.set_pixel(self.yellowX,self.yellowY,9)
                 self.state = "LetsGOGreen"



        elif self.state == "LetsGOGreen":
            if utime.ticks_diff(utime.ticks_ms(),self.startTime) > self.yellowInterval:
                 display.set_pixel(self.yellowX,self.yellowY,0)
                 self.startTime = utime.ticks_ms()
                 display.set_pixel(self.greenX,self.greenY,9)
                 self.state = "LetsGORed"


        elif self.state == "LetsGORed":
            if utime.ticks_diff(utime.ticks_ms(),self.startTime) > self.yellowInterval:
                 display.set_pixel(self.greenX,self.greenY,0)
                 self.startTime = utime.ticks_ms()
                 display.set_pixel(self.redX,self.redY,9)
                 self.state = "WaitTimeoutRed"






semaforo = Semaforo(0,0,5000,2000,3000)
semaforo2 = Semaforo(1,0,3000,1000,2000)
semaforo3 = Semaforo(2,0,4000,3000,2000)

while True:
    semaforo.update()
    semaforo2.update()
    semaforo3.update()

```
