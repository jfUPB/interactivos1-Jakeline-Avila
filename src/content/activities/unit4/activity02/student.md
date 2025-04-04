### Mi solución 2

##### Estructura y Funcionalidad General:
###### Variables globales:

- c: almacena el color actual de la línea o el elemento que se dibuja.

- lineModuleSize: determina el tamaño del módulo (elemento gráfico) que se dibuja.

- angle: almacena el ángulo de rotación del módulo.

- angleSpeed: velocidad a la que rota el módulo.

- lineModule: es un array que contiene los diferentes módulos (imágenes SVG) que se pueden usar para dibujar.

- lineModuleIndex: índice que determina qué módulo se está utilizando actualmente.

- Pre-carga de archivos SVG (función preload()):

- Se cargan 4 imágenes SVG (representadas por los índices 1 a 4 de lineModule) que se pueden usar como "pinceles" para el dibujo. Si no se usan, se dibujan líneas simples.

######## Configuración inicial (función setup()):

- Se crea el lienzo con createCanvas(windowWidth, windowHeight), lo que hace que el tamaño del lienzo se ajuste al tamaño de la ventana del navegador.

- Se configura el fondo en blanco y el grosor de la línea con strokeWeight(0.75).

- Se inicializa el color predeterminado c en un tono de amarillo.

###### Dibujo (función draw()):

- Si el botón izquierdo del ratón está presionado (mouseIsPressed), el código dibuja en la posición actual del ratón (mouseX, mouseY).

- Si se mantiene presionada la tecla Shift, el dibujo se limita a una dirección, es decir, solo puede moverse de manera horizontal o vertical, según la dirección predominante entre el movimiento horizontal y vertical.

- La función rotate(radians(angle)) rota el módulo en función del ángulo acumulado (angle).

- Si lineModuleIndex no es 0 (lo que indica que se usa un SVG como módulo), se dibuja ese SVG con image(lineModule[lineModuleIndex], 0, 0, lineModuleSize, lineModuleSize). Si no, se dibuja una línea simple.

#####Interacción con el mouse:

- Cuando se presiona el ratón, se guarda la posición en clickPosX y clickPosY. Esto ayuda a limitar el movimiento del dibujo si se presiona la tecla Shift.

#####Eventos de teclado:

###### Flechas de dirección:

- UP_ARROW aumenta el tamaño del módulo (lineModuleSize).

- DOWN_ARROW lo reduce.

- LEFT_ARROW disminuye la velocidad de rotación (angleSpeed).

- RIGHT_ARROW aumenta la velocidad de rotación.

######## Teclas para cambiar el color:

- 1 a 4 cambian el color a una de las opciones predefinidas.

- Space genera un color aleatorio con opacidad variable.

###### Teclas para cambiar el módulo (pincel):

- 5 a 9 seleccionan entre diferentes módulos (imágenes SVG) para dibujar.

- Tecla d invierte la dirección de rotación y espejea el ángulo.

- Tecla s guarda el lienzo como una imagen PNG.

- Tecla delete o backspace limpia el lienzo.

###### Función mousePressed():

- Se establece un nuevo tamaño aleatorio para el módulo cada vez que se hace clic.

- Se guarda la posición del clic para limitar el movimiento cuando se usa la tecla Shift.

######## Función keyPressed():

- Permite ajustar el tamaño del módulo y la velocidad de rotación mediante las teclas de flecha, y permite seleccionar el módulo o color que se desea.

########Función keyReleased():

- Se ejecutan diferentes acciones dependiendo de la tecla que se suelta. Por ejemplo, se guarda el lienzo si se presiona la tecla s, o se cambia el color si se presiona space.
