### Mi solucion a la actividad 12

¿Qué tanto aprendí en esta unidad?

En esta unidad, siento que he aprendido bastante, especialmente en cuanto a conceptos clave de programación como máquinas de estado, manejo de eventos, funciones, y estructuras de control. Estos conceptos me han permitido entender mejor cómo estructurar programas complejos y cómo manejar diferentes situaciones en mi código.

Conceptos que aprendí:
Máquinas de estado: Aprendí cómo implementar máquinas de estado para gestionar el flujo de un programa, especialmente en el caso de la bomba en p5.js y micro:bit. He entendido cómo organizar estados diferentes (inicialización, configuración, armado, etc.) y las transiciones entre ellos.

Ejemplo: En el código que desarrollé para controlar la bomba, utilicé estados como STATE_INIT, STATE_CONFIG, STATE_ARMED, y STATE_EXPLODED para controlar las transiciones del sistema en función de las entradas del usuario (botones, movimientos, etc.). Este enfoque me ayudó a tener un control claro sobre cómo debía comportarse mi programa en cada fase.

Manejo de eventos: Aprendí cómo manejar eventos tanto en el micro:bit como en p5.js, y cómo estas interacciones pueden modificar el comportamiento del programa. Esto es clave cuando quiero que el programa reaccione a entradas específicas, como un botón presionado o una sacudida.

Ejemplo: En p5.js, usé eventos de clics de botones para controlar la conexión al micro:bit y enviar datos, y también usé eventos del micro:bit para cambiar el estado de la bomba según las interacciones del usuario (sacudidas o presionar botones).

Funciones: El uso de funciones para organizar mejor el código y evitar repeticiones también fue una gran lección. Las funciones me ayudaron a modularizar el programa y hacer que partes del código fueran reutilizables.

Ejemplo: Utilicé funciones como show_time() para actualizar la pantalla con el tiempo restante de la bomba, evitando la repetición de código en el ciclo principal.

Conceptos que no aprendí completamente:
Concurrencia y asincronía: Aunque trabajé con eventos y algunos conceptos básicos de concurrencia, aún me cuesta trabajo comprender cómo manejar múltiples tareas que se ejecutan al mismo tiempo sin que interfieran entre sí. La parte de asincronía (promesas, async/await, callbacks) me parece complicada y necesito profundizar más en cómo coordinar múltiples procesos de manera eficiente.

Ejemplo: En algunas pruebas, cuando intenté manejar múltiples eventos de forma simultánea en el micro:bit y p5.js, a veces el código no reaccionaba de manera correcta, y el flujo de eventos se desordenaba.

Manejo avanzado de errores: Aunque sé cómo usar bloques try/catch, no siempre logro prever todos los posibles errores que podrían surgir en mi código. Todavía me falta una comprensión más profunda de las excepciones y cómo manejarlas en escenarios más complejos.

Ejemplo: Cuando el micro:bit no se conectaba correctamente en algunos casos, no pude prever ese tipo de error y mi código no reaccionaba adecuadamente.

Estrategias para mejorar en los conceptos que no aprendí:
Concurrencia y asincronía:

Estrategia: Para mejorar, dedicaré más tiempo a practicar con ejercicios que involucren tareas asincrónicas. Empezaré con ejemplos simples de promesas y async/await, luego avanzaré a casos más complejos con hilos y tareas concurrentes.

Recursos: Utilizaré tutoriales en línea como los de MDN, FreeCodeCamp y videos de YouTube. Además, haré proyectos donde implemente promesas y callback functions para tener más práctica.

Tiempo: Dedicaré al menos 30 minutos al día, enfocándome en concurrencia y asincronía durante las próximas 2 semanas.

Fecha de inicio: Comenzaré inmediatamente después de esta autoevaluación.

Manejo avanzado de errores:

Estrategia: Para mejorar en el manejo de errores, practicaré más con el uso de bloques try/catch y trataré de anticipar posibles errores en diferentes partes de mis programas. También aprenderé a crear mis propias excepciones para manejar situaciones específicas.

Recursos: Consultaré guías de manejo de errores en JavaScript y Python, y trabajaré con ejemplos de aplicaciones que interactúan con hardware (como el micro:bit), donde los errores son más comunes.

Tiempo: Dedicaré 30 minutos por día a este tema, realizando ejercicios prácticos y revisando el manejo de errores en proyectos reales.

Fecha de inicio: Comenzaré mañana, enfocándome en integrar correctamente el manejo de errores en mis proyectos actuales.

