### Mi solucion en actividad 10

1. ¿Por qué la máquina de estados es poderosa para la escalabilidad y manejo de eventos?

La máquina de estados es efectiva para aplicaciones que requieren manejar múltiples eventos y estados, como el control de la bomba. Sus ventajas son:

Organización clara de la lógica del sistema, facilitando el mantenimiento y expansión.

Manejo eficiente de concurrencia al dividir el sistema en estados aislados.

Escalabilidad: Nuevos estados y transiciones se pueden agregar fácilmente.

Manejo de eventos: Los eventos solo afectan el estado en el que se encuentra el sistema, reduciendo conflictos.

Esto facilita el control de la bomba y permite agregar nuevos eventos sin complicar la estructura del código.

2. Ventajas y desventajas de las pruebas realizadas

Ventajas:

Cobertura completa de todos los estados y eventos importantes.

Claridad en cada prueba, asegurando que las funcionalidades se comportan como se espera.

Automatización de las pruebas, facilitando su ejecución.

Desventajas:

Dependencia del entorno (p5.js y micro:bit), lo que puede generar inconsistencias.

Falta de pruebas de casos excepcionales, que no cubren todos los posibles errores.

Pruebas manuales para la interacción física (sacudidas, botones), lo que aumenta el esfuerzo.

3. ¿Por qué son importantes las pruebas de regresión?

Las pruebas de regresión aseguran que los cambios no rompan funcionalidades previas. Son esenciales porque:

Mantienen la estabilidad del sistema al detectar errores introducidos por cambios.

Identifican efectos secundarios que puedan surgir tras modificaciones.

Consecuencias de no hacer pruebas de regresión:

Errores no detectados que podrían afectar la funcionalidad.

Inestabilidad del sistema que podría causar fallos inesperados.

Desconfianza de los usuarios si experimentan fallos no detectados.

