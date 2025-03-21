### Mi solución a la actividad 14

``` js
from microbit import *
import utime
import music

// Definición de estados
STATE_INIT = 0
STATE_CONFIG = 1
STATE_ARMED = 2
STATE_EXPLODED = 3

// Intervalos (en ms) para mostrar mensajes o actualizar

CONFIG_INTERVAL = 500    // Mostrar/actualizar la pantalla cada 500 ms en config
COUNTDOWN_INTERVAL = 1000 // Disminuir el tiempo cada 1000 ms
EXPLODED_INTERVAL = 2000   // Mostrar "FIN" o un ícono de explosión cada cierto tiempo

// Límites de tiempo en segundos
MIN_TIME = 10
MAX_TIME = 60

// Variables de la bomba
current_state = STATE_INIT
start_time = 0          // Para medir cuándo hacer la siguiente acción
interval = 0            // Tiempo de espera entre acciones/actualizaciones
time_left = 20          // Tiempo configurado inicialmente (segundos)

def show_time(t):
    // Función sencilla para mostrar el tiempo en segundos
    // en el display. Se puede mejorar o cambiar por un dibujito.
    display.scroll(str(t))

while True:
    if current_state == STATE_INIT:
        // Pseudoestado para inicializar
        display.show(Image.HAPPY)      // Mostramos algo inicial
        time_left = 20                 // Valor inicial de la bomba
        start_time = utime.ticks_ms()
        interval = CONFIG_INTERVAL     // Cada 500 ms refrescaremos
        current_state = STATE_CONFIG

    elif current_state == STATE_CONFIG:
        // Estado de configuración: podemos subir/bajar el tiempo
        // con los botones A y B. se muestrta el tiempo cada 'interval'.
        
        // Chequear si ya pasó el tiempo para refrescar la pantalla
        if utime.ticks_diff(utime.ticks_ms(), start_time) > interval:
            show_time(time_left)
            start_time = utime.ticks_ms()  // Reiniciamos el contador

        // Subir tiempo
        if button_a.was_pressed():
            time_left = min(time_left + 1, MAX_TIME)
            show_time(time_left)

        // Bajar tiempo
        if button_b.was_pressed():
            time_left = max(time_left - 1, MIN_TIME)
            show_time(time_left)

        // Armar la bomba con un “shake”
        if accelerometer.was_gesture('shake'):
            # Pasamos al estado ARMADO
            current_state = STATE_ARMED
            start_time = utime.ticks_ms()
            interval = COUNTDOWN_INTERVAL  // 1000 ms para descontar 1s
            display.show(Image.SQUARE_SMALL)

    elif current_state == STATE_ARMED:
        // Estado armado: cada 'interval' (1s) se baja el contador
        if utime.ticks_diff(utime.ticks_ms(), start_time) >= interval:
            time_left -= 1
            start_time = utime.ticks_ms()
            show_time(time_left)

            if time_left <= 0:
                current_state = STATE_EXPLODED
                start_time = utime.ticks_ms()
                interval = EXPLODED_INTERVAL
                display.show(Image.SKULL) 

        // Botón “touch” para desarmar (ejemplo: pin1.read_digital() == 1)
        
        
            if button_b.was_pressed():
             current_state = STATE_CONFIG
             start_time = utime.ticks_ms()
             interval = CONFIG_INTERVAL
             display.show(Image.HAPPY)

    elif current_state == STATE_EXPLODED:
        // La bomba ha explotado: mostramos algún mensaje/imagen y
        
        // Cada 'interval' refrescamos la pantalla o reproducimos un sonido.
        
        if utime.ticks_diff(utime.ticks_ms(), start_time) >= interval:
            // Mostrar un mensaje de explosión
            display.scroll("BOOM!")
            display.show(Image.SKULL)
            music.play(music.FUNERAL)
            
            speaker.off()
            
            start_time = utime.ticks_ms()

        //  volver a config tocando un botón
        if button_a.was_pressed():
            // Reiniciamos
            current_state = STATE_CONFIG
            start_time = utime.ticks_ms()
            interval = CONFIG_INTERVAL
            time_left = 20
            display.show(Image.HAPPY)
```

- El pin 1 no funcionaba nada por lo que me toco cambiarlo por el boton , volvi a revisar el codigo y no se que más podría mejorar debido al riesgo de hacer que el codigo no funcione al cambiar por ejemplo de los intervalos.
