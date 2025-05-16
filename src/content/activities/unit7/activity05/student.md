### Mi solución a la actividad 5



https://github.com/user-attachments/assets/c7b1c062-7a14-4df8-8fd6-58fba445c9e8

#### Cambios clave en mobile/sketch.js
En el archivo del cliente móvil, ahora ya no solo envío las coordenadas del toque, sino también el color y si quiero usar stroke o no.
Todo eso lo mando cuando el usuario toca la pantalla (dentro de touchMoved()).

Agregué variables para el color y el estado del stroke, que se generan aleatoriamente en setup():

``` js
let colorR, colorG, colorB;
let useStroke = false;

function setup() {
    // ...
    colorR = random(255);
    colorG = random(255);
    colorB = random(255);
}
``` 
Y dentro de touchMoved(), el mensaje ahora se ve así:

``` js
let touchData = {
    type: 'touch',
    x: mouseX,
    y: mouseY,
    r: colorR,
    g: colorG,
    b: colorB,
    stroke: useStroke
};

socket.emit('message', JSON.stringify(touchData));
```
Cada vez que muevo el dedo por la pantalla, se envía un objeto JSON con la posición, color y si se usa stroke o no.

#### Cambios clave en desktop/sketch.js
En el cliente de escritorio tuve que adaptar el código para que reciba esos datos y los use para crear partículas con comportamiento visual más interesante.

Primero, hice una clase Particle como en el ejemplo base de la actividad. Ahí uso los valores de posición, color y stroke que me llegan desde el móvil:

``` js
class Particle {
    constructor(x, y, r, g, b, strokeEnabled) {
        this.pos = createVector(x, y);
        this.vel = p5.Vector.random2D();
        this.lifespan = 255;
        this.size = random(5, 20);
        this.noiseOffset = random(1000);
        this.r = r;
        this.g = g;
        this.b = b;
        this.strokeEnabled = strokeEnabled;
    }

    update() {
        // mismo comportamiento de la actividad base
    }

    display() {
        if (this.strokeEnabled) {
            stroke(255);
        } else {
            noStroke();
        }
        fill(this.r, this.g, this.b, this.lifespan);
        ellipse(this.pos.x, this.pos.y, this.size);
    }

    isDead() {
        return this.lifespan < 0;
    }
}
```
Después, en socket.on('message'), cada vez que llega un mensaje del móvil, lo parseo y creo una nueva partícula con esos datos:

``` js
socket.on('message', (data) => {
    try {
        let parsedData = JSON.parse(data);
        if (parsedData.type === 'touch') {
            let p = new Particle(
                parsedData.x,
                parsedData.y,
                parsedData.r,
                parsedData.g,
                parsedData.b,
                parsedData.stroke
            );
            particles.push(p);
        }
    } catch (e) {
        console.error("Error parsing JSON:", e);
    }
});
```
Y en el draw() de escritorio, igual que en la actividad base, dibujo y actualizo todas las partículas:

``` js
function draw() {
    background(0, 20);
    for (let i = particles.length - 1; i >= 0; i--) {
        particles[i].update();
        particles[i].display();
        if (particles[i].isDead()) {
            particles.splice(i, 1);
        }
    }
}
```
#### ¿Modifiqué server.js?
No fue necesario. El servidor simplemente reenvía el mensaje del móvil al resto de los clientes. Como ya usábamos socket.broadcast.emit('message', message), todo funcionó bien sin tocar nada en el servidor.

