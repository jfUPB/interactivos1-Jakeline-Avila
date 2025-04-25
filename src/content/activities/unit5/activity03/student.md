### Mi solución a la actividad 3

- En la unidad anterior, cuando el micro:bit enviaba datos en formato de texto (ASCII), era necesario usar separadores (como comas) y un delimitador de fin de línea (\n) para poder identificar claramente:

Dónde termina un dato y empieza el siguiente.

Dónde termina un mensaje completo.

- Por ejemplo, la línea:

``` js
500,524,True,False\n

```
necesitaba separadores porque:

Los datos tienen tamaños variables (por ejemplo, 5, 50, 500, -134, etc.).

Se envían como texto plano, no como bytes binarios con tamaño fijo.

El salto de línea (\n) indicaba el fin de cada paquete de datos, lo que facilitaba leer cada línea como un mensaje completo.

- Ahora, en formato binario, ya no es necesario usar delimitadores porque:

Cada dato tiene un tamaño fijo:

xValue: 2 bytes

yValue: 2 bytes

aState: 1 byte

bState: 1 byte

El total es siempre de 6 bytes exactos por mensaje, así que ya no hace falta un carácter especial para marcar el final.

El receptor (p5.js) puede simplemente leer 6 bytes a la vez, sabiendo que cada grupo es un paquete completo.

Esto reduce el tamaño de cada mensaje, mejora la velocidad de procesamiento y evita errores por malformación del texto o saltos de línea faltantes.

####  Compara el código de la unidad anterior con este. ¿Qué cambios ves?

En la unidad anterior, los datos llegaban como texto (ASCII), y el código tenía que:

Leer los datos como una línea de texto completa, usando port.readUntil("\n").

Separar los valores con split(",").

Convertir cada string en número con int() y comprobar si los valores booleanos estaban en formato "true" o "false".

Ejemplo:

``` js

let data = port.readUntil("\n");
let values = data.split(",");
microBitX = int(values[0]) + windowWidth / 2;
microBitY = int(values[1]) + windowHeight / 2;
microBitAState = values[2].toLowerCase() === "true";
microBitBState = values[3].toLowerCase() === "true";
```
En el código actual, al recibir datos en formato binario, ya no se necesita manipular strings ni hacer separaciones manuales. Los cambios más importantes son:

Se usa port.readBytes(6) para leer directamente 6 bytes del puerto serial.

Los datos se interpretan con un DataView, que permite leer tipos específicos desde un buffer binario:

getInt16(offset) para los enteros con signo de 2 bytes (x, y).

getUint8(offset) para los valores de 1 byte (botones A y B).

No se usa ningún split() ni conversión de texto.

Ejemplo:

``` js

let data = port.readBytes(6);
const view = new DataView(new Uint8Array(data).buffer);
microBitX = view.getInt16(0);
microBitY = view.getInt16(2);
microBitAState = view.getUint8(4) === 1;
microBitBState = view.getUint8(5) === 1;

```
#### ¿Qué ves en la consola? ¿Por qué crees que se produce este error?

Este error se debe a un problema de sincronización entre el micro:bit y p5.js. Como los datos se están enviando de forma continua y en binario sin ningún tipo de delimitador o encabezado:

- Si p5.js pierde un byte por cualquier razón (lag, interferencia, interrupción del puerto, etc.), ya no sabrá dónde comienza el siguiente paquete.

- Y como no hay un marcador o “inicio de paquete”, la lectura sigue en el lugar incorrecto, desfasando todos los datos siguientes.

- Esto se llama desalineación de bytes.

### Que se ve en la consola despues de implementar los cambios?

![image](https://github.com/user-attachments/assets/4c022f60-ce59-4512-a366-6ca984c7e4c4)

### Cambios en el programa

Cambios en los programas

- micro:bit
Se añadió un header (0xAA) al inicio de cada paquete de datos. Este byte actúa como un marcador claro de comienzo de paquete.

Se añadió un checksum (suma de los bytes de los datos módulo 256) al final del paquete, para verificar que los datos lleguen completos y sin errores.

El paquete ahora tiene esta estructura fija de 8 bytes:
HEADER (1) + DATA (6) + CHECKSUM (1) = 8 bytes

- p5.js
Se creó un buffer de recepción (serialBuffer) para acumular y procesar los bytes de manera más controlada.

Se agregó una función readSerialData() que:

Busca el byte 0xAA para encontrar el inicio del paquete.

Verifica que haya al menos 8 bytes disponibles antes de procesar.

Calcula y compara el checksum para validar el paquete.

Ignora paquetes corruptos o incompletos.

Con estos cambios, se garantiza la sincronización y la integridad de los datos, incluso si se pierden bytes o llegan fuera de orden.

👀 ¿Qué se observa en la consola de p5.js?
Se muestran mensajes como:

``` python
Microbit ready to draw
A pressed
B released

```
Y también las lecturas limpias y consistentes de los valores:


``` js
microBitX: 512 microBitY: 503 microBitAState: true microBitBState: false
microBitX: 514 microBitY: 504 microBitAState: false microBitBState: false

```
Si en algún momento un paquete no pasa el chequeo de integridad, puede aparecer:

```
Checksum error in packet

```
Esto indica que el sistema está detectando errores y los está manejando correctamente sin colapsar o mostrar datos erróneos.
