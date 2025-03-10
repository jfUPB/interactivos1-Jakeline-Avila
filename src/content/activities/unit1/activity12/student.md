## Mi solución a la actividad 12

- Código de micro:bit python editor

``` js
from microbit import *

uart.init(baudrate=115200)

while True:
    if uart.any():
        data = uart.read(1)
        if data:
            if data == b'1':  
                display.show(Image.HEART)
            elif data == b'2':  
                display.show(Image.HAPPY)
            elif data == b'3':  
                display.show(Image.BUTTERFLY)
            elif data == b'4':  
                display.show(Image.GHOST)
```
- Código de p5.js

``` js
let port;
let selectMenu;

function setup() {
    createCanvas(400, 400);
    background(220);

    port = createSerial();
    
    let connectBtn = createButton('Connect to micro:bit');
    connectBtn.position(100, 360);
    connectBtn.mousePressed(connectBtnClick);

    selectMenu = createSelect();
    selectMenu.position(100, 320);
    selectMenu.option('Corazón', '1');
    selectMenu.option('Carita Feliz', '2');
    selectMenu.option('Mariposa', '3');
    selectMenu.option('Fantasma', '4');

    let sendBtn = createButton('Enviar Imagen');
    sendBtn.position(220, 320);
    sendBtn.mousePressed(sendImage);
}

function draw() {
    background(220);
}

function connectBtnClick() {
    if (!port.opened()) {
        port.open('MicroPython', 115200);
    } else {
        port.close();
    }
}

function sendImage() {
    if (port.opened()) {
        let selectedValue = selectMenu.value();
        port.write(selectedValue); 
    }
}
```
### Resumen:
- Se crea un botón de selección con 4 opciones ya propuestas que son fantasma, corazon, carita feliz y mariposa.
- Se crea el botón enviar imagen y se envia el numero correspondiente del 1-4 al microbit por el puerto serial.
- Se usa port.write(selectValue) para enviar el número de la imagen seleccionada.
- En micropythn se leee el dato recibido con uart.read(1)
- Dependiendo del numero recibido se muestra la imagen correspondiente en la pantalla del microbit.
