### Mi solución a la actividad 9

``` js
from microbit import *
import utime

class Semaforo:
    def __init__(self):

        self.state = "RED"
        self.startTime = utime.ticks_ms()
        self.intervals = {
            "RED": 3000,    
            "GREEN": 3000,  
            "YELLOW": 1000  
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
            display.set_pixel(2, 0, 9)     
        elif self.state == "YELLOW":
            display.set_pixel(2, 2, 9)  
        elif self.state == "GREEN":
            display.set_pixel(2, 4, 9)    


semaforo = Semaforo()

semaforo.display_state()

while True:
    semaforo.update()

```

- Estados
"RED" (Rojo):

Descripción: El semáforo está en rojo.
Acción en visualización: Se enciende el LED en la posición (2, 0) para representar el color rojo.
Duración: 3000 ms.
"GREEN" (Verde):

Descripción: El semáforo está en verde.
Acción en visualización: Se enciende el LED en la posición (2, 4) para representar el color verde.
Duración: 3000 ms.
"YELLOW" (Amarillo):

Descripción: El semáforo está en amarillo.
Acción en visualización: Se enciende el LED en la posición (2, 2) para representar el color amarillo.
Duración: 1000 ms.

- Eventos
  
Evento Principal – Timeout (paso del tiempo):
En cada estado, el semáforo espera que transcurra un tiempo específico (definido en el diccionario self.intervals). Este evento se evalúa en el método update con la condición:
python
Copiar
Editar
if utime.ticks_diff(current_time, self.startTime) >= self.intervals[self.state]:
Cuando esta condición se cumple, se reconoce que el tiempo asignado al estado actual ha expirado, lo que dispara el evento de "timeout".

- Acciones
Acciones en respuesta al evento de Timeout:
Cambio de Estado:

Si el semáforo está en "RED", pasa a "GREEN".
Si está en "GREEN", pasa a "YELLOW".
Si está en "YELLOW", pasa a "RED".
Reinicio del Contador de Tiempo:
Se actualiza self.startTime con el tiempo actual para comenzar a contar el intervalo del nuevo estado.

Actualización de la Visualización:
Se llama al método display_state() para reflejar en el display la transición del semáforo, encendiendo el LED correspondiente al nuevo estado.
