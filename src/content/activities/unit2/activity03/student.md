<!-- Explorando el Micro:bit y Micropython
Enunciado: investiga las características del micro:bit y las funciones básicas de Micropython para controlarlo. Consulta en los recursos.

Recursos:

Características del micro:bit
Referencia
Entrega: en tu bitácora indica

Al menos 4 entradas y 4 salidas del micro:bit con su descripción.
Descripción de al menos 3 funciones de Micropython para entradas y 3 para salidas, con ejemplos de código sencillos. -->

### Mi solución a la actividad 3

#### Entradas:

- Botón A y B: El micro:bit tiene dos botones en la parte frontal. El botón A y el botón B se pueden utilizar como entradas para hacer que sucedan cosas en tu programa cuando se pulsan

``` js
from microbit import *



while True:
    

    if button_a.was_pressed():
        display.scroll('A')


    if button_b.was_pressed():
        display.scroll('B')

```

- Acelerómetro: El acelerómetro del micro:bit mide las fuerzas en 3 dimensiones, incluida la gravedad, para que el programa puedan saber en qué dirección está tu micro:bit. Está en la parte trasera del micro:bit.

``` js
from microbit import *



while True:
    

    if accelerometer.was_gesture('shake'):
        display.show(Image.CONFUSED)
```

- Sensor de luz: El micro:bit es capaz de usar la matriz de LEDs para detectar la cantidad de luz ambiental que hay.

``` js
from microbit import *



while True:
    
    display.scroll(display.read_light_level())

```
- Los pines táctiles: Hay bastantes pines tactiles que pueden detectar toques o conexiones con otros dispositivos.

``` js

from microbit import *



while True:
    
  

    if pin0.is_touched():
        display.show(0)
```

#### Salidas:

- Matriz de LEDs (5x5): Pantalla de 25 LEDs que permite mostrar imágenes, números o texto.

``` js
from microbit import *


while True:
    
    display.scroll('JOHANNY Y JEINNY')    
   
```

- Speaker: Genera sonidos y melodías mediante programación.

``` js
from microbit import *
import music




while True:
    
    music.play(music.BA_DING)
```

- Salida en serie: Se pueden enviar mensajes al pc mediante la conexión USB.

``` js

from microbit import *



while True:
    
   print("Hola desde Micro:bit")
```
- Los pines de salida: Permiten controlar dispositivos externos como LEDs motores o sensores.

``` js
from microbit import *





while True:
    
    if pin1.is_touched():
        display.show(1)
```

#### Funciones de entrada:


1. button_a.is_pressed() y button_b.is_pressed()

Descripción: Detecta si se ha presionado un botón.



2. accelerometer.get_x(), get_y(), get_z()

Descripción: Obtiene la aceleración en cada eje.



3. pinX.is_touched()

Descripción: Detecta si un pin ha sido tocado.





---

#### Funciones para Salidas

1. display.show()

Descripción: Muestra texto o imágenes en la matriz de LEDs.



2. music.play()

Descripción: Reproduce una melodía o tono.



3. pinX.write_digital(1) y pinX.write_digital(0)

Descripción: Envía una señal digital (1 o 0) a un pin.
