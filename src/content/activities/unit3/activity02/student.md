### Mi solucion a la actividad 2


``` js
from microbit import * 
import utime              
import music
             
# Definición de estados          
STATE_INIT = 0            
STATE_CONFIG = 1                                 
STATE_ARMED = 2               
STATE_EXPLODED = 3                
              
# Intervalos (en ms) para mostrar mensajes o actualizar              
CONFIG_INTERVAL = 500    # Actualización cada 500 ms en config                          
COUNTDOWN_INTERVAL = 1000 # Disminuir el tiempo cada 1000 ms             
EXPLODED_INTERVAL = 2000  # Actualización en estado de explosión              

# Límites de tiempo (en segundos)               
MIN_TIME = 10            
MAX_TIME = 60                     

# Variables globales de la bomba               
current_state = STATE_INIT               
start_time = 0          # Para medir el tiempo entre acciones              
interval = 0            # Tiempo de espera entre actualizaciones               
time_left = 20          # Tiempo configurado inicialmente (segundos)                
                   
# Secuencia para desactivar la bomba: botón A, botón B, botón A, shake               
DISARM_SEQUENCE = ["A", "B", "A", "SHAKE"]                        
disarm_index = 0  # Progreso actual en la secuencia            

def show_time(t):                 
    # Función para mostrar el tiempo en segundos                                    
    display.scroll(str(t))                 

while True:            
    if current_state == STATE_INIT:               
        display.show(Image.HAPPY)            
        time_left = 20            
        start_time = utime.ticks_ms()                   
        interval = CONFIG_INTERVAL          
        current_state = STATE_CONFIG               

   elif current_state == STATE_CONFIG:                       
        if utime.ticks_diff(utime.ticks_ms(), start_time) > interval:           
            show_time(time_left)             
            start_time = utime.ticks_ms()             
        if button_a.was_pressed():                 
            time_left = min(time_left + 1, MAX_TIME)            
            show_time(time_left)            
        if button_b.was_pressed():             
            time_left = max(time_left - 1, MIN_TIME)                                        
            show_time(time_left)                     
        # Armar la bomba con un shake y reiniciar la secuencia de desarme           
        if accelerometer.was_gesture('shake'):                        
            current_state = STATE_ARMED             
            disarm_index = 0          
            start_time = utime.ticks_ms()           
            interval = COUNTDOWN_INTERVAL          
            display.show(Image.SQUARE_SMALL)                                    
               
   elif current_state == STATE_ARMED:                
        # Actualizar cuenta regresiva cada segundo 
        if utime.ticks_diff(utime.ticks_ms(), start_time) >= interval:             
            time_left -= 1            
            start_time = utime.ticks_ms()          
            show_time(time_left)            
            if time_left <= 0:          
                current_state = STATE_EXPLODED           
                start_time = utime.ticks_ms()                            
                interval = EXPLODED_INTERVAL
                display.show(Image.SKULL)                                    
      
   # Verificar la secuencia de desactivación: A, B, A, shake           
   if button_a.was_pressed():
            if DISARM_SEQUENCE[disarm_index] == "A":                 
                disarm_index += 1           
            else:           
                disarm_index = 0                        
        if button_b.was_pressed():
            if DISARM_SEQUENCE[disarm_index] == "B":                 
                disarm_index += 1
            else:            
                disarm_index = 0            
        if accelerometer.was_gesture("shake"):                                     
            if DISARM_SEQUENCE[disarm_index] == "SHAKE":
                disarm_index += 1          
            else:              
                disarm_index = 0               
        
   # Si se completa la secuencia, se desarma la bomba y vuelve a configuración           
   if disarm_index == len(DISARM_SEQUENCE):                          
            current_state = STATE_CONFIG           
            start_time = utime.ticks_ms()                            
            interval = CONFIG_INTERVAL            
            time_left = 20                         
            display.show(Image.HAPPY)      
             
  elif current_state == STATE_EXPLODED:          
        if utime.ticks_diff(utime.ticks_ms(), start_time) >= interval:                                            
            display.scroll("BOOM!")                              
            display.show(Image.SKULL)                 
            music.play(music.FUNERAL)                
            start_time = utime.ticks_ms()          
        # Reiniciar la bomba tocando el botón A        
        if button_a.was_pressed():           
            current_state = STATE_CONFIG
            start_time = utime.ticks_ms()         
            interval = CONFIG_INTERVAL              
            time_left = 20                
            display.show(Image.HAPPY)             
```
