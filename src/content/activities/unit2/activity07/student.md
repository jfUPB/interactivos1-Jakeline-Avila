<!-- Produciendo Sonidos con el Micro:bit
Enunciado: investiga y experimenta con la generación de sonidos utilizando el speaker o buzzer del micro:bit.

Entrega:

Código que genere al menos 2 sonidos diferentes.
Documentación del código y descripción del proceso. -->

### Mi solución a la actividad 7

``` js
from microbit import *
import music



while True:
    
    display.show(Image.MEH)
    sleep(400)
    music.play(music.WEDDING)
    audio.play(Sound.YAWN)
    speaker.off()
```

- Primero se importa la música o más bien se utiliza el modulo de música del micro:bit
- Se crea el ciclo pero la música se va a detener cuando llegue a speaker.off().
- music.play(music.WEDDING) es música ya incorporada y audio.play(Sound.YAWN) también.
