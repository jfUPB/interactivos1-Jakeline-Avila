### Mi solucion a la actividad 1

- ![image](https://github.com/user-attachments/assets/148bf779-870e-4d6f-8603-3ae79a59c786)

  - ![image](https://github.com/user-attachments/assets/0db9cc4d-c846-43a5-b127-1ffc5ad4aad8)

  - ![image](https://github.com/user-attachments/assets/f90d1b76-5f28-4db6-abf6-f33302523bb1)

- ![image](https://github.com/user-attachments/assets/abda080b-4905-4154-82de-24ea697178d0)


1. ¿Qué ocurrió en la terminal cuando ejecuté npm install? ¿Cuál creo que es su propósito?

Al ejecutar npm install, la terminal comenzó a descargar e instalar una serie de paquetes desde el registro de npm. Se creó una carpeta llamada node_modules y un archivo package-lock.json. Su propósito es instalar todas las dependencias necesarias que el proyecto requiere para funcionar correctamente. Es un paso fundamental porque sin estas librerías, el código no podría ejecutarse.

2. ¿Qué mensaje específico apareció en la terminal después de ejecutar npm start? ¿Qué indica este mensaje?

Después de ejecutar npm start, apareció el mensaje:
Server listening on port 3000
Esto indica que el servidor del proyecto se levantó correctamente y está listo para recibir conexiones HTTP en el puerto 3000. Es la confirmación de que el backend está funcionando.

3. Describe lo que vi inicialmente en page1 y page2 en el navegador

En page1, vi una interfaz simple que mostraba un canvas con un punto rojo.

En page2, observé una interfaz similar a la de page1, pero independiente visualmente.


Ambas páginas parecen estar sincronizadas o conectadas entre sí a través del servidor.

4. ¿Qué mensajes aparecieron en la terminal del servidor cuando abrí page1 y page2?

Cada vez que abría una página, la terminal mostraba algo como:
Client connected from /127.0.0.1
Esto indica que un nuevo cliente se conectó al servidor, probablemente mediante WebSockets u otra forma de conexión en tiempo real.

5. Describe qué sucede cuando muevo una de las ventanas

Al mover una de las ventanas (por ejemplo, arrastrando una figura dentro de page1), vi que la posición también se actualizaba en la otra ventana (page2) en tiempo real. Esto indica que existe sincronización entre las páginas a través del servidor.

En la consola del navegador (F12 > Consola), aparecieron mensajes tipo:
Received position update: x=..., y=...

Y en la terminal del servidor, se mostraban líneas como:
Broadcasting updated position to clients

