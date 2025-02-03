<!-- Control de movimiento con micro:bit
Enunciado: crea un programa en p5.js que muestre un círculo en la pantalla. Utiliza los botones A y B para controlar la posición en x del círculo en el canvas de p5.js.

Nota: Esta actividad puede requerir combinar dos sesiones de 30 minutos.

Entrega: Código del programa en p5.js y el código para micro:bit. Describe cómo es posible mapear los botones al movimiento del círculo. -->

### Mi solución a la actividad 11

### p5.js
``` js
let port;
let circleX;

function setup() {
    createCanvas(400, 400);
    background(220);
    circleX = width / 2;

    port = createSerial();
    let connectBtn = createButton('Connect to micro:bit');
    connectBtn.position(80, 360);
    connectBtn.mousePressed(connectBtnClick);
}

function draw() {
    background(220);
    fill('blue');
    ellipse(circleX, height / 2, 50, 50);

    if (port.availableBytes() > 0) {
        let dataRx = port.read(1);
        if (dataRx == 'A') {
            circleX -= 10; 
        } else if (dataRx == 'B') {
            circleX += 10; 
        }
        circleX = constrain(circleX, 25, width - 25); 
    }

    if (!port.opened()) {
        select('button').html('Connect to micro:bit');
    } else {
        select('button').html('Disconnect');
    }
}

function connectBtnClick() {
    if (!port.opened()) {
        port.open('MicroPython', 115200);
    } else {
        port.close();
    }
}
``` 
### micro:bit Python
``` js
from microbit import *

uart.init(baudrate=115200)
display.show(Image.ARROW_E) 

while True:
    if button_a.is_pressed():
        uart.write('A')
        display.show(Image.ARROW_W)  
        sleep(200)
        display.show(Image.ARROW_E)

    if button_b.is_pressed():
        uart.write('B')  # Envía 'B' a p5.js
        display.show(Image.ARROW_E) 
        sleep(200)
```

### Resumen de lo que se hizo en el código:

- Se dibuja el círculo en p5.js
- Los botones A y B del micro:bit lo mueven en x, es decir, horizontalmente.
- El micro:bit muestra las flechas según el botón presionado.
