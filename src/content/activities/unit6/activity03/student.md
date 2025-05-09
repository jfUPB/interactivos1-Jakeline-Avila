### Mi solucion a la actividad 3

1. ¿Por qué es útil usar módulos o librerías en lugar de escribir todo desde cero? ¿Qué ventajas aporta?

Respuesta: Usar módulos o librerías permite ahorrar tiempo, evitar errores y reutilizar soluciones probadas por otros desarrolladores. Además, simplifican tareas complejas (como crear un servidor web o manejar sockets) y hacen que el código sea más limpio y fácil de mantener.


---

4. ¿Qué ocurre si cambio la carpeta ‘views’ por una que no existe? ¿Por qué? ¿Qué mensaje ves en el navegador?

![image](https://github.com/user-attachments/assets/0686dfce-b8ef-426f-974b-9fcbd6ce706b)
![image](https://github.com/user-attachments/assets/1afeee85-569b-4d82-8db0-d41f01fad1d5)

- Pasa esto debido a que en views esta almacenado todo lo de las paginas, por lo tanto cambiarlo  una carpeta que no existe no hay forma de que pueda
buscarla porque no existe.




---

5. ¿Qué pasa si cambio la ruta '/page1' a '/pagina_uno'? ¿Funciona la URL original? ¿Qué aprendes?

Respuesta: Al cambiar la ruta a /pagina_uno, http://localhost:3000/page1 dejará de funcionar (dará error 404), pero http://localhost:3000/pagina_uno sí funcionará. Esto demuestra que las rutas definidas en app.get() son específicas: si cambia la URL, también debe cambiarse en el navegador. El servidor solo responde a las rutas que definimos explícitamente.


---

6. ¿Qué mensaje ves al abrir page1 y page2 en el navegador? ¿Los IDs son diferentes? ¿Coinciden los IDs al desconectarse?

Respuesta: ![image](https://github.com/user-attachments/assets/4f183f43-a468-4d72-a482-0f04bcaea83c)
- No coinciden al desconectarse, son diferentes al reiniciarse



---

7. ¿Qué evento se registra al mover cada ventana? ¿Qué datos ves? ¿Qué pasa si quitas broadcast?

Respuesta:

Al mover page1, se registra el evento win1update

Al mover page2, aparece win2update con datos similares.

Si cambias socket.broadcast.emit(...) por socket.emit(...): Solo el cliente que envió el mensaje recibe la respuesta. Eso significa que los otros clientes no verán los cambios, porque emit() sin broadcast no los incluye. Por eso es crucial usar broadcast.emit() para que otros clientes vean los cambios.



---

8. ¿Qué ocurre si cambio el puerto a 3001? ¿Qué URL funciona? ¿Qué aprendes?

Respuesta: Si cambias el puerto a 3001, el mensaje del servidor será:
Server is listening on http://localhost:3001

http://localhost:3000/page1 no funcionará, porque el servidor ya no está en ese puerto.
![image](https://github.com/user-attachments/assets/819d96f2-6a48-475d-a9ac-94e52e3887b0)

http://localhost:3001/page1 sí funcionará.
![image](https://github.com/user-attachments/assets/2dd300e7-1308-4b6e-9d80-6d444b1e62e2)



Esto demuestra que el navegador debe hacer peticiones al mismo puerto donde el servidor está escuchando. La variable port y server.listen() determinan en qué dirección el servidor acepta conexiones.
