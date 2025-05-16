<!-- Leyendo Datos Seriales
Enunciado: investiga y experimenta con la recepción de datos seriales en el micro:bit. Recuerdas en la unidad anterior el botón de "Send Love" (puedes comenzar tu investigación por aquí)

Entrega:

Código que lea un carácter enviado por el puerto serial y lo muestre en la pantalla LED.
Documentación del proceso -->

### Mi solución a la actividad 5

- Usando el código de la actividad 6  unidad 1

``` js
from microbit import *

uart.init(baudrate=115200)
display.show(Image.BUTTERFLY)

while True:
    if button_a.is_pressed():
        uart.write('A')
        sleep(500)
    if button_b.is_pressed():
        uart.write('B')
        sleep(500)
    if accelerometer.was_gesture('shake'):
        uart.write('C')
        sleep(500)
    if uart.any():
        data = uart.read(1)
        if data:
            if data[0] == ord('h'):
                display.show(Image.HEART)
                sleep(500)
                display.show(Image.HAPPY)
```

``` js

let port;
let connectBtn;

function setup() {
    createCanvas(400, 400);
    background(220);
    port = createSerial();
    connectBtn = createButton('Connect to micro:bit');
    connectBtn.position(80, 300);
    connectBtn.mousePressed(connectBtnClick);
    let sendBtn = createButton('Send Love');
    sendBtn.position(220, 300);
    sendBtn.mousePressed(sendBtnClick);
    fill('white');
    ellipse(width / 2, height / 2, 100, 100);
}

function draw() {

    if(port.availableBytes() > 0){
        let dataRx = port.read(1);
        if(dataRx == 'A'){
            fill('red');   
        }
        else if(dataRx == 'B'){
            fill('yellow'); 
        }
        else{
            fill('green'); 
        }
        background(220);
        ellipse(width / 2, height / 2, 100, 100);
        fill('black');
        text(dataRx, width / 2, height / 2);
    }    


    if (!port.opened()) {
        connectBtn.html('Connect to micro:bit');
    } 
    else {
        connectBtn.html('Disconnect');
    }
}

function connectBtnClick() {
    if (!port.opened()) {
        port.open('MicroPython', 115200);
    } else {
        port.close();
    }
}

function sendBtnClick() {
    port.write('h');
}

```
- Usando este codigo primeramente en la configuracion inicial (setup)) se crea un lienzo de 400x400 pixeles, se inicializa el puerto serial con createSerial(), se agregan dos botones
Connect to micro:bit y Send love que basicamente envía el caracter 'h' al micro:bit.
- Se hace el dibujado con (draw()).
- Dependiendo del carácter recibido 'A': Dibuja un circulo rojo. 'B': Dibuja un círculo amarillo y cualquier otro carácter: Dibuja un círculo verde.
  y se muestra el carácter recibido en la pantalla.

- ConnectBtnClick(): Abre o cierra la conexion con el micro:bit.
- SendBtnClick(): Envía el carácter 'h' al micro:bit.


