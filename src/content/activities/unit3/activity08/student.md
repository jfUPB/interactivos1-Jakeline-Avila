### Mi solución a la actividad 8

``` js
from microbit import *
import music
import radio

# Configuración de radio para la comunicación
radio.on()

# Definición de estados de la bomba
STATE_INIT = 0
STATE_ARMED = 1
STATE_DISARMED = 2
STATE_EXPLODED = 3

# Intervalos de tiempo
COUNTDOWN_INTERVAL = 1000  # 1 segundo para el conteo regresivo
EXPLODED_INTERVAL = 2000   # Intervalo de explosión

# Variables de la bomba
current_state = STATE_INIT
time_left = 20  # Tiempo inicial para la bomba
start_time = running_time()

def send_time_left():
    # Enviar el tiempo restante al puerto serial
    uart.write('T' + str(time_left) + '\n')

def show_time(t):
    # Mostrar el tiempo restante en el display
    display.scroll(str(t))

while True:
    # Escuchar los comandos desde p5.js
    if uart.any():
        data = uart.read()

        # Comandos recibidos desde p5.js
        if data == b'ARM':
            current_state = STATE_ARMED
            time_left = 20  # Reiniciar el tiempo
            display.show(Image.SQUARE_SMALL)
            uart.write(b'A')  # Responder con 'A' (armada)

        elif data == b'DISARM':
            current_state = STATE_DISARMED
            display.show(Image.HAPPY)
            uart.write(b'B')  # Responder con 'B' (desarmada)

    # Si la bomba está armada, comenzar el conteo regresivo
    if current_state == STATE_ARMED:
        if running_time() - start_time >= COUNTDOWN_INTERVAL:
            time_left -= 1
            start_time = running_time()  # Reiniciar el temporizador
            send_time_left()  # Enviar el tiempo restante al serial
            show_time(time_left)  # Mostrar en el display del micro:bit

            # Verificar si el tiempo se ha agotado
            if time_left <= 0:
                current_state = STATE_EXPLODED
                display.show(Image.SKULL)
                music.play(music.FUNERAL)
                uart.write(b'E')  # Enviar 'E' (explosion)

    # Si la bomba está desarmada, detener el conteo
    if current_state == STATE_DISARMED:
        display.show(Image.HAPPY)

    # Esperar un pequeño tiempo antes de la siguiente iteración
    sleep(100)

```
ahora en p5.js

``` js
let port;
let connectBtn;
let armBtn;
let disarmBtn;
let timerValue = 20; // Tiempo inicial para la bomba
let isArmed = false; // Estado de la bomba (armada/desarmada)

function setup() {
    createCanvas(400, 400);
    background(220);

    // Crear el objeto de comunicación serial
    port = createSerial();

    // Crear botones para conectar, armar y desarmar la bomba
    connectBtn = createButton('Connect to micro:bit');
    connectBtn.position(80, 300);
    connectBtn.mousePressed(connectBtnClick);

    armBtn = createButton('Arm Bomb');
    armBtn.position(80, 350);
    armBtn.mousePressed(armBomb);

    disarmBtn = createButton('Disarm Bomb');
    disarmBtn.position(220, 350);
    disarmBtn.mousePressed(disarmBomb);

    fill('white');
    ellipse(width / 2, height / 2, 100, 100);
}

function draw() {
    // Leer datos disponibles del micro:bit
    if (port.availableBytes() > 0) {
        let dataRx = port.read(1); // Leer un byte de datos

        // Si se recibe el estado de la bomba, actualizar la interfaz
        if (dataRx == 'A') {
            isArmed = true;
            fill('red');
        } else if (dataRx == 'B') {
            isArmed = false;
            fill('green');
        } else if (dataRx == 'T') {
            // Si se recibe un valor de tiempo restante (por ejemplo, "T20")
            timerValue = int(port.readLine().trim()); 
        }

        // Redibujar el fondo y el círculo
        background(220);
        ellipse(width / 2, height / 2, 100, 100);

        // Mostrar el tiempo restante y el estado de la bomba
        fill('black');
        textSize(24);
        textAlign(CENTER, CENTER);
        text(timerValue, width / 2, height / 2);
        textSize(16);
        text(isArmed ? 'Armed' : 'Disarmed', width / 2, height / 2 + 30);
    }

    // Actualizar el texto del botón según el estado de la conexión
    if (!port.opened()) {
        connectBtn.html('Connect to micro:bit');
    } else {
        connectBtn.html('Disconnect');
    }
}

// Función para conectar/desconectar al puerto serial
function connectBtnClick() {
    if (!port.opened()) {
        port.open('MicroPython', 115200);  // Asegúrate de que el puerto sea correcto
    } else {
        port.close();
    }
}

// Función para armar la bomba
function armBomb() {
    if (port.opened()) {
        port.write('ARM'); // Enviar el comando 'ARM' al micro:bit
    }
}

// Función para desarmar la bomba
function disarmBomb() {
    if (port.opened()) {
        port.write('DISARM'); // Enviar el comando 'DISARM' al micro:bit
    }
}
```
