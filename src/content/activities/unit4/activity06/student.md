### Mi solución a la actividad 5

- ¿Qué es un protocolo de comunicación y por qué es importante en la comunicación serial?

Creo que un protocolo de comunicación es básicamente un conjunto de reglas que permiten que dos dispositivos se entiendan entre sí. En la comunicación serial, eso es clave porque sin esas reglas, los datos pueden no ser interpretados correctamente, y no sabríamos cómo enviarlos o recibirlos. En mi código, se usan reglas como los datos separados por comas y un salto de línea al final para que el receptor sepa cuándo ha terminado un mensaje:

``` js
let data = port.readUntil("\n");
```
Esta línea ayuda a saber cuándo los datos del micro:bit han terminado y están listos para ser procesados.

- ¿Por qué se separan los datos con comas en el protocolo ASCII que exploramos?

Yo diría que las comas se usan como un delimitador porque es una manera fácil de separar los datos sin complicarse mucho. Es como si estuviéramos diciendo "aquí termina una cosa, y empieza otra". Así, cuando los datos llegan al receptor, sabe exactamente qué corresponde a cada valor. En el código, usamos split() para dividir la cadena por comas:

``` js
let values = data.split(",");
```
Esto permite separar valores como las coordenadas x y y del micro:bit.

- ¿Por qué es necesario terminar los datos con un carácter que marque el fin del mensaje?

Creo que terminar los datos con algo como un salto de línea es importante porque indica que ya no hay más datos para leer. Sin esto, el receptor no sabría cuándo parar, y podría intentar leer algo que no existe o quedar confundido. En el código, usamos \n como el delimitador al final del mensaje:
``` python
data = "{},{},{},{}\n".format(xValue, yValue, aState, bState)
```
El salto de línea ayuda a marcar el final de la cadena de datos.

- ¿Por qué fue necesario usar una máquina de estados en la aplicación modificada de p5.js?

En la aplicación, usé una máquina de estados para poder manejar diferentes situaciones de la aplicación, como esperar la conexión del micro:bit y luego cambiar a "ejecutando" cuando ya estaba conectado. Lo que me gusta de esto es que puedes manejar cada parte de la aplicación de manera organizada y no todo se mezcla en un solo bloque de código. Es más fácil de controlar.

``` js
const STATES = {
  WAIT_MICROBIT_CONNECTION: "WAIT_MICROBIT_CONNECTION",
  RUNNING: "RUNNING",
};
``` 
Esto me ayuda a organizar el flujo entre los diferentes estados de la aplicación.

- ¿Cómo se formatean los datos en el micro:bit para ser enviados por el puerto serial?

En el micro:bit, los datos se formatean como una cadena de texto que tiene valores separados por comas. Por ejemplo, el micro:bit envía algo como "123,756,False,True\n". Eso facilita mucho el trabajo al receptor porque cada valor se puede separar por las comas y saber qué significa cada uno.

``` python
data = "{},{},{},{}\n".format(xValue, yValue, aState, bState)
```
Este es el formato con el que se envían los datos desde el micro:bit.

- ¿Qué significa que los datos enviados por el micro:bit están codificados en ASCII?

Cuando dicen que los datos están codificados en ASCII, lo que entiendo es que cada carácter que se envía (como los números, las letras o las comas) tiene un valor específico en la tabla ASCII. Entonces, el receptor sabe cómo interpretar esos caracteres porque están en un formato estándar.

- ¿Por qué es necesario en la aplicación de p5.js preguntar si hay bytes disponibles en el puerto serial antes de leerlos?

Creo que es necesario hacer esa verificación porque si no, podríamos intentar leer cuando no hay datos disponibles y eso podría generar errores o devolver valores vacíos, lo cual no serviría. Al preguntar si hay bytes disponibles, nos aseguramos de que solo intentamos leer cuando realmente hay datos para recibir. En el código lo hacemos así:

``` js
if (port.availableBytes() > 0) {
  let data = port.readUntil("\n");
}
```
Esto verifica que haya datos antes de intentar leerlos.

- ¿Qué pasa si esto no se hace?

Si no verificamos si hay datos disponibles y tratamos de leer de todas formas, el programa podría leer información incorrecta o no leer nada. Eso haría que el programa no funcione bien.

- ¿Cómo se elimina el retorno de carro o salto de línea de un string en p5.js?

Para eliminar el salto de línea o retorno de carro, usé el método .trim() en p5.js. Este método elimina los espacios y saltos de línea al principio y al final de la cadena, lo cual es útil cuando recibimos datos del micro:bit y no queremos que esos saltos de línea interfieran con el procesamiento.

``` js
data = data.trim();
```
Este método limpia la cadena antes de que la procesemos.

- Si una cadena tiene información separada por espacios y quieres dividir dicha información en varias cadenas individuales, ¿Qué función de p5.js usarías?

Usaría split() en p5.js. Este método te permite dividir una cadena en un arreglo de subcadenas, usando un delimitador, como un espacio.

``` js
let values = data.split(",");
```
Esto me permite dividir los datos recibidos en valores individuales que luego puedo usar.

- ¿Por qué es necesario en la aplicación del caso de estudio convertir las cadenas a números enteros antes de usarlas en el sketch de p5.js?

Es necesario convertir las cadenas a números enteros porque si no lo hacemos, esos valores serían tratados como texto, y no podríamos hacer cálculos o comparaciones con ellos. Necesitamos esos valores como números para poder usarlos en las funciones de dibujo o cálculos matemáticos.

``` js
let microBitX = int(values[0]) + windowWidth / 2;
let microBitY = int(values[1]) + windowHeight / 2;
```
Esto convierte los valores de las coordenadas en enteros, que es lo que necesito para dibujar en el canvas.

- Si el micro:bit tiene los siguientes datos xValue: 123, yValue: 756, aState: False, bState: True, ¿Qué bytes se enviarían por el puerto serial?

Si el micro:bit tiene esos valores, el mensaje que se enviaría por el puerto serial sería algo como:

```
"123,756,False,True\n"
```
Cada valor se separa por comas, y al final hay un salto de línea que indica el final del mensaje.

- ¿Qué aprendiste nuevo del micro:bit que no sabías antes?

Antes no sabía cómo enviar datos de un micro:bit a un navegador web usando comunicación serial. Aprendí que es bastante fácil, y que puedo enviar información como valores del acelerómetro o el estado de los botones para interactuar con una aplicación en tiempo real.

- ¿Qué aprendiste nuevo de p5.js que no sabías antes?

Aprendí a usar la Web Serial API en p5.js para conectarme a dispositivos externos como el micro:bit. También aprendí a manejar los datos que vienen del puerto serial y a usarlos en tiempo real para crear gráficos interactivos. ¡Eso fue algo bastante nuevo para mí!
