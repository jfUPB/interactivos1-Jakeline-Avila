### Mi solución a la actividad 4

#### Propósito de mobile/sketch.js y desktop/sketch.js
En mobile/sketch.js, el objetivo es capturar el toque del usuario en el móvil y enviarlo al servidor a través de Socket.IO. 
Cuando el usuario mueve el dedo, la función touchMoved detecta el movimiento y, si es significativo (supera un umbral), manda las coordenadas del toque al servidor.

Por otro lado, en desktop/sketch.js, el cliente recibe esas coordenadas del servidor y mueve un círculo en el canvas para reflejar la posición del toque que se hizo en el móvil.

#### Función touchMoved en el Móvil
La función touchMoved se activa cada vez que el usuario mueve el dedo en la pantalla. Si el movimiento es más grande que el umbral 
definido (para evitar enviar demasiados mensajes), crea un objeto con el tipo de mensaje y las coordenadas del toque. Luego, convierte ese objeto a JSON y lo manda al servidor.
Si no se tuviera el umbral, el servidor recibiría muchos mensajes innecesarios, lo que podría hacer que todo se vuelva más lento.

Si quisiera que también respondiera al clic del ratón, usaría funciones como mouseMoved() o mousePressed() de p5.js. Así, el código podría funcionar tanto en el móvil como en un computador.

#### Recibiendo y Procesando los Datos en el Cliente de Escritorio
En desktop/sketch.js, el cliente se conecta al servidor y escucha el evento 'message'. Cuando llega un mensaje, 
lo convierte de nuevo de JSON a un objeto con JSON.parse(). Si es un mensaje válido con el tipo de 'touch', se actualizan las coordenadas del círculo en el canvas 
para reflejar la posición del toque.

Es importante usar try...catch para asegurarnos de que si los datos no son válidos, no se rompa todo el código. Si el tamaño del canvas fuera diferente entre móvil y escritorio,
se podría ajustar las coordenadas con la función map() de p5.js. Y si el servidor mandara un evento distinto como 'updateDesktop', solo tendría que cambiarse el nombre del evento en el código.



#### b¿Por qué enviar los datos como un objeto JSON?

Enviar los datos como JSON es útil porque permite estructurar la información de forma clara y facilita que en el futuro se puedan agregar más datos si es necesario.

#### ¿Qué pasaría si se quita el threshold?

Si eliminamos el umbral, el servidor recibiría un mensaje por cada pequeño movimiento del dedo, lo que sobrecargaría la red y haría que la aplicación fuera más lenta.

#### ¿Cómo adaptarías el código para que también funcione con el ratón?

Usaría las funciones mouseMoved() y mousePressed() de p5.js. De esta manera, el código respondería tanto a toques en el móvil como a clics con el ratón.


#### ¿Por qué usar JSON.parse() dentro de try...catch?

Usar try...catch es necesario para evitar que el código se detenga si ocurre un error al intentar convertir el JSON. Así podemos manejar cualquier problema sin interrumpir la ejecución.

#### ¿Cómo ajustarías las coordenadas si los canvas tienen tamaños diferentes?

Para que las coordenadas del móvil se adapten a un canvas de tamaño diferente en escritorio, usaría la función map() de p5.js para hacer la conversión proporcional.

#### ¿Qué cambiarías si el servidor enviara 'updateDesktop' en lugar de 'message'?

Tendría que cambiar el nombre del evento que escucha el cliente. En lugar de socket.on('message', ...), usaría socket.on('updateDesktop', ...).
