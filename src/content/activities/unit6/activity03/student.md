### Mi solucion a la actividad 3

1. ¿Por qué es útil usar módulos o librerías en lugar de escribir todo desde cero? ¿Qué ventajas aporta?

Respuesta: Usar módulos o librerías permite ahorrar tiempo, evitar errores y reutilizar soluciones probadas por otros desarrolladores. Además, simplifican tareas complejas (como crear un servidor web o manejar sockets) y hacen que el código sea más limpio y fácil de mantener.


---

4. ¿Qué ocurre si cambio la carpeta ‘views’ por una que no existe? ¿Por qué? ¿Qué mensaje ves en el navegador?

Respuesta: Si cambias views por una carpeta que no existe, al abrir http://localhost:3000/page1.html el navegador mostrará un error 404 (Archivo no encontrado), y en la consola del desarrollador podrías ver que el recurso no se carga. Esto ocurre porque Express intenta buscar los archivos estáticos en una carpeta que no puede encontrar.


---

5. ¿Qué pasa si cambio la ruta '/page1' a '/pagina_uno'? ¿Funciona la URL original? ¿Qué aprendes?

Respuesta: Al cambiar la ruta a /pagina_uno, http://localhost:3000/page1 dejará de funcionar (dará error 404), pero http://localhost:3000/pagina_uno sí funcionará. Esto demuestra que las rutas definidas en app.get() son específicas: si cambia la URL, también debe cambiarse en el navegador. El servidor solo responde a las rutas que definimos explícitamente.


---

6. ¿Qué mensaje ves al abrir page1 y page2 en el navegador? ¿Los IDs son diferentes? ¿Coinciden los IDs al desconectarse?

Respuesta: Al abrir page1, el servidor imprime algo como:
A user connected - ID: abc123
Y al abrir page2, otro ID diferente, por ejemplo:
A user connected - ID: def456

Sí, cada pestaña obtiene un ID diferente. Cuando cierras una de las pestañas, aparece un mensaje como:
User disconnected - ID: abc123,
y coincide con el ID del cliente que se cerró.


---

7. ¿Qué evento se registra al mover cada ventana? ¿Qué datos ves? ¿Qué pasa si quitas broadcast?

Respuesta:

Al mover page1, se registra el evento win1update y en consola aparece algo como:
Received win1update from ID: abc123 Data: {x: ..., y: ..., width: ..., height: ...}

Al mover page2, aparece win2update con datos similares.

Si cambias socket.broadcast.emit(...) por socket.emit(...): Solo el cliente que envió el mensaje recibe la respuesta. Eso significa que los otros clientes no verán los cambios, porque emit() sin broadcast no los incluye. Por eso es crucial usar broadcast.emit() para que otros clientes vean los cambios.



---

8. ¿Qué ocurre si cambio el puerto a 3001? ¿Qué URL funciona? ¿Qué aprendes?

Respuesta: Si cambias el puerto a 3001, el mensaje del servidor será:
Server is listening on http://localhost:3001

http://localhost:3000/page1 no funcionará, porque el servidor ya no está en ese puerto.

http://localhost:3001/page1 sí funcionará.


Esto demuestra que el navegador debe hacer peticiones al mismo puerto donde el servidor está escuchando. La variable port y server.listen() determinan en qué dirección el servidor acepta conexiones.
