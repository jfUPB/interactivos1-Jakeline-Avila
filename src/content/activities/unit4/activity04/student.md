### Mi solucion a la actividad 4

- Compara el código original del caso de estudio con el anterior. ¿Qué notas de diferente?

R/= Al comparar el código original del micro:bit en Python con la estructura del nuevo proyecto en p5.js, se observan varias diferencias significativas. El código en Python se ejecuta directamente en el micro:bit 
y se encarga de recoger datos de los sensores integrados (como el acelerómetro y los botones) para luego enviarlos por el puerto serial utilizando uart.write(). En cambio, el proyecto en p5.js está diseñado para correr 
en un navegador web, utilizando JavaScript junto con la librería p5.js para la visualización gráfica y la librería p5.webserial.js para establecer la comunicación serial desde el navegador con el micro:bit. Mientras que 
el código en Python se enfoca en la recolección y transmisión de datos, el entorno p5.js está orientado a la recepción de esos datos y su representación visual o interacción en la interfaz web. Además, el código HTML 
vincula los archivos necesarios para que el navegador pueda conectarse al micro:bit, 
lo cual no es necesario en el código que corre directamente en el dispositivo.

