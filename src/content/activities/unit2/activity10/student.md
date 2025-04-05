<!-- Controlando la Pantalla con una Máquina de Estados y Concurrencia
Enunciado: implementa el siguiente programa que busca gestionar la concurrencia entre la secuencia de imágenes y la respuesta a la pulsación de botones.

Lee la siguiente descripción del problema y luego analiza la solución implementada. Para el análisis trata de observar estas preguntas:

¿Cómo es posible estructurar una aplicación usando una máquina de estados para poder atender varios eventos de manera concurrente?
¿Cómo haces para probar que el programa está correcto?
Descripción del problema

Imagina un programa para el micro:bit que muestra diferentes expresiones en la pantalla según un ciclo de tiempo, pero que también reacciona de inmediato si presionas un botón. Al iniciar, se muestra una cara feliz durante un segundo y medio. Después, el micro:bit cambia a una expresión sonriente que dura un segundo. Luego, aparece una cara triste durante dos segundos, y el ciclo vuelve a comenzar.

Sin embargo, si en cualquier momento se presiona el botón A mientras la cara feliz o la sonriente están en pantalla, el micro:bit interrumpe el ciclo y muestra inmediatamente la cara triste o feliz, respectivamente. Si se presiona el botón A mientras la cara triste está en pantalla, el dispositivo cambia a la expresión sonriente. Así, el programa combina una secuencia visual predefinida con la capacidad de responder rápidamente a la interacción del usuario.

from microbit import *
import utime

STATE_INIT = 0
STATE_HAPPY = 1
STATE_SMILE = 2
STATE_SAD = 3

HAPPY_INTERVAL = 1500
SMILE_INTERVAL = 1000
SAD_INTERVAL = 2000

current_state = STATE_INIT
start_time = 0
interval = 0

while True:
    # pseudoestado STATE_INIT
    if current_state == STATE_INIT:
        display.show(Image.HAPPY)
        start_time = utime.ticks_ms()
        interval = HAPPY_INTERVAL
        current_state = STATE_HAPPY
    elif current_state == STATE_HAPPY:
        if button_a.was_pressed():
            # Acciones para el evento
            display.show(Image.SAD)
            # Acciones de entrada para el siguiente estado
            start_time = utime.ticks_ms()
            interval = SAD_INTERVAL
            current_state = STATE_SAD
        if utime.ticks_diff(utime.ticks_ms(), start_time) > interval:
            # Acciones para el evento
            display.show(Image.SMILE)
            # Acciones de entrada para el siguiente estado
            start_time = utime.ticks_ms()
            interval = SMILE_INTERVAL
            current_state = STATE_SMILE
    elif current_state == STATE_SMILE:
        if button_a.was_pressed():
            display.show(Image.HAPPY)
            start_time = utime.ticks_ms()
            interval = HAPPY_INTERVAL
            current_state = STATE_HAPPY
        if utime.ticks_diff(utime.ticks_ms(), start_time) > interval:
            display.show(Image.SAD)
            start_time = utime.ticks_ms()
            interval = SAD_INTERVAL
           current_state = STATE_SAD
    elif current_state == STATE_SAD:
        if button_a.was_pressed():
            display.show(Image.SMILE)
            start_time = utime.ticks_ms()
            interval = SMILE_INTERVAL
            current_state = STATE_SMILE
        if utime.ticks_diff(utime.ticks_ms(), start_time) > interval:
            display.show(Image.HAPPY)
            start_time = utime.ticks_ms()
            interval = HAPPY_INTERVAL
            current_state = STATE_HAPPY -->

### Mi solución a la actividad 10

1. Importaciones


from microbit import *  
import utime  
from microbit import *: Esto importa las funcionalidades de la librería del micro:bit, lo que permite acceder a su pantalla LED, los botones, y otras funcionalidades.
import utime: Importa la librería utime, que proporciona funciones de tiempo, como ticks_ms y ticks_diff, que se utilizan para trabajar con tiempos y temporizadores.  

2. Definición de Constantes

STATE_INIT = 0  
STATE_HAPPY = 1  
STATE_SMILE = 2  
STATE_SAD = 3  
Estas son constantes que definen los diferentes "estados" en los que puede estar el programa:  

STATE_INIT: Estado inicial.  
STATE_HAPPY: Estado feliz.  
STATE_SMILE: Estado sonriente.  
STATE_SAD: Estado triste.  


HAPPY_INTERVAL = 1500  
SMILE_INTERVAL = 1000  
SAD_INTERVAL = 2000       

Aquí se definen los intervalos de tiempo (en milisegundos) para cada uno de los estados:

HAPPY_INTERVAL: El tiempo que el micro:bit permanecerá en el estado "feliz" antes de cambiar al estado "sonriente".                   
SMILE_INTERVAL: El tiempo en el que se mostrará el estado "sonriente" antes de cambiar al estado "triste".                  
SAD_INTERVAL: El tiempo que el micro:bit estará en el estado "triste" antes de cambiar al estado "feliz".             

3. Variables de Estado

current_state = STATE_INIT           
start_time = 0             
interval = 0              
current_state: Variable que almacena el estado actual del programa.                
start_time: Almacena el momento en que comienza un nuevo intervalo (tiempo de inicio de cada estado).            
interval: Almacena el tiempo de duración del estado actual (basado en el tipo de estado, ya sea "feliz", "sonriente", o "triste").            

6. Bucle Principal while True               
Este bucle ejecuta continuamente el código, manteniendo el micro:bit en un ciclo constante de verificación de botones y temporizadores.                

Estado 0: STATE_INIT                

if current_state == STATE_INIT:               
    display.show(Image.HAPPY)                   
    start_time = utime.ticks_ms()                 
    interval = HAPPY_INTERVAL                    
    current_state = STATE_HAPPY                      
Si el programa está en el estado inicial (STATE_INIT), muestra la imagen "feliz" en la pantalla.                    
Luego, toma el tiempo actual usando utime.ticks_ms(), lo guarda en start_time y establece el intervalo de tiempo a HAPPY_INTERVAL.                               
Finalmente, cambia el estado a STATE_HAPPY.                     
 
Estado 1: STATE_HAPPY              
 
elif current_state == STATE_HAPPY:                 
    if button_a.was_pressed():                  
        display.show(Image.SAD)                    
        start_time = utime.ticks_ms()                
        interval = SAD_INTERVAL               
        current_state = STATE_SAD                       
    if utime.ticks_diff(utime.ticks_ms(), start_time) > interval:              
        display.show(Image.SMILE)                       
        start_time = utime.ticks_ms()                      
        interval = SMILE_INTERVAL                      
        current_state = STATE_SMILE                             
        
Si el programa está en el estado "feliz" (STATE_HAPPY):                     
Si el usuario presiona el botón A (button_a.was_pressed()), se cambia a la imagen "triste" y se inicia un nuevo intervalo con el tiempo de SAD_INTERVAL, luego cambia el estado a STATE_SAD.                      
Si el tiempo ha pasado (es decir, si el tiempo transcurrido desde start_time supera HAPPY_INTERVAL), se cambia a la imagen "sonriente" y el intervalo se ajusta a SMILE_INTERVAL. Luego, el estado cambia a STATE_SMILE.                     

Estado 2: STATE_SMILE                  

elif current_state == STATE_SMILE:                     
    if button_a.was_pressed():                       
        display.show(Image.HAPPY)                  
        start_time = utime.ticks_ms()                  
        interval = HAPPY_INTERVAL                  
        current_state = STATE_HAPPY                    
    if utime.ticks_diff(utime.ticks_ms(), start_time) > interval:                   
        display.show(Image.SAD)                
        start_time = utime.ticks_ms()                             
        interval = SAD_INTERVAL                        
        current_state = STATE_SAD                  
Si el programa está en el estado "sonriente" (STATE_SMILE):                      
Si el usuario presiona el botón A, se cambia la imagen a "feliz" y el intervalo se establece a HAPPY_INTERVAL, luego el estado cambia a STATE_HAPPY.                   
Si el tiempo transcurrido supera SMILE_INTERVAL, se cambia a la imagen "triste" y el intervalo se ajusta a SAD_INTERVAL. Luego, el estado cambia a STATE_SAD.                  

Estado 3: STATE_SAD                    
   
elif current_state == STATE_SAD:            
    if button_a.was_pressed():                 
        display.show(Image.SMILE)                 
        start_time = utime.ticks_ms()            
        interval = SMILE_INTERVAL                  
        current_state = STATE_SMILE                   
    if utime.ticks_diff(utime.ticks_ms(), start_time) > interval:               
        display.show(Image.HAPPY)                 
        start_time = utime.ticks_ms()                      
        interval = HAPPY_INTERVAL                 
        current_state = STATE_HAPPY                  
        
Si el programa está en el estado "triste" (STATE_SAD):           
Si el usuario presiona el botón A, se cambia a la imagen "sonriente" y el intervalo se establece a SMILE_INTERVAL, luego el estado cambia a STATE_SMILE.           
Si el tiempo transcurrido supera SAD_INTERVAL, se cambia a la imagen "feliz" y el intervalo se ajusta a HAPPY_INTERVAL. Luego, el estado cambia a STATE_HAPPY.                                          
