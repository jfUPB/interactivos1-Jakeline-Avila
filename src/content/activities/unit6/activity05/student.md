### Mi solucion a la actividad 6

- Conceptualiza:
Idea: La nueva aplicación será un simple juego de seguimiento de ventanas en el que dos jugadores moverán sus ventanas y un círculo se desplazará entre sus pantallas en tiempo real. Los jugadores podrán ver el círculo moverse de una ventana a la otra, simulando una especie de interacción sin necesidad de interacción directa.

Interacción: La información que necesita intercambiarse entre los clientes es principalmente la posición de las ventanas (coordenadas de la pantalla y tamaño de la ventana) y la posición de un círculo que se mueve entre las pantallas.

Entre los clientes: Se intercambiarán las posiciones de las ventanas para que cada jugador vea la posición de la ventana del otro. Además, la posición del círculo será compartida entre ambos jugadores para que lo vean moverse.

- Visualización: ![image](https://github.com/user-attachments/assets/af64c0f2-eae6-4fa6-824a-6d9057b25d63)


Dos pantallas que muestran un círculo moviéndose entre las dos.

El círculo se moverá entre las ventanas, creando una sensación de interactividad sin necesidad de hacer clic ni tocar nada.

Los jugadores solo verán el círculo moviéndose entre sus pantallas, sin interacción directa.

- Planifica:
Modificaciones necesarias:

server.js:

Nuevo evento para actualizar la posición del círculo entre las dos pantallas.

Emitir las posiciones de las ventanas y las del círculo a todos los clientes conectados.

page1.js y page2.js:

Ambas páginas deben enviar y recibir las posiciones de las ventanas.

Ambas páginas deben actualizar y visualizar la posición del círculo.

El cliente debe detectar el cambio en las posiciones de las ventanas y actualizar la visualización del círculo según los datos recibidos.

- Implementa:

Codigo del server.js

``` js
const express = require('express');
const http = require('http');
const socketIO = require('socket.io');
const path = require('path');
const app = express();
const server = http.createServer(app);
const io = socketIO(server);
const port = 3000;

let players = {};
let ballData = { x: 300, y: 300, radius: 20 };

app.use(express.static(path.join(__dirname, 'views')));

app.get('/page1', (req, res) => {
    res.sendFile(path.join(__dirname, 'views', 'page1.html'));
});

app.get('/page2', (req, res) => {
    res.sendFile(path.join(__dirname, 'views', 'page2.html'));
});

io.on('connection', (socket) => {
    console.log('A user connected - ID:', socket.id);
    players[socket.id] = { score: 0, pageData: { x: 0, y: 0, width: 100, height: 100 } };

    socket.on('disconnect', () => {
        console.log('User disconnected - ID:', socket.id);
        delete players[socket.id];
    });

    // Actualización de la ventana de los jugadores
    socket.on('playerUpdate', (windowData, playerId) => {
        if (players[playerId]) {
            players[playerId].pageData = windowData;
        }
        broadcastGameData();
    });

    // Detectar si un jugador atrapa la pelota
    socket.on('ballCaught', (playerId) => {
        if (players[playerId]) {
            players[playerId].score++;
            // Mueve la pelota a una nueva posición aleatoria
            ballData = {
                x: Math.random() * 800 + 100,
                y: Math.random() * 600 + 100,
                radius: 20
            };
            broadcastGameData();
        }
    });
});

function broadcastGameData() {
    let gameData = {
        ballData: ballData,
        remotePage: Object.values(players).map(player => player.pageData)
    };
    io.emit('updateGameData', gameData);
}

server.listen(port, () => {
    console.log(`Server is listening on http://localhost:${port}`);
});

```

Codigo de page2.js:

``` js
let currentPageData = {
    x: window.screenX,
    y: window.screenY,
    width: window.innerWidth,
    height: window.innerHeight
};

let remotePageData = { x: 0, y: 0, width: 100, height: 100 };
let ballData = { x: window.innerWidth / 2, y: window.innerHeight / 2, radius: 20 };
let socket = io('http://localhost:3000');

socket.on('connect', () => {
    console.log(socket.id);
    socket.emit('playerUpdate', currentPageData, socket.id);
});

socket.on('updateGameData', (gameData) => {
    remotePageData = gameData.remotePage;
    ballData = gameData.ballData;
});

let previousPageData = {
    x: window.screenX,
    y: window.screenY,
    width: window.innerWidth,
    height: window.innerHeight
};

function setup() {
    createCanvas(windowWidth, windowHeight);
    frameRate(60);
}

function checkWindowPosition() {
    currentPageData = {
        x: window.screenX,
        y: window.screenY,
        width: window.innerWidth,
        height: window.innerHeight
    };

    if (currentPageData.x !== previousPageData.x || currentPageData.y !== previousPageData.y ||
        currentPageData.width !== previousPageData.width || currentPageData.height !== previousPageData.height) {

        socket.emit('playerUpdate', currentPageData, socket.id);
        previousPageData = currentPageData;
    }
}

function draw() {
    background(220);

    // Dibuja la pelota
    fill(255, 0, 0);
    ellipse(ballData.x, ballData.y, ballData.radius * 2, ballData.radius * 2);

    // Dibuja la ventana del jugador
    fill(0, 255, 0);
    rect(currentPageData.x, currentPageData.y, currentPageData.width, currentPageData.height);

    checkWindowPosition();

    // Chequear si la ventana está cerca de la pelota
    let distance = dist(currentPageData.x + currentPageData.width / 2, currentPageData.y + currentPageData.height / 2, ballData.x, ballData.y);
    if (distance < ballData.radius + currentPageData.width / 2) {
        socket.emit('ballCaught', socket.id);
    }

    // Mostrar la línea entre las ventanas de los dos jugadores
    let vector1 = createVector(currentPageData.x, currentPageData.y);
    let vector2 = createVector(remotePageData.x, remotePageData.y);
    let resultingVector = createVector(vector2.x - vector1.x, vector2.y - vector1.y);
    stroke(50);
    strokeWeight(20);
    line(currentPageData.x + currentPageData.width / 2, currentPageData.y + currentPageData.height / 2,
        resultingVector.x + remotePageData.width / 2, resultingVector.y + remotePageData.height / 2);
}

function windowResized() {
    resizeCanvas(windowWidth, windowHeight);
}

```

Codigo de page1.js:

``` js
let currentPageData = {
    x: window.screenX,
    y: window.screenY,
    width: window.innerWidth,
    height: window.innerHeight
};

let remotePageData = { x: 0, y: 0, width: 100, height: 100 };
let ballData = { x: window.innerWidth / 2, y: window.innerHeight / 2, radius: 20 };

let socket = io('http://localhost:3000');

socket.on('connect', () => {
    console.log(socket.id);
    socket.emit('playerUpdate', currentPageData, socket.id);
});

socket.on('updateGameData', (gameData) => {
    remotePageData = gameData.remotePage;
    ballData = gameData.ballData;
});

function setup() {
    createCanvas(windowWidth, windowHeight);
    frameRate(60);
}

function checkWindowPosition() {
    currentPageData = {
        x: window.screenX,
        y: window.screenY,
        width: window.innerWidth,
        height: window.innerHeight
    };

    socket.emit('playerUpdate', currentPageData, socket.id);
}

function draw() {
    background(220);

    // Dibuja la pelota
    fill(255, 0, 0);
    ellipse(ballData.x, ballData.y, ballData.radius * 2, ballData.radius * 2);

    // Dibuja la ventana del jugador
    fill(0, 255, 0);
    rect(currentPageData.x, currentPageData.y, currentPageData.width, currentPageData.height);

    checkWindowPosition();

    // Chequear si la ventana está cerca de la pelota
    let distance = dist(currentPageData.x + currentPageData.width / 2, currentPageData.y + currentPageData.height / 2, ballData.x, ballData.y);
    if (distance < ballData.radius + currentPageData.width / 2) {
        socket.emit('ballCaught', socket.id);
    }
}

function windowResized() {
    resizeCanvas(windowWidth, windowHeight);
}

```
- Facil y dificil

Reflexión sobre el proceso:
Lo fácil: Implementar la comunicación entre los clientes usando socket.io fue bastante sencillo. Solo necesitábamos manejar la actualización de posiciones y emitir los datos a todos los clientes conectados.

Lo difícil: Asegurarse de que la posición del círculo fuera coherente y se moviera de forma fluida entre las ventanas en tiempo real. Además, gestionar la comunicación sin latencias visibles para el usuario fue un desafío leve.

Enlaces: http://localhost:3000/page1
http://localhost:3000/page2
