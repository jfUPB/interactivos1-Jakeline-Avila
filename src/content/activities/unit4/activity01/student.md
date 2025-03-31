### Mi solución a la actividad 1

- De generative design: http://www.generative-gestaltung.de/2/sketches/?01_P/P_1_1_1_01
- De p5.js examples: https://p5js.org/examples/math-and-physics-smoke-particle-system/
- Openprocessing: https://openprocessing.org/sketch/2552728

- El ejemplo de generative design:

Este código tiene como objetivo mostrar un espectro de colores generados dinámicamente al mover el mouse.
A medida que el mouse se mueve, se ajustan dos variables (stepX y stepY) que determinan el tamaño de los rectángulos que se dibujan en la pantalla, creando una interacción en tiempo real con el usuario.

Tecnicas de programacion: 

- createCanvas():

Función: createCanvas(width, height)

Descripción: Crea un lienzo (canvas) donde se pueden dibujar gráficos. En este caso, se establece un lienzo de 800 píxeles de ancho y 400 píxeles de alto.

Uso en el código: createCanvas(800, 400);

- noStroke():

Función: noStroke()

Descripción: Desactiva el contorno o borde de las formas que se dibujan (en este caso, los rectángulos). Esto significa que sólo se verá el relleno de los rectángulos sin bordes.

Uso en el código: noStroke();

- colorMode():

Función: colorMode(mode, max1, max2, max3)

Descripción: Define el modo de color en p5.js. El modo de color HSB (Hue, Saturation, Brightness) se utiliza en este código, donde el matiz (Hue) se determina por la posición horizontal (eje X), la saturación (Saturation) se ajusta según la posición vertical (eje Y) y el brillo (Brightness) está fijo en 100.

Uso en el código: colorMode(HSB, width, height, 100);

- fill():

Función: fill(r, g, b) o fill(h, s, b) dependiendo del modo de color.

Descripción: Asigna un color de relleno para las formas que se van a dibujar. En este caso, el color se define usando el sistema HSB, donde gridX controla el matiz (tono del color), height - gridY controla la saturación y 100 es el valor constante de brillo.

Uso en el código: fill(gridX, height - gridY, 100);

- rect():

Función: rect(x, y, width, height)

Descripción: Dibuja un rectángulo en las coordenadas x, y con el tamaño especificado. En este caso, se usa para crear una cuadrícula de rectángulos con un tamaño dinámico basado en el movimiento del mouse.

Uso en el código: rect(gridX, gridY, stepX, stepY);

- mouseX y mouseY:

Descripción: Estas son variables predefinidas en p5.js que contienen las coordenadas actuales del mouse dentro del lienzo. mouseX almacena la posición horizontal y mouseY la posición vertical.

Uso en el código: Se utilizan para ajustar los valores de stepX y stepY, lo que a su vez cambia el tamaño de los rectángulos dibujados.

- keyPressed():

Función: keyPressed()

Descripción: Esta es una función de p5.js que se ejecuta cada vez que una tecla es presionada. En este código, se usa para detectar si la tecla presionada es 's' o 'S' y, en tal caso, guardar la imagen generada.

Uso en el código: if (key == 's' || key == 'S') saveCanvas(gd.timestamp(), 'png');

- saveCanvas():

Función: saveCanvas(filename, extension)

Descripción: Guarda el lienzo actual como una imagen en el formato especificado (en este caso, PNG). Se usa para guardar la imagen cuando el usuario presiona la tecla 'S'.

Uso en el código: saveCanvas(gd.timestamp(), 'png');
