### Mi solución a la actividad 4

#### Experimento 1: Detectar la Presión del Botón A

#### ¿Qué quería comprobar?

- Quería verificar si el Micro:bit puede detectar cuando el botón A está presionado y si se mantiene la detección mientras el botón siga presionado.

#### Hipótesis

- Si uso la función button_a.is_pressed(), el Micro:bit debería mostrar un mensaje solo cuando el botón A esté presionado y debería desaparecer cuando se suelte.

``` js
from microbit import *

while True:
    if button_a.is_pressed():
        display.show("A")
    else:
        display.clear()

```

#### Descripción de los resultados

- Al presionar el botón A, la pantalla muestra la letra "A".

- Cuando suelto el botón, la pantalla se borra.


#### Análisis de los resultados

- El resultado confirma que button_a.is_pressed() devuelve True solo mientras el botón A esté presionado. Si no está presionado, devuelve False, lo que hace que la pantalla se borre.

#### Conclusión

- La función button_a.is_pressed() es útil para detectar presiones momentáneas en el botón A, pero no registra si se presionó y luego se soltó.


---

#### Experimento 2: Detectar el Botón B y Mostrar un Contador

#### ¿Qué quería comprobar?

- Quería ver si el Micro:bit puede contar cuántas veces se ha presionado el botón B y mostrar ese número en la pantalla.

#### Hipótesis

- Si incremento una variable cada vez que el botón B es presionado y muestro su valor, el Micro:bit debería llevar un conteo de las veces que se ha presionado el botón B.

``` js

from microbit import *

contador = 0

while True:
    if button_b.was_pressed():
        contador += 1
        display.show(contador)
```

#### Descripción de los resultados

- Cada vez que presiono el botón B, el número en la pantalla aumenta en 1.

- La pantalla muestra el último valor contado hasta que presione B nuevamente.


#### Análisis de los resultados

- A diferencia de is_pressed(), la función was_pressed() detecta si el botón fue presionado y luego soltado. Esto permite contar eventos sin que el número aumente rápidamente mientras el botón está presionado.

#### Conclusión

- was_pressed() es útil para registrar eventos únicos de presionados de botón, en lugar de detectar presiones continuas.


---

#### Experimento 3: Detectar la Presión del Botón Virtual del Logo

#### ¿Qué quería comprobar?

- Quería saber si el Micro:bit puede detectar la presión del botón virtual del logo y responder mostrando un ícono en la pantalla.

#### Hipótesis

- Si uso pin_logo.is_touched(), la pantalla debería mostrar un corazón cuando toco el logo y desaparecer cuando lo suelte.

``` js

from microbit import *

while True:
    if pin_logo.is_touched():
        display.show(Image.HEART)
    else:
        display.clear()

```

#### Descripción de los resultados

- Cuando toco el logo, aparece un corazón en la pantalla.

- Cuando lo suelto, la pantalla se borra.


#### Análisis de los resultados

- El logo del Micro:bit funciona como un sensor táctil. pin_logo.is_touched() devuelve True solo mientras se toca el logo, similar a is_pressed() en los botones físicos.

#### Conclusión

El logo actúa como una entrada táctil digital. Puede usarse como un botón adicional en programas que necesiten más entradas sin usar pines físicos.


---

#### Conclusión General

- Los botones A y B funcionan de manera diferente según la función usada:

- is_pressed() detecta si un botón está siendo presionado en ese momento.

- was_pressed() detecta si un botón fue presionado y luego soltado, útil para conteos.

- pin_logo.is_touched() permite usar el logo como un sensor táctil adicional.
