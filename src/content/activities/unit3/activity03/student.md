### Mi solucion a la actividad 3

 ``` js
from microbit import *
import utime
import music

uart.init(baudrate=115200)

# Definición de estados
STATE_INIT = 0
STATE_CONFIG = 1
STATE_ARMED = 2
STATE_EXPLODED = 3

# Intervalos (en ms)
CONFIG_INTERVAL = 500    # Actualización en configuración cada 500 ms
COUNTDOWN_INTERVAL = 1000 # Descenso del contador cada 1000 ms
EXPLODED_INTERVAL = 2000  # Intervalo en estado de explosión

# Límites de tiempo (segundos)
MIN_TIME = 10
MAX_TIME = 60

# Variables globales de la bomba
current_state = STATE_INIT
start_time = 0          # Para medir el tiempo entre acciones
interval = 0            # Tiempo de espera entre actualizaciones
time_left = 20          # Tiempo configurado inicialmente (segundos)

# Variables globales para el manejo de eventos
event_available = False  # Indica si ocurrió un evento
event_value = ''         # Almacena el valor del evento ('A', 'B', 'S', 'T')

def show_time(t):
    display.scroll(str(t))

def tareaBomba():
    global current_state, start_time, interval, time_left, event_available, event_value
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
        if event_available:
            # Ajuste del tiempo mediante eventos 'A' y 'B'
            if event_value == 'A':
                time_left = min(time_left + 1, MAX_TIME)
                show_time(time_left)
            elif event_value == 'B':
                time_left = max(time_left - 1, MIN_TIME)
                show_time(time_left)
            # Armado de la bomba con el evento shake ('S')
            elif event_value == 'S':
                current_state = STATE_ARMED
                start_time = utime.ticks_ms()
                interval = COUNTDOWN_INTERVAL
                display.show(Image.SQUARE_SMALL)
            event_available = False  # Consumir el evento

    elif current_state == STATE_ARMED:
        if utime.ticks_diff(utime.ticks_ms(), start_time) >= interval:
            time_left -= 1
            start_time = utime.ticks_ms()
            show_time(time_left)
            if time_left <= 0:
                current_state = STATE_EXPLODED
                start_time = utime.ticks_ms()
                interval = EXPLODED_INTERVAL
                display.show(Image.SKULL)
        if event_available:
            # Desarme de la bomba con evento touch ('T')
            if event_value == 'T':
                current_state = STATE_CONFIG
                time_left = 20
                start_time = utime.ticks_ms()
                interval = CONFIG_INTERVAL
                display.show(Image.HAPPY)
            event_available = False  # Consumir el evento

    elif current_state == STATE_EXPLODED:
        if utime.ticks_diff(utime.ticks_ms(), start_time) >= interval:
            display.scroll("BOOM!")
            display.show(Image.SKULL)
            music.play(music.FUNERAL)
            start_time = utime.ticks_ms()
        if event_available:
            # Permite reiniciar la bomba en estado EXPLODED con el evento 'A'
            if event_value == 'A':
                current_state = STATE_CONFIG
                time_left = 20
                start_time = utime.ticks_ms()
                interval = CONFIG_INTERVAL
                display.show(Image.HAPPY)
            event_available = False  # Consumir el evento

def tareaEventos():
    global event_available, event_value
    # Lectura de eventos de sensores del micro:bit
    if button_a.was_pressed():
        event_value = 'A'
        event_available = True
    elif button_b.was_pressed():
        event_value = 'B'
        event_available = True
    elif accelerometer.was_gesture('shake'):
        event_value = 'S'
        event_available = True
    elif pin1.is_touched():
        event_value = 'T'
        event_available = True

    # Lectura de eventos desde el puerto serial
    if uart.any():
        serial_data = uart.read().strip()
        try:
            serial_str = serial_data.decode('utf-8').strip()
            if serial_str in ['A', 'B', 'S', 'T']:
                event_value = serial_str
                event_available = True
        except:
            pass

while True:
    tareaBomba()
    tareaEventos()
    utime.sleep_ms(50)
 ```
