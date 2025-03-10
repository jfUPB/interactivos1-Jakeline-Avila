### Mi solucion a la actividad 1

Codigo:


from microbit import *
import utime

class Semaforo:
    def __init__(self, col, red_time, yellow_time, green_time):  
        self.col = col                        # Columna asignada para este semáforo  
        self.state = "RED"                    # Estado inicial   
        self.startTime = utime.ticks_ms()   # Momento en que se inicia el estado   
        # Tiempos de cada estado en milisegundos   
        self.intervals = {  
            "RED": red_time,  
            "YELLOW": yellow_time,  
            "GREEN": green_time  
        }
        self.display_state()                  # Mostrar el estado inicial  

  def update(self):     
        current_time = utime.ticks_ms()     
        # Verificar si se cumplió el tiempo del estado actual  
        if utime.ticks_diff(current_time, self.startTime) >= self.intervals[self.state]:    
            # Transición de estados      
            if self.state == "RED":     
                self.state = "GREEN"     
            elif self.state == "GREEN":      
                self.state = "YELLOW"            
            elif self.state == "YELLOW":          
                self.state = "RED"          
            self.startTime = current_time        
            self.display_state()             

  def display_state(self):         
        # Limpia solo la columna asignada (sin afectar otros semáforos)           
        for row in range(5):         
            display.set_pixel(self.col, row, 0)           
        # Representación de los estados en función de la fila:                           
        if self.state == "RED":                  
            display.set_pixel(self.col, 0, 9)    # Rojo: fila superior                
        elif self.state == "YELLOW":                
            display.set_pixel(self.col, 2, 9)    # Amarillo: fila central               
        elif self.state == "GREEN":                 
            display.set_pixel(self.col, 4, 9)    # Verde: fila inferior                                             

// Creación de los tres semáforos con sus tiempos específicos (en milisegundos)                
// Semáforo 1: 5 s en rojo, 2 s en amarillo, 3 s en verde               
semaforo1 = Semaforo(0, 5000, 2000, 3000)            
// Semáforo 2: 3 s en rojo, 1 s en amarillo, 2 s en verde              
semaforo2 = Semaforo(2, 3000, 1000, 2000)             
// Semáforo 3: 4 s en rojo, 3 s en amarillo, 2 s en verde           
semaforo3 = Semaforo(4, 4000, 3000, 2000)                  

while True:       
    semaforo1.update()            
    semaforo2.update()           
    semaforo3.update()             
    utime.sleep_ms(100)            

  - Reflexion: El uso de clases en este proyecto permite encapsular tanto los datos (como la columna asignada y los tiempos de cada estado) como los comportamientos (actualización y visualización del semáforo) en una única entidad, facilitando la reutilización y escalabilidad del código; además,
    la implementación de máquinas de estado garantiza transiciones claras y controladas entre los diferentes estados, lo que simplifica el manejo concurrente de varios semáforos en un solo display.
