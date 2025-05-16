### Mi solucion a la actividad 4

- Compara el código original del caso de estudio con el anterior. ¿Qué notas de diferente?

R/= Al comparar el código original del micro:bit en Python con la estructura del nuevo proyecto en p5.js, se observan varias diferencias significativas. El código en Python se ejecuta directamente en el micro:bit 
y se encarga de recoger datos de los sensores integrados (como el acelerómetro y los botones) para luego enviarlos por el puerto serial utilizando uart.write(). En cambio, el proyecto en p5.js está diseñado para correr 
en un navegador web, utilizando JavaScript junto con la librería p5.js para la visualización gráfica y la librería p5.webserial.js para establecer la comunicación serial desde el navegador con el micro:bit. Mientras que 
el código en Python se enfoca en la recolección y transmisión de datos, el entorno p5.js está orientado a la recepción de esos datos y su representación visual o interacción en la interfaz web. Además, el código HTML 
vincula los archivos necesarios para que el navegador pueda conectarse al micro:bit, 
lo cual no es necesario en el código que corre directamente en el dispositivo.

- Ejecuta el sketch. Si no tienes errores podrás continuar. Reflexiona ¿Para qué se usan estas imágenes? ¿Qué representan? Revisa de nuevo el sketch original y analiza.

R/= Estas imágenes pueden funcionar como un sistema de estados visuales: si el micro:bit se inclina hacia un lado, se podría mostrar la imagen 03.svg; si se presiona un botón, se podría cambiar a 04.svg, etc. Es decir, representan visualmente los datos del sensor en tiempo real de manera gráfica y llamativa. Estas imágenes SVG están precargadas en el sketch mediante la función preload(), lo que indica que son elementos gráficos esenciales para la visualización. 

- Para la bitacora: En este proyecto, la aplicación se comporta de forma distinta dependiendo de su estado. Por eso usamos una máquina de estados: una estructura de control que permite dividir el comportamiento del programa según situaciones claramente definidas. En este caso, los estados son:

WAIT_MICROBIT_CONNECTION: cuando aún no se ha conectado el micro:bit.

RUNNING: cuando ya se ha conectado, y se están recibiendo datos para usarlos.

Antes, en otros proyectos, había un pseudoestado INIT, que se encargaba de preparar la aplicación. En este caso, ese rol lo cumple la función setup(), ya que se ejecuta una sola vez al inicio del sketch. Por eso decimos que setup() actúa como ese pseudoestado INIT.

- ¿Qué pasaría ahora si das click al botón?

R/= Cuando presionás el botón "Connect to micro:bit":

La función connectBtnClick() se ejecuta gracias a mousePressed(connectBtnClick);.

Esa función evalúa si el puerto serial ya está abierto con port.opened().

Si NO está abierto, entonces:

Se abre el puerto serial usando el nombre "MicroPython" y una velocidad de 115200 baudios:
port.open("MicroPython", 115200);

Esto inicia la comunicación entre tu navegador y el micro:bit.

Si ya está abierto, se cierra la conexión con port.close();.

- Nota que solo se leen datos del micro:bit si el puerto está abierto ¿Por qué? ¿Podrías leer datos si el puerto está cerrado? ¿Qué pasaría si el puerto está cerrado y el micro:bit envía datos?
R/= Se leen datos del micro:bit únicamente si el puerto está abierto porque este actúa como un canal de comunicación; si está cerrado, simplemente no existe conexión entre el micro:bit y la aplicación, por lo tanto no es posible recibir datos. Intentar leer con el puerto cerrado no generará errores visibles si se verifica correctamente, pero tampoco se obtendrá información, ya que el navegador no puede acceder a lo que el micro:bit está enviando. Aunque el micro:bit continúe transmitiendo datos cada 100 ms, estos se pierden si no hay un puerto abierto escuchando, lo que significa que el micro:bit está enviando información “al vacío” sin un receptor disponible para procesarla.

- Cuando el micro:bit envía datos, lo hace siguiendo un protocolo: separa los valores con comas y finaliza cada paquete con un salto de línea (\n). Este formato es fundamental para que la aplicación en p5.js pueda interpretar correctamente los datos. En la aplicación, primero se verifica que haya datos disponibles con port.availableBytes() > 0, y luego se espera a recibir una línea completa con port.readUntil("\n"). Si el micro:bit no enviara ese \n, la aplicación quedaría esperando indefinidamente, sin saber cuándo termina el paquete. Una vez recibido, el dato se limpia con .trim() y se separa en partes con split(","), generando un arreglo con los cuatro valores esperados. Se verifica que efectivamente se hayan recibido los cuatro datos con values.length == 4, asegurando la integridad del paquete. Como todos los datos llegan como texto (codificado en ASCII), es necesario convertir los valores x e y en enteros, y los estados de los botones A y B en booleanos. Finalmente, se suma windowWidth/2 y windowHeight/2 a x e y respectivamente para centrar el punto de origen en la mitad de la pantalla, lo que facilita representar el movimiento del micro:bit respecto al centro del lienzo y no desde la esquina superior izquierda.

- La función updateButtonStates se encarga de detectar los cambios en el estado de los botones A y B del micro:bit para generar acciones específicas, como si se tratara de eventos de teclado. Por ejemplo, cuando el botón A pasa de no estar presionado a estarlo, se interpreta como un evento de "keypressed", y cuando el botón B pasa de estar presionado a no estarlo, se considera un "keyreleased". Para lograr esto es necesario almacenar el estado anterior de cada botón, ya que de lo contrario la aplicación no podría detectar el cambio de estado, y por lo tanto se ejecutaría el evento de forma continua en cada frame (aunque el botón siga presionado), o directamente no se distinguiría si se trató de una pulsación o simplemente de un estado mantenido. Al comparar el estado actual con el anterior (prevmicroBitAState y prevmicroBitBState), el algoritmo puede saber con precisión cuándo ocurre un cambio y actuar en consecuencia, garantizando que cada acción asociada a un evento se dispare solo una vez por transición.

- Comparando el código original con esta nueva versión, se observa un cambio importante en la estructura del flujo de la aplicación, que ahora está controlado por una máquina de estados. Antes, todos los eventos del teclado, mouse y la lógica de dibujo estaban activos desde el inicio. En cambio, ahora la aplicación espera explícitamente a que se conecte el micro:bit para pasar al estado RUNNING, asegurando así que no se realicen dibujos sin datos válidos. Además, algunos eventos del mouse como clics o movimiento fueron eliminados o desactivados, ya que ahora la interacción se basa completamente en los datos que vienen del micro:bit (como los botones A y B) y no en acciones del usuario en la pantalla. También desapareció la funcionalidad que antes se vinculaba con la barra espaciadora, posiblemente porque el control ahora lo asume el botón A del micro:bit, que genera un comportamiento equivalente al iniciar un trazo desde una posición específica. Todo esto hace que el sistema sea más autónomo y dependa directamente del dispositivo conectado, en lugar de requerir acciones manuales locales.

- El mensaje "No se están recibiendo 4 datos del micro:bit" se genera cuando el número de datos recibidos no coincide con el esperado (en este caso, 4 datos). Este mensaje indica que la función que lee los datos del micro:bit está recibiendo algo que no sigue el formato esperado o que el paquete de datos no está completo.

Analizando por qué este mensaje aparece en la consola y en qué circunstancias:

¿Por qué aparece el mensaje?

El mensaje aparece cuando el data.split(",") no devuelve un arreglo con exactamente 4 elementos. Si hay menos de 4 elementos o más, el programa detecta que la cantidad de datos no es correcta.

Este tipo de situación puede ocurrir si el micro:bit está enviando datos incompletos o si hay problemas en la transmisión de los datos, como interferencias o problemas con el puerto serial.

¿Ocurre en varios frames o solo en uno?

Este mensaje probablemente aparece solo una vez en un frame específico cuando los datos recibidos no tienen el formato esperado. En el siguiente frame, si los datos llegan correctamente, el mensaje no se volvería a mostrar.

Si el micro:bit está enviando datos con frecuencia, el mensaje aparecería cada vez que los datos estén incompletos (por ejemplo, si el micro:bit no está enviando el retorno de línea \n correctamente, o si envía datos incompletos en algún momento).

¿Qué se puede hacer para solucionar este problema?

Asegurarse de que el micro:bit siempre envíe datos completos: Es fundamental garantizar que el micro:bit envíe los 4 datos correctos en cada paquete y que cada paquete termine con un \n. Si el micro:bit falla en enviar el retorno de línea, readUntil("\n") no podrá capturar todo el paquete de datos.

Verificar la conexión y la transmisión de datos: Asegúrate de que la conexión serial esté funcionando correctamente, ya que un puerto serial interrumpido o inestable podría provocar que los datos lleguen de manera incompleta.

Incluir un manejo de errores más robusto: Puedes agregar un mecanismo de reintento o espera en caso de que los datos no sean los esperados. Por ejemplo, en lugar de imprimir un mensaje de error cada vez que los datos no sean correctos, podrías almacenar los datos incompletos y esperar a recibir el paquete completo en el siguiente ciclo.

Depurar los datos recibidos: Para obtener más detalles, se podría añadir un print de los datos recibidos para asegurarse de qué está enviando el micro:bit. Esto también puede ayudar a detectar si hay algún patrón en los datos incompletos.

Sincronizar la lectura de los datos: Podrías modificar la aplicación para que lea solo una línea completa a la vez. Si el paquete recibido no es completo (es decir, no contiene los 4 valores), la aplicación podría seguir esperando hasta que reciba los 4 datos completos. Esto podría evitar que el sistema intente procesar datos incompletos.

En resumen, este problema puede ser debido a la falta de datos completos o a problemas de sincronización en la transmisión. Dependiendo de las circunstancias, las soluciones podrían ser asegurarse de que el micro:bit envíe los datos completos y manejar de manera más robusta los errores de recepción de datos.




