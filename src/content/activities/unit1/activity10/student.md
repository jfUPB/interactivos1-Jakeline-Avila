<!-- Interacción básica con micro:bit
Enunciado: crea un programa en p5.js que muestre un cuadrado en la pantalla. Conecta un micro:bit y programa un botón para que, al
presionarlo, cambie el color del cuadrado en p5.js. Recuerda que ya exploraste una actividad en la cuál al presionar botones pasan cosas en el programa en p5.js. ¿Recuerdas?

Nota: esta actividad puede requerir combinar dos sesiones de 30 minutos, todo depende de tu ritmo de trabajo.

Entrega: código del programa en p5.js y el código para micro:bit. Describe el proceso de conexión y comunicación entre ambos dispositivos. -->

### Solución a la actividad 10

- Usé el mismo código de la actividad 06 pero con modificaciones.

#### Código en micro:bit Python Editor:

``` js
from microbit import *

uart.init(baudrate=115200)
display.show(Image.SQUARE)  

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
                display.show(Image.SQUARE)  
```
- Usé el mismo código pero añadí en lo último display.show(Image.SQUARE) para que se muestre el cuadrado en el micro:bit.

#### Código en p5.js Web Editor

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
    rect(width / 2 - 50, height / 2 - 50, 100, 100);
}

function draw() {

    if (port.availableBytes() > 0) {
        let dataRx = port.read(1);
        if (dataRx == 'A') {
            fill('red');   
        }
        else if (dataRx == 'B') {
            fill('yellow'); 
        }
        else {
            fill('green'); 
        }
        background(220);
        rect(width / 2 - 50, height / 2 - 50, 100, 100);
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
- Usé el mismo código de la actividad 06 pero hice el cambio de ellipse() por rect()
- rect(width / 2 - 50, height / 2 - 50, 100, 100); dibuja un cuadrado de 100x100 píxeles en el centro de la pantalla

#### Descripción del proceso de comunicación entre ambos dispositivos:

La comunicación entre micro:bit y p5.js se realiza a través de puertos seriales, lo que permite que ambos dispositivos envíen y reciban datos en tiempo real.


---

#### 1. Estableciendo la Conexión

Micro:bit (MicroPython)

- Se inicia la comunicación serial con la computadora usando:

``` js
uart.init(baudrate=115200)
```

Esto configura el micro:bit para enviar y recibir datos a 115200 baudios a través del puerto USB.

- Se muestra un cuadrado en la pantalla del micro:bit:

``` js
display.show(Image.SQUARE)

```

Si se presionan botones o se detecta un movimiento, el micro:bit envía una letra correspondiente a p5.js:

Botón A → Envía 'A'

Botón B → Envía 'B'

Agitar → Envía 'C'



---

p5.js 

- En setup(), se crea una conexión serial:
  
``` js
port = createSerial();
```

Esto permite que p5.js se comunique con el micro:bit a través de un puerto USB virtual.

- Se crea un botón para conectar o desconectar el micro:bit :

``` js
connectBtn = createButton('Connect to micro:bit');
connectBtn.mousePressed(connectBtnClick);
```

- Al hacer clic en el botón, se ejecuta:

``` js
port.open('MicroPython', 115200);
```

que abre la conexión serial con el micro:bit.




---

#### 2. Enviando Datos

- Cuando se presiona el botón "Send Love" en p5.js, se ejecuta:

``` js
port.write('h');
```
Esto envía la letra 'h' al micro:bit.

- Micro:bit recibe este dato y lo interpreta en el código:

``` js
if uart.any():
    data = uart.read(1)  # Lee 1 byte de datos
    if data and data[0] == ord('h'):
        display.show(Image.HEART)
        sleep(500)
        display.show(Image.SQUARE)  # Vuelve a mostrar el cuadrado
```
Si recibe 'h', muestra un corazón por 500 ms y luego vuelve al cuadrado.



---

#### 3. Recibiendo Datos

- Cuando p5.js detecta que hay datos disponibles en el puerto serial:

``` js
if (port.availableBytes() > 0) {
    let dataRx = port.read(1);
```

Si recibe 'A', cambia el color del cuadrado a rojo.

Si recibe 'B', cambia el color del cuadrado a amarillo.

Si recibe otro dato ('C'), cambia el color a verde.

#### Foto del microbit y el código

![WhatsApp Image 2025-01-31 at 5 27 06 PM (3)](https://github.com/user-attachments/assets/18736584-8745-4e80-ba7f-e88f9f5901f4)

![WhatsApp Image 2025-01-31 at 5 27 06 PM (2)](https://github.com/user-attachments/assets/54e9eea9-a092-4615-9556-dd40c00812f8)

