### Mi solucion a la actividad 6

Título de la aplicación:

"Sincronía Creativa" — Un lienzo compartido en tiempo real


---

1. Descripción de la idea

La aplicación convierte las dos ventanas (page1 y page2) en un lienzo colaborativo interactivo:
Cada usuario puede mover su mouse y "dibujar" con su puntero. Lo interesante es que cada cliente ve en su pantalla la trayectoria del otro usuario, como si estuvieran dibujando juntos en tiempo real desde diferentes lugares.

La visualización combina líneas suaves que siguen el movimiento de la otra persona. Incluso se puede agregar que si ambos punteros están cerca, se cambie el color o espesor de las líneas (representando "encuentros").


---

2. Plan de implementación

a) Información que se intercambia:

Posición del puntero del mouse (x, y) de cada cliente.

Estado del mouse (presionado o no).

Opcional: color aleatorio al conectar, para distinguir a cada usuario.


b) Cambios necesarios en el código:

server.js

Recibe la posición del mouse y el estado (presionado) desde cada cliente.

Reenvía esos datos al otro cliente mediante socket.broadcast.emit.


page1.js y page2.js

Detectan el movimiento y clic del mouse.

Envían sus coordenadas al servidor si están dibujando.

Reciben las coordenadas del otro cliente y las visualizan como trazos en su propio canvas.



---

3. Implementación resumida del código

server.js

const io = require('socket.io')(3000);

io.on('connection', (socket) => {
  console.log('User connected:', socket.id);

  socket.on('drawing', (data) => {
    socket.broadcast.emit('draw-other', data);
  });

  socket.on('disconnect', () => {
    console.log('User disconnected:', socket.id);
  });
});


---

page1.js (igual para page2.js con distinto color, por ejemplo)

let socket = io('http://localhost:3000');
let remotePoints = [];
let drawing = false;

function setup() {
  createCanvas(windowWidth, windowHeight);
  strokeWeight(3);
}

function draw() {
  background(255);

  // Dibuja las líneas remotas
  stroke(200, 0, 100);
  noFill();
  beginShape();
  for (let pt of remotePoints) {
    vertex(pt.x, pt.y);
  }
  endShape();

  // Si estamos dibujando, enviamos nuestra posición
  if (drawing) {
    socket.emit('drawing', { x: mouseX, y: mouseY });
  }
}

function mousePressed() {
  drawing = true;
}

function mouseReleased() {
  drawing = false;
}

socket.on('draw-other', (data) => {
  remotePoints.push(data);
  if (remotePoints.length > 100) remotePoints.shift(); // Limita la cantidad
});


---

4. Reflexión final

¿Qué fue fácil?

Establecer la comunicación con Socket.IO ya que el ejemplo base está bien estructurado.

Dibujar en p5.js y enviar datos simples como coordenadas.


¿Qué fue difícil?

Controlar cuándo enviar datos (evitar sobrecargar la red).

Coordinar el dibujo para que no se desborde (usé remotePoints.shift() para limpiar memoria).


¿Qué aprendí?

La importancia de detectar cambios y enviar solo información relevante.

Cómo una arquitectura cliente-servidor simple puede volverse poderosa y expresiva.

Cómo p5.js puede ser más que visual: puede ser colaborativa cuando se conecta con Socket.IO.



---

5. Enlace de entrega
