### Mi solución a la actividad 2

1. ¿Qué pasaría si la “rampa de acceso” (Wi-Fi o cable de red) se corta?

Si se corta la conexión (la rampa de acceso), mi computador pierde la capacidad de enviar y recibir datos por Internet. No podría cargar páginas web, enviar mensajes ni conectarme con servidores. Es como si mi vehículo (navegador) se quedara atrapado sin poder salir al mundo exterior.


---

2. Ejemplo de relación Cliente-Servidor en la vida diaria

En un restaurante:

Cliente: Yo, cuando pido una hamburguesa.

Servidor: El mesero.

Petición: La orden de comida.

Respuesta: El plato de comida servido en la mesa.

---

3. Análisis de una URL (ejemplo: https://www.wikipedia.org/wiki/Internet)

Protocolo: https:// – Usa HTTPS para comunicación segura.

Dominio: www.wikipedia.org – Es el nombre del servidor.

Ruta: /wiki/Internet – Especifica el contenido exacto que quiero ver.


Si escribo solo www.wikipedia.org, el servidor me envía una página por defecto (la página principal) como respuesta.


---

4. Comparación de HTTP con protocolos seriales (como el de micro:bit)

Similitudes:

Ambos siguen reglas (protocolos) para entenderse entre dispositivos.

Hay mensajes que se envían y reciben.


Diferencias:

HTTP es más complejo, maneja múltiples tipos de datos y errores.

Es más estructurado (usa encabezados, códigos de estado, etc.).

El protocolo serial era más directo y de bajo nivel.


Razón para la complejidad: HTTP necesita manejar muchos tipos de dispositivos, conexiones y contenidos. Debe asegurar compatibilidad, seguridad y estructura en la comunicación.


---

5. Partes del formulario de login

HTML: Campos de texto, el botón, el formulario en sí.

CSS: Color del botón, diseño del formulario, márgenes.

JavaScript: Verifica si los campos están llenos antes de enviar, muestra errores sin recargar la página.



---

6. Comparación del modelo draw() de p5.js vs eventos en JS

Ventajas del modelo basado en eventos:

Más eficiente: el navegador no redibuja ni recalcula todo constantemente.

Más responsivo: reacciona solo cuando algo cambia (ej. clic, mensaje recibido).

Ideal para interfaces de usuario donde solo algunas acciones requieren respuesta.


Ineficiencia del draw() continuo: Sí, redibujar 60 veces por segundo toda una página web sería un desperdicio si no hay cambios. Solo es útil en contextos como animaciones o juegos.


---

7. ¿Por qué es útil usar JavaScript en el cliente y el servidor?

Misma lógica, mismo lenguaje: Los desarrolladores pueden reutilizar código.

Aprendizaje más sencillo: Solo se necesita dominar un lenguaje (JavaScript).

Desarrollo más rápido: Menos tiempo cambiando de contexto entre lenguajes.



---

8. Diferencia entre HTTP tradicional y WebSockets

HTTP tradicional: Cada mensaje es una nueva solicitud; el servidor solo responde si el cliente pregunta.

WebSockets: Se abre una conexión persistente y ambas partes pueden enviarse mensajes en cualquier momento, sin necesidad de una nueva solicitud.


Aplicaciones típicas:

Chats en tiempo real.

Juegos multijugador.

Colaboración en línea (como Google Docs).

Seguimiento de ubicación o movimientos (como en nuestro caso de estudio con las ventanas).
