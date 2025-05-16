### Mi solucion a la actividad 9

1. Definición de los Estados y Eventos
Estados del sistema (bomba):

STATE_INIT (Inicialización): Estado en el que el sistema está esperando configuraciones.

STATE_CONFIG (Configuración): El usuario configura el tiempo de la bomba (aumenta o disminuye el tiempo).

STATE_ARMED (Armada): La bomba está armada y cuenta el tiempo hacia la explosión.

STATE_EXPLODED (Explotada): La bomba ha explotado y muestra el estado de la explosión.

Eventos posibles:

Botón A (p5.js): Aumenta el tiempo de la bomba.

Botón B (p5.js): Disminuye el tiempo de la bomba.

Botón de "Conectar" (p5.js): Conecta el puerto serial entre p5.js y el micro:bit.

Botón de "Desarmar" (p5.js): Desarma la bomba si está armada.

Botón de "Armar" (p5.js): Arma la bomba si está configurada.

Sacudida (micro:bit): En el micro:bit, una sacudida activa el estado de la bomba (ARMED).

Botón Touch (micro:bit): Un toque en el pin 1 desarma la bomba si está armada.

Tiempo de cuenta regresiva (micro:bit): El micro:bit decrementa el tiempo y cambia el estado a EXPLODED cuando el tiempo llega a 0.

Comando 'h' (p5.js): Enviar un mensaje desde p5.js para verificar la conexión con el micro:bit.

2. Vectores de Prueba
Pruebas de Configuración (Estado: CONFIG)
Estas pruebas deben asegurarse de que los botones A y B en p5.js funcionan correctamente para ajustar el tiempo de la bomba.

Vector de prueba 1: Aumentar el tiempo de la bomba (con botón A).

Entrada: Presionar el botón A en p5.js.

Esperado: El tiempo de la bomba aumenta en 1 segundo y se muestra en la interfaz.

Vector de prueba 2: Disminuir el tiempo de la bomba (con botón B).

Entrada: Presionar el botón B en p5.js.

Esperado: El tiempo de la bomba disminuye en 1 segundo y se muestra en la interfaz.

Vector de prueba 3: Configurar el tiempo en el rango permitido (entre 10 y 60 segundos).

Entrada: Cambiar el valor de tiempo a través de los botones A y B para estar entre 10 y 60 segundos.

Esperado: El tiempo no debe ser menor que 10 segundos ni mayor que 60 segundos.

Pruebas de Armar/Desarmar la Bomba (Estado: ARMED y DISARMED)
Vector de prueba 4: Armar la bomba desde p5.js.

Entrada: Presionar el botón "Armar" en p5.js.

Esperado: La bomba se arma, y el micro:bit comienza el conteo regresivo de la bomba (p5.js debería recibir el tiempo restante).

Vector de prueba 5: Desarmar la bomba desde p5.js.

Entrada: Presionar el botón "Desarmar" en p5.js.

Esperado: La bomba se desarma y se detiene el conteo regresivo en el micro:bit.

Vector de prueba 6: Sacudida en el micro:bit (para armar la bomba).

Entrada: Sacudir el micro:bit.

Esperado: El micro:bit responde armandola bomba (p5.js debe recibir el estado armado).

Vector de prueba 7: Toque en el micro:bit para desarmar la bomba.

Entrada: Tocar el pin 1 del micro:bit (si la bomba está armada).

Esperado: La bomba se desarma y el micro:bit lo notifica.

Pruebas de Cuenta Regresiva (Estado: ARMED)
Vector de prueba 8: Verificar la cuenta regresiva.

Entrada: La bomba está armada.

Esperado: El micro:bit debe decrementar el tiempo restante cada segundo y actualizarlo en la interfaz p5.js. Cuando el tiempo llega a 0, debe cambiar al estado EXPLODED.

Vector de prueba 9: Verificar explosión de la bomba (tiempo agotado).

Entrada: El tiempo llega a 0 en el micro:bit.

Esperado: El micro:bit debe mostrar una explosión en el display y reproducir un sonido de funeral. Además, p5.js debe mostrar "BOOM!" o la imagen de la explosión.

Pruebas de Desconexión/Conexión (Estado: CONFIG)
Vector de prueba 10: Conectar y desconectar el puerto serial.

Entrada: Conectar y desconectar el puerto serial en p5.js usando el botón "Connect to micro:bit".

Esperado: Al conectar, p5.js debería enviar un mensaje al micro:bit (por ejemplo, enviar un 'h' para verificar la conexión).

3. Pruebas de Regresión
Las pruebas de regresión se realizan para verificar que las funciones que funcionaban correctamente en versiones anteriores del código sigan funcionando correctamente tras realizar cambios o modificaciones en el código.

Prueba de regresión 1: Comprobación de la conexión serial
Entrada: Conectar el micro:bit a través de p5.js.

Esperado: Al presionar "Conectar", p5.js debería conectarse al micro:bit sin problemas.

Posible fallo: Si la conexión serial no se establece correctamente, revisa el puerto serial que estás utilizando (puede estar configurado incorrectamente o el micro:bit no está conectado correctamente).

Prueba de regresión 2: Actualización del tiempo y armar/desarmar la bomba
Entrada: Cambiar el tiempo usando los botones A y B y probar el armado/desarmado de la bomba.

Esperado: El sistema debe actualizar el tiempo correctamente y permitir que el usuario arme o desarme la bomba.

Posible fallo: Si el tiempo no se actualiza correctamente, revisa que la variable time_left se esté modificando adecuadamente al presionar los botones y que se sincronice correctamente con el micro:bit.

Prueba de regresión 3: Explotar la bomba
Entrada: Verificar que la bomba explote cuando el tiempo llegue a 0.

Esperado: Cuando el tiempo llega a 0, el micro:bit debe mostrar la imagen de explosión y reproducir música.

Posible fallo: Si la bomba no explota correctamente, revisa el manejo del temporizador en el micro:bit y asegúrate de que el cambio de estado a EXPLODED se esté realizando adecuadamente.

4. Resumen y Corrección de Fallos
Cuando una prueba falla, es crucial investigar la causa subyacente y corregirla. Algunos pasos comunes para corregir errores son:

Asegúrate de que las variables se estén actualizando correctamente en ambos dispositivos (p5.js y micro:bit).

Verifica que la comunicación serial esté funcionando correctamente entre p5.js y el micro:bit (puerto correcto, velocidad de transmisión, etc.).

Comprueba que los botones y sensores (sacudida, toque) estén correctamente configurados y respondiendo como se espera.
