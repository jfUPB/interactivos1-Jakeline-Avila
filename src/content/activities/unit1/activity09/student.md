<!-- Generando patrones visuales
Enunciado: crea un programa en p5.js que genere patrones visuales aleatorios utilizando funciones matemáticas simples (ej. random(), sin(), cos()). Experimenta con diferentes parámetros para modificar los patrones generados. No es necesario conectar con micro:bit en esta actividad.

Entrega: código del programa en p5.js y una captura de pantalla del patrón visual generado. Describe brevemente las funciones utilizadas y cómo modificaste los parámetros. -->

### Solución a la actividad 9

#### Tomando en cuenta el código de la actividad 6

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

Lo cambié de forma que no necesite al microbit y realicé este nuevo código con nuevos cambios para que de diferentes cambios de patrones.

``` js
let changePatternBtn;

function setup() {
  createCanvas(400, 400);
  background(220);
  
  changePatternBtn = createButton('Cambiar Patrón');
  changePatternBtn.position(140, 360);
  changePatternBtn.mousePressed(drawPattern);

  drawPattern();
}

function drawPattern() {
  background(220);
  let patternType = int(random(3));

  if (patternType == 0) {
    drawWavePattern();
  } else if (patternType == 1) {
    drawCircleGrid();
  } else {
    drawRandomLines();
  }
}

function drawWavePattern() {
  stroke(random(50, 255), random(50, 255), random(50, 255));
  strokeWeight(2);
  noFill();
  for (let y = 0; y < height; y += 20) {
    beginShape();
    for (let x = 0; x < width; x += 10) {
      let yOffset = sin(x * 0.05) * 20;
      vertex(x, y + yOffset);
    }
    endShape();
  }
}

function drawCircleGrid() {
  for (let x = 20; x < width; x += 40) {
    for (let y = 20; y < height; y += 40) {
      let size = random(10, 30);
      fill(random(255), random(255), random(255));
      noStroke();
      ellipse(x, y, size, size);
    }
  }
}

function drawRandomLines() {
  strokeWeight(2);
  for (let i = 0; i < 50; i++) {
    stroke(random(255), random(255), random(255));
    let x1 = random(width);
    let y1 = random(height);
    let x2 = x1 + cos(random(TWO_PI)) * 50;
    let y2 = y1 + sin(random(TWO_PI)) * 50;
    line(x1, y1, x2, y2);
  }
}
```

#### Explicación de cada paso y diferencias:

``` js
let changePatternBtn;

function setup() {
  createCanvas(400, 400);
  background(220);
  
  changePatternBtn = createButton('Cambiar Patrón');
  changePatternBtn.position(140, 360);
  changePatternBtn.mousePressed(drawPattern);

  drawPattern();
}
```

- Aquí no creo ninguno de los port para el microbit debido a que se dijo en la actividad que no se necesita, así que solo cree el canva, el fondo y un botón para cambiar el patrón.

``` js
function drawPattern() {
  background(220);
  let patternType = int(random(3));

  if (patternType == 0) {
    drawWavePattern();
  } else if (patternType == 1) {
    drawCircleGrid();
  } else {
    drawRandomLines();
  }
}
```
- La actividad dice que se utilice las funciones matematicas simples, usé int con random para generar un número aleatorio entre 0 y 2; con if (patternType ==o) DrawWavePattern() lo que hago es que se genere
el patrón de ondas (olas) si el número generado es 0; lo mismo pasa con else if (patternType == 1) drawCircleGrid() si el número generado es 1; Si es otro número se generan lineas distintas.

``` js
function drawWavePattern() {
  stroke(random(50, 255), random(50, 255), random(50, 255));
  strokeWeight(2);
  noFill();
  for (let y = 0; y < height; y += 20) {
    beginShape();
    for (let x = 0; x < width; x += 10) {
      let yOffset = sin(x * 0.05) * 20;
      vertex(x, y + yOffset);
    }
    endShape();
  }
```

- Esta es la función para crear el patrón de ondas, con stroke(()) se genera el color aleatorio
- Con StrokeWeight basicamente el tamaño del grosor de la línea.
- NoFill para que se dibujen las líneas sin relleno.
- For (Let y = 0; y < height; y +=20...) es para que se generen en en una cuadricula de 20 pixeles entre las filas y el otro for es para que se generen en 10, de esta forma tenemos más control
del proceso de generación.
- Utilizamos sin(x *0.05)*20 para generar la onda.
- Vertex(x, y + yosett para dibujar la línea siguiendo la forma de la onda.

``` js
function drawCircleGrid() {
  for (let x = 20; x < width; x += 40) {
    for (let y = 20; y < height; y += 40) {
      let size = random(10, 30);
      fill(random(255), random(255), random(255));
      noStroke();
      ellipse(x, y, size, size);
    }
  }
}

```
- Se hace lo mismo pero la diferencia es que se genera en una cuadricula 40x40.
-  Se utiliza esta vez fill con colores aleatorios.
-  Se eliminan los bordes de la linea con noStroke().
-  Se dibuja un circulo con ellipse en (x, y) con el tamaño aleatorio.

``` js
function drawRandomLines() {
  strokeWeight(2);
  for (let i = 0; i < 50; i++) {
    stroke(random(255), random(255), random(255));
    let x1 = random(width);
    let y1 = random(height);
    let x2 = x1 + cos(random(TWO_PI)) * 50;
    let y2 = y1 + sin(random(TWO_PI)) * 50;
    line(x1, y1, x2, y2);
  }
}
```
- Lo mismo pero puse que se dibujaran 50 lineas con colores aleatorios.
- Elijo el punto de inicio aleatorio.
- Los puntos finales con cos y sin son aleatorios pero dentro de un rango de 50 pixeles
- Se dibuja cada linea con las coordenadas dadas.

#### Pequeño resumen

1, Con el setup(), creo el lienzo y el botón para cambiar los patrones.
2. drawPattern() limpia el lienzo y elige el patron aleatorio.
3. drawWavePattern() Dibuja ondas usando sin().
4. drawCircleGrid() Dibuja circulos con tamaños y colores aleatorios.
5. DrawRandomLines() Dibuja lineas con colores aleatorios.
