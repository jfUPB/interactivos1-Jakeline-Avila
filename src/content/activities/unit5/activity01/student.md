### Mi solucion a la actividad 1

El micro:bit se comunica con el sketch de p5.js mediante el puerto serial, usando una velocidad de 115200 baudios. Envía datos cada 100 milisegundos, lo que equivale a una frecuencia de 10 Hz. El sketch usa la librería p5.webserial para recibir los datos y procesarlos en tiempo real.

El micro:bit envía cuatro datos separados por comas: el valor del eje X y el valor del eje Y del acelerómetro, y los estados de los botones A y B (True o False). Ejemplo: -134,278,False,True. Esta es una estructura simple basada en texto ASCII, en formato CSV, finalizada por un salto de línea \n.

En el sketch de p5.js, los datos se leen dentro del ciclo draw(), verificando si hay datos disponibles en el puerto. Luego se hace readUntil("\n") para capturar la línea completa. Se usa split(',') para separar los datos, y se transforman: las coordenadas X e Y se ajustan al centro de la pantalla, y los estados de los botones se convierten en booleanos. Finalmente, se llama a la función updateButtonStates() para manejar los eventos.

Los eventos “A pressed” y “B released” se generan comparando los estados actuales de los botones con los anteriores. Si A pasa de falso a verdadero, se considera que fue presionado, y se crea una nueva figura. Si B pasa de verdadero a falso, se considera que fue soltado, y se cambia el color del trazo. Esto permite una interacción precisa basada en los datos que envía el micro:bit.![20250425_163333](https://github.com/user-attachments/assets/102e1de6-44c7-43f0-b70f-c362f2429fb8)
![Uploading 20250425_163333.png…]()
![20250425_163315](https://github.com/user-attachments/assets/17b6c757-d96a-44c6-9728-e34c7ab0cf2b)
![20250425_163249](https://github.com/user-attachments/assets/b7325dce-945c-4580-a6fe-d3d9eb5702b2)
