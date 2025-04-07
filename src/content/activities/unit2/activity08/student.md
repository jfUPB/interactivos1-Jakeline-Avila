<!-- Analizando un Programa con Máquinas de Estados
Enunciado: analiza el siguiente código identificando estados, eventos y acciones. Responde las preguntas planteadas.

from microbit import *
import utime

class Pixel:
    def __init__(self,pixelX,pixelY,initState,interval):
        self.state = "Init"
        self.startTime = 0
        self.interval = interval
        self.pixelX = pixelX
        self.pixelY = pixelY
        self.pixelState = initState

    def update(self):

        if self.state == "Init":
            self.startTime = utime.ticks_ms()
            self.state = "WaitTimeout"
            display.set_pixel(self.pixelX,self.pixelY,self.pixelState)

        elif self.state == "WaitTimeout":
            if utime.ticks_diff(utime.ticks_ms(),self.startTime) > self.interval:
                self.startTime = utime.ticks_ms()
                if self.pixelState == 9:
                    self.pixelState = 0
                else:
                    self.pixelState = 9
                display.set_pixel(self.pixelX,self.pixelY,self.pixelState)

pixel1 = Pixel(0,0,0,1000)
pixel2 = Pixel(4,4,0,500)

while True:
    pixel1.update()
    pixel2.update()
Describe detalladamente cómo funciona este ejemplo. Usa chatGPT para indagar y profundizar en todas las dudas que puedas tener.
Del contexto del programa ¿Puedes identificar algún estado? Los estados son momentos del programa en los cuales este espera a que ocurra algo.
Del contexto del programa ¿Puedes identificar algún evento? Los eventos son aquellas cosas por las que el programa pregunta durante un estado.
Del contexto del programa ¿Puedes identificar alguna acción? Las acciones son aquellas cosas que el programa ejecuta en respuesta a la ocurrencia de un evento.
Entrega: respuesta a las preguntas.-->

### Mi solución a la actividad 8

``` js
class Pixel: // Definición de la Clase Pixel:


    def __init__(self,pixelX,pixelY,initState,interval): 
        self.state = "Init" // state: Es el estado interno de la máquina de estados. Inicialmente se establece en "Init".
        self.startTime = 0 // startTime: Almacena el tiempo (en milisegundos) en el que se inicia una acción o espera.
        self.interval = interval // interval: Es el tiempo en milisegundos que el pixel debe esperar antes de cambiar su estado.
        self.pixelX = pixelX // pixelX y pixelY: Determinan la posición del pixel en la matriz LED.
        self.pixelY = pixelY
        self.pixelState = initState  // pixelState: Representa el brillo del pixel (por ejemplo, 0 para apagado y 9 para encendido).

    def update(self):

        if self.state == "Init":
            self.startTime = utime.ticks_ms()
            self.state = "WaitTimeout"
            display.set_pixel(self.pixelX,self.pixelY,self.pixelState)

        elif self.state == "WaitTimeout":
            if utime.ticks_diff(utime.ticks_ms(),self.startTime) > self.interval:
                self.startTime = utime.ticks_ms()
                if self.pixelState == 9:
                    self.pixelState = 0
                else:
                    self.pixelState = 9
                display.set_pixel(self.pixelX,self.pixelY,self.pixelState)

/* Método update:
Este método es el corazón de la máquina de estados y se encarga de actualizar el comportamiento del pixel según su estado actual:

Estado "Init":
Acción:
Se registra el tiempo actual usando utime.ticks_ms() en startTime.
Se establece el brillo del pixel en la posición definida utilizando display.set_pixel(pixelX, pixelY, pixelState).
Transición: Cambia el estado a "WaitTimeout".
Estado "WaitTimeout":
Evento:
Se verifica continuamente si ha transcurrido el intervalo especificado:
python
Copiar
Editar
if utime.ticks_diff(utime.ticks_ms(), self.startTime) > self.interval:
Este chequeo es el evento clave, ya que espera a que el tiempo transcurrido supere el valor de interval.
Acción en respuesta al evento:
Se actualiza startTime con el tiempo actual para reiniciar la cuenta del intervalo.
Se alterna el pixelState: si estaba en 9 (encendido) se cambia a 0 (apagado) y viceversa.
Se actualiza la pantalla con el nuevo estado del pixel usando display.set_pixel.
Instanciación y Ciclo Principal:

Se crean dos objetos Pixel:
pixel1 en la posición (0,0) con un intervalo de 1000 ms (1 segundo).
pixel2 en la posición (4,4) con un intervalo de 500 ms (0.5 segundos).
El ciclo while True llama continuamente al método update() de cada pixel, lo que permite que cada uno
 actualice su estado de forma independiente según su propio intervalo. */

pixel1 = Pixel(0,0,0,1000)
pixel2 = Pixel(4,4,0,500)

while True:
    pixel1.update()
    pixel2.update()

```

- Estados:

"Init": Es el estado inicial en el que el pixel configura su tiempo de inicio y establece su primer estado visual en la pantalla.
Espera: No espera un evento externo, sino que realiza una acción de inicialización.
"WaitTimeout": En este estado, el pixel está esperando que transcurra el tiempo definido en interval para cambiar su brillo.
Espera: Se espera que ocurra el evento del "timeout" (el paso del tiempo suficiente).

- Eventos:

Timeout (paso del tiempo):
Dónde se detecta: Dentro del estado "WaitTimeout".
Cómo se detecta: Al comprobar si la diferencia de tiempo (utime.ticks_diff(utime.ticks_ms(), self.startTime)) es mayor que el interval.
Función: Este evento indica que ha pasado el tiempo suficiente para realizar el cambio de estado del pixel.

- Acciones:

En el estado "Init":
Registrar el tiempo inicial (startTime).
Establecer el brillo inicial del pixel en la pantalla.
En el estado "WaitTimeout" al ocurrir el evento (timeout):
Actualizar el tiempo: Se vuelve a asignar startTime con el tiempo actual para empezar una nueva cuenta.
Alternar el estado del pixel: Cambiar el brillo de 9 a 0 o de 0 a 9.
Actualizar la pantalla: Se llama a display.set_pixel para reflejar el cambio en el hardware.
