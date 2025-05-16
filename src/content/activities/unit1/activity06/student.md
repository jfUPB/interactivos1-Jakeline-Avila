<!--Entrega: vas a tratar de analizar todos los pasos anteriores usando
los conocimientos que tienes hasta este punto de la carrera. No te asustes con esta actividad, 
la idea es que intentes, estamos explorando y en las próximas unidades vas a poder entender muy bien todo. Por ahora, solo quiero que intentes.

Describe qué pasa en el punto 15 y cómo crees que esto se logre.
Describe qué pasa en el punto 16 y cómo crees que esto se logre.
Describe qué pasa en el punto 17 y cómo crees que esto se logre.-->
## Solución a la actividad 6

- En el punto 15 opino que en el codigo con la funcion draw y al utilizar los condicionales en esta linea
``` js
if(port.availableBytes() > 0){
        let dataRx = port.read(1);
        if(dataRx == 'A'){
            fill('red');   
        }
        else if(dataRx == 'B'){
            fill('yellow'); 
        }
```
En la función draw permitimos que el computador lea y verifique si está el port A disponibles y dependiendo del boton oprimido en la parte de fill lo llena del
color del port oprimido, ejemplo si oprimimos el port A se llena de rojo el circulo y si oprimimos B se llena de amarillo.

- En el punto 16 cuando se sacude el microbit y cambia el color a verde opino que sucede debido a que en esta linea de código:
``` js
else{
            fill('green'); 
        }

if accelerometer.was_gesture('shake'):
        uart.write('C')
        sleep(500)
```
Si se detecta otra acción aparte de oprimir los ports A y B, se va a llenar el circulo del color verde, el microbit tal vez tenga un sensor para que se detecte el movimiento del microbit, en este caso es un acelerometro
y se recibe la señal.


- En el punto 17 cuando se presiona el botón send love:
``` js
let sendBtn = createButton('Send Love');
    sendBtn.position(220, 300);
    sendBtn.mousePressed(sendBtnClick);
    fill('white');
    ellipse(width / 2, height / 2, 100, 100);

if uart.any():
        data = uart.read(1)
        if data:
            if data[0] == ord('h'):
                display.show(Image.HEART)
                sleep(500)
                display.show(Image.HAPPY)
```
El programa envia datos al microbit y el microbit puede responder enviando datos al programa, en este caso el microbit esta configurado que al presionar el boton se activa la funcion sendBtnClick y el
se envia el caracter h al port serial del microbit.
