### Mi solucion a la actividad 4 

``` js
let port;
let connectBtn;
let armBtn;
let disarmBtn;
let timeBtnUp;
let timeBtnDown;
let timeLeft = 20;

function setup() {
    createCanvas(400, 400);
    background(220);

    // Crear el puerto serial
    port = createSerial();

    // Botón de conexión
    connectBtn = createButton('Connect to micro:bit');
    connectBtn.position(80, 300);
    connectBtn.mousePressed(connectBtnClick);

    // Botón para armar la bomba
    armBtn = createButton('Arm Bomb');
    armBtn.position(80, 340);
    armBtn.mousePressed(armBtnClick);

    // Botón para desarmar la bomba
    disarmBtn = createButton('Disarm Bomb');
    disarmBtn.position(220, 340);
    disarmBtn.mousePressed(disarmBtnClick);

    // Botones para subir y bajar el tiempo
    timeBtnUp = createButton('Increase Time');
    timeBtnUp.position(80, 380);
    timeBtnUp.mousePressed(increaseTime);

    timeBtnDown = createButton('Decrease Time');
    timeBtnDown.position(220, 380);
    timeBtnDown.mousePressed(decreaseTime);

    // Mostrar tiempo restante
    textSize(32);
    fill(0);
    text('Time Left: ' + timeLeft, width / 2 - 100, height / 2);
}

function draw() {
    // Actualiza el estado si se recibe datos desde el micro:bit
    if (port.availableBytes() > 0) {
        let dataRx = port.read(1);
        if (dataRx == 'A') {
            fill('red');
        } else if (dataRx == 'B') {
            fill('yellow');
        } else if (dataRx == 'C') {
            fill('green');
        } else if (dataRx == 'H') {
            fill('blue');
        } else {
            fill('white');
        }

        background(220);
        ellipse(width / 2, height / 2, 100, 100);
        fill('black');
        text('Time Left: ' + timeLeft, width / 2 - 100, height / 2);
    }

    // Si no está conectado, cambia el texto del botón
    if (!port.opened()) {
        connectBtn.html('Connect to micro:bit');
    } else {
        connectBtn.html('Disconnect');
    }
}

// Conectar al micro:bit a través del puerto serial
function connectBtnClick() {
    if (!port.opened()) {
        port.open('MicroPython', 115200); // Ajusta el nombre del puerto serial
    } else {
        port.close();
    }
}

// Función para armar la bomba
function armBtnClick() {
    port.write('A');  // Envía el mensaje 'A' al micro:bit para armar la bomba
    console.log('Bomb Armed');
}

// Función para desarmar la bomba
function disarmBtnClick() {
    port.write('B');  // Envía el mensaje 'B' al micro:bit para desarmar la bomba
    console.log('Bomb Disarmed');
}

// Función para aumentar el tiempo de la bomba
function increaseTime() {
    timeLeft = min(timeLeft + 1, 60);  // Aumenta el tiempo, máximo 60 segundos
    port.write('T' + timeLeft);  // Envia el tiempo actualizado al micro:bit
    console.log('Time Left: ' + timeLeft);
}

// Función para disminuir el tiempo de la bomba
function decreaseTime() {
    timeLeft = max(timeLeft - 1, 10);  // Disminuye el tiempo, mínimo 10 segundos
    port.write('T' + timeLeft);  // Envia el tiempo actualizado al micro:bit
    console.log('Time Left: ' + timeLeft);
}
```
