<!-- Generando Salidas Visuales
Enunciado: experimenta con la pantalla LED del micro:bit para mostrar diferentes imágenes y texto.

Entrega:

Código que muestre al menos 3 imágenes o textos diferentes en la pantalla.
Explicación del funcionamiento -->

### Mi solución a la actividad 6

``` js
from microbit import *



while True:
    display.show(Image.HEART)
    sleep(1000)
    
    display.show(Image.HAPPY)
    sleep(400)
    display.show(Image.SNAKE)
    sleep(400)
```
- Con el while true se crea un LOOP infinito, con display.show podemos poner imagenes en el micro:bit, si queremos texto sería con display.scroll, las imagenes ya estan hechas desde el programa y construidas asi que no hay problema en ya ponerlas.
