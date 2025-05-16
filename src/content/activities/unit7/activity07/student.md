### Mi solución a la actividad 7

<!-- Revisa los objetivos: vuelve a leer la sección ¿Qué aprenderás en esta unidad? 💡.
Evalúa tu confianza: para cada objetivo de aprendizaje, evalúate honestamente usando una escala simple (ej: 2=Necesito repasar mucho, 3=Lo entiendo pero con dudas, 4=Lo entiendo bien, 5=Podría explicarlo a un compañero).
Configurar y usar VS Code Dev Tunnels: [Tu Puntuación]
Implementar arquitectura cliente-servidor (móvil->servidor): [Tu Puntuación]
Usar Socket.IO para retransmitir datos (servidor->escritorio): [Tu Puntuación]
Capturar y procesar eventos en el móvil (p5.js): [Tu Puntuación]
Modificar sistema interactivo para crear la experiencia: [Tu Puntuación]
Analizar y explicar flujo de datos completo (móvil->servidor->escritorio): [Tu Puntuación]
Reflexiona sobre el proceso:
¿Qué concepto o actividad de esta unidad te resultó más fácil de entender o realizar? ¿Por qué crees que fue así?
¿Qué concepto o actividad te presentó mayor dificultad? ¿Qué pasos seguiste para intentar superarla? ¿Qué recursos o estrategias te fueron más útiles?
Describe con tus propias palabras, como si se lo explicaras a alguien que no tomó el curso, cuál es el flujo principal de información en la aplicación que construimos (desde la interacción del usuario en el móvil hasta la imagen en el escritorio). ¿Qué rol juega cada tecnología (Node.js, Socket.IO, Dev Tunnels, p5.js)?
¿Cómo crees que podrías aplicar lo aprendido en esta unidad (usar un móvil como controlador, comunicación en tiempo real, túneles) en otros proyectos o contextos?
-->

#### Evalúa tu confianza: para cada objetivo de aprendizaje, evalúate honestamente usando una escala simple (ej: 2=Necesito repasar mucho, 3=Lo entiendo pero con dudas, 4=Lo entiendo bien, 5=Podría explicarlo a un compañero).

Configurar y usar VS Code Dev Tunnels: 5
Implementar arquitectura cliente-servidor (móvil->servidor): 5
Usar Socket.IO para retransmitir datos (servidor->escritorio): 4
Capturar y procesar eventos en el móvil (p5.js): 4
Modificar sistema interactivo para crear la experiencia: 3
Analizar y explicar flujo de datos completo (móvil->servidor->escritorio): 3

1. r/= Creo que lo mas facil fue configurar el port para transmitir datos por devtunnels, debido a que la guía del enunciado fue muy buena
2. R/= En la aplicación intentando que funcionara pero se logró, tuve que experimentar demasiado y poner mi codigo en ia para ver que errores tenia.
3. R/= Es como si el usuario controlara el escritorio con el dedo. Primero, cuando toco la pantalla del móvil, ese toque se convierte en datos: las coordenadas del dedo (x, y), el color, y si las partículas deben tener borde (stroke). Todo eso se mete en un objeto JSON.
Ese objeto se manda al servidor usando Socket.IO. Pero como el servidor está en mi máquina, y yo uso el móvil desde otra red, uso Dev Tunnels para que el móvil lo pueda encontrar.
4.R/=Creo que esta forma de usar el móvil como control remoto es útil para muchas cosas. Por ejemplo:
En instalaciones interactivas, donde el público puede controlar algo que se proyecta en una pantalla.
