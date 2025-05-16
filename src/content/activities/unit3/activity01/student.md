### Mi solucion a la actividad 1

Codigo:

``` js
from microbit import *
import utime

class Semaforo:
    def __init__(self, rojo, amarillo, verde):
        self.state = "RED"
        self.startTime = utime.ticks_ms()
        self.intervals = {
            "RED": rojo,    
            "GREEN": verde,  
            "YELLOW": amarillo  
        }

    def update(self):
        current_time = utime.ticks_ms()
      
        if utime.ticks_diff(current_time, self.startTime) >= self.intervals[self.state]:
        
            if self.state == "RED":
                self.state = "GREEN"
            elif self.state == "GREEN":
                self.state = "YELLOW"
            elif self.state == "YELLOW":
                self.state = "RED"
          
            self.startTime = current_time
            self.display_state()

    def display_state(self):
        display.clear()
     
        if self.state == "RED":
            display.set_pixel(0, 0, 9) 
        elif self.state == "YELLOW":
            display.set_pixel(2, 2, 9)  
        elif self.state == "GREEN":
            display.set_pixel(4, 4, 9)  



semaforo1 = Semaforo(5000, 2000, 3000)
semaforo2 = Semaforo(3000, 1000, 2000)
semaforo3 = Semaforo(4000, 3000, 2000)


semaforo1.display_state()
semaforo2.display_state()
semaforo3.display_state()

while True:
    semaforo1.update()
    semaforo2.update()
    semaforo3.update()
```

  - Reflexion: El uso de clases en este proyecto permite encapsular tanto los datos (como la columna asignada y los tiempos de cada estado) como los comportamientos (actualización y visualización del semáforo) en una única entidad, facilitando la reutilización y escalabilidad del código; además,
  - Las propiedades y métodos del semáforo están encapsulados dentro de la clase. Esto significa que la lógica de cómo se maneja el semáforo no se dispersa por todo el código, sino que está contenida en una estructura bien definida, lo que hace que el código sea más limpio y fácil de mantener.
    la implementación de máquinas de estado garantiza transiciones claras y controladas entre los diferentes estados, lo que simplifica el manejo concurrente de varios semáforos en un solo display.
