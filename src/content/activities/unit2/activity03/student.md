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

- 

