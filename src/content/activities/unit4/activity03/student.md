### Mi solucion a la actividad 3

Descripción general del código
El código proporciona una forma de transmitir información desde un micro:bit a través del puerto serie utilizando la interfaz UART. A continuación, vamos a desglosar y analizar cada sección del código:

``` js
from microbit import *  # Importa todas las funciones necesarias del micro:bit

uart.init(115200)  # Inicializa la comunicación serial UART con una velocidad de 115200 baudios
display.set_pixel(0, 0, 9)  # Enciende el píxel en la posición (0, 0) en el display LED del micro:bit

while True:
    xValue = accelerometer.get_x()  # Lee el valor del acelerómetro en el eje X
    yValue = accelerometer.get_y()  # Lee el valor del acelerómetro en el eje Y
    aState = button_a.is_pressed()  # Verifica si el botón A está presionado
    bState = button_b.is_pressed()  # Verifica si el botón B está presionado
    data = "{},{},{},{}\n".format(xValue, yValue, aState, bState)  # Formatea los datos como una cadena
    uart.write(data)  # Escribe los datos en el puerto serial
    sleep(100)  # Pausa de 100 ms (envía datos a 10 Hz)
```
Este código está diseñado para transmitir constantemente información sobre el estado del acelerómetro (valores de los ejes X y Y) y el estado de los botones A y B del micro:bit a través de UART. El micro:bit envía estos datos a un receptor serial, como un ordenador o una aplicación que lea los datos de ese puerto (por ejemplo, SerialTerminal).

2. ¿Qué información se está enviando?
El código envía los siguientes datos:

xValue: Valor del eje X del acelerómetro.

yValue: Valor del eje Y del acelerómetro.

aState: Estado del botón A (True si está presionado, False si no lo está).

bState: Estado del botón B (True si está presionado, False si no lo está).

La información se envía en un formato de texto con las variables separadas por comas. Cada línea termina con un salto de línea \n.

3. Formato de los datos:
La parte del código que genera la cadena de datos es:

``` js
data = "{},{},{},{}\n".format(xValue, yValue, aState, bState)
```
Esta línea crea una cadena de texto donde las variables se insertan en el formato indicado:

{}: Especifica dónde se insertarán los valores de las variables xValue, yValue, aState, y bState.

Las comas (,) actúan como delimitadores entre los valores.

El \n al final de la cadena es un salto de línea, que indica el final de una transmisión de datos.

Por ejemplo, si xValue = 969, yValue = 652, aState = True, y bState = False, el string generado sería:

```
"969,652,True,False\n"
```
4. ¿Por qué se separan los datos con comas y se termina con un salto de línea?
Comas: Se usan para separar los valores de forma clara y estructurada, permitiendo que un programa que reciba estos datos pueda descomponer la cadena en componentes individuales más fácilmente.

Salto de línea (\n): Esto indica el final de una transmisión de datos. Es común en protocolos de comunicación para marcar el fin de un mensaje o una línea de datos, permitiendo que el receptor sepa cuándo ha llegado el final de los datos y pueda procesarlos.

Si no se separan con comas ni se termina con un salto de línea, el receptor podría no ser capaz de interpretar correctamente los datos. La falta de separación dificultaría la identificación de cada valor individual y la ausencia de salto de línea dificultaría la distinción de un conjunto de datos completo.

5. Función sleep(100)
La función sleep(100) hace que el micro:bit espere 100 milisegundos antes de continuar con la siguiente iteración del ciclo while. Esto limita la frecuencia de envío de datos a 10 Hz (es decir, se envían datos una vez cada 100 milisegundos).

Si no se usara sleep(100), el micro:bit enviaría datos de forma continua y muy rápida, lo que podría saturar el puerto serie o hacer que el receptor no pueda procesar todos los datos a tiempo.

6. Valores de xValue y yValue cuando el micro:bit se inclina
El acelerómetro del micro:bit devuelve valores en un rango de -1024 a 1024 para los ejes X, Y y Z. Los valores se relacionan con la inclinación del dispositivo:

Cuando el micro:bit está plano sobre una superficie, los valores de xValue y yValue son cercanos a 0.

Si el micro:bit se inclina hacia la izquierda, el valor de xValue disminuye.

Si el micro:bit se inclina hacia la derecha, el valor de xValue aumenta.

Si el micro:bit se inclina hacia adelante, el valor de yValue disminuye.

Si el micro:bit se inclina hacia atrás, el valor de yValue aumenta.

7. Valores de aState y bState
aState: Si el botón A está presionado, aState es True. Si no está presionado, aState es False.

bState: Si el botón B está presionado, bState es True. Si no está presionado, bState es False.

8. Diferencia entre is_pressed() y was_pressed()
is_pressed(): Devuelve True si el botón está presionado en el momento actual.

was_pressed(): Devuelve True solo si el botón ha sido presionado en el ciclo anterior (es decir, en el último tiempo de ejecución del código).

9. Envío de bytes en el puerto serial
Si los valores que se están enviando son:

xValue = 969

yValue = 652

aState = True

bState = False

El formato generado sería:

```
"969,652,True,False\n"
```
Esto se enviará como una secuencia de caracteres, donde cada número y cada valor de estado se representa en ASCII. Los caracteres son enviados por el puerto serial como bytes en su formato hexadecimal.




