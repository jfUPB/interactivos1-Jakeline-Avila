### Mi solucion actividad 7

``` python
let timeLeft = 20;  // Tiempo inicial de la bomba
let bombState = 'IDLE';  // Estado de la bomba: 'IDLE' (en espera), 'ARMED' (armada), 'EXPLODED' (explotada)
let countdownInterval;
let countdownSpeed = 20000; // Velocidad de cuenta atrás en milisegundos (1 segundo)

function setup() {
  createCanvas(400, 400);
  textAlign(CENTER, CENTER);
  textSize(32);
  // Mensaje inicial
  displayMessage("Bomba en espera");
}

function draw() {
  background(220);

  // Dibujar la bomba
  drawBomb();

  // Mostrar el estado actual
  fill(0);
  text('Estado: ' + bombState, width / 2, height - 50);

  // Mostrar el tiempo restante
  if (bombState === 'ARMED') {
    fill(255, 0, 0); // Rojo cuando la bomba está armada
    text(timeLeft, width / 2, height / 2);
  } else if (bombState === 'EXPLODED') {
    fill(0, 255, 0); // Verde cuando la bomba ha explotado
    text('BOOM!', width / 2, height / 2);
  }

  // Si la bomba está armada, reducir el tiempo
  if (bombState === 'ARMED' && timeLeft > 0) {
    timeLeft -= 1;
  }

 
}

// Función para dibujar la bomba
function drawBomb() {
  // Dibujar una bomba simple como un círculo
  fill(0);
  ellipse(width / 2, height / 2, 100, 100); // Círculo central
  fill(255, 0, 0);
  rect(width / 2 - 10, height / 2 - 60, 20, 60);  // "Mecha" en la parte superior

  // Cambiar el color de la bomba según el estado
  if (bombState === 'ARMED') {
    fill(255, 0, 0);  // Rojo cuando está armada
  } else if (bombState === 'EXPLODED') {
    fill(0, 255, 0);  // Verde cuando ha explotado
  } else {
    fill(150);  // Gris cuando está inactiva
  }

  ellipse(width / 2, height / 2, 100, 100);  // Círculo central (la bomba)
}

// Función para armar la bomba (botón o comando simulado)
function armBomb() {
  if (bombState === 'IDLE') {
    bombState = 'ARMED';
    timeLeft = 20;  // Reiniciar el tiempo
    displayMessage("Bomba armada, tiempo: " + timeLeft + " segundos.");
    // Aquí comenzamos el temporizador solo cuando la bomba se arme
    countdownInterval = setInterval(() => {
      if (bombState === 'ARMED' && timeLeft > 0) {
        timeLeft--;
      }   // Si el tiempo llega a cero, la bomba explota
  if (timeLeft <= 0 && bombState === 'ARMED') {
    bombState = 'EXPLODED';
    displayMessage("BOOM! La bomba explotó.");
    clearInterval(countdownInterval);  // Detener el temporizador cuando explota
  }
    }, countdownSpeed);  // Llamar a esta función cada segundo
  }
}

// Función para desarmar la bomba
function disarmBomb() {
  if (bombState === 'ARMED') {
    bombState = 'IDLE';
    timeLeft = 20;
    displayMessage("Bomba desarmada.");
    clearInterval(countdownInterval);  // Detener la cuenta atrás si se desarma
  }

}

// Función para reiniciar la bomba después de una explosión
function resetBomb() {
  bombState = 'IDLE';
  timeLeft = 20;
  displayMessage("Bomba reiniciada.");
}

// Función para mostrar mensajes en la pantalla
function displayMessage(message) {
  fill(0);
  textSize(24);
  text(message, width / 2, height - 100);
}

// Función para manejar los eventos de teclas (Ejemplo de interacción)
function keyPressed() {
  if (key === 'A' || key === 'a') {
    armBomb();  // Armar la bomba al presionar la tecla A
  } else if (key === 'B' || key === 'b') {
    disarmBomb();  // Desarmar la bomba al presionar la tecla B
  } else if (key === 'R' || key === 'r') {
    resetBomb();  // Reiniciar la bomba al presionar la tecla R
  }
}

```
