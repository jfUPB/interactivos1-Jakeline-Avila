### Mi solución a la actividad 3

#### Función de express.static('public')
Creo que la función principal de express.static('public') es permitir que el servidor sirva automáticamente archivos estáticos, como HTML, CSS o imágenes, desde la carpeta public. 
Si un cliente pide una URL, como /desktop/, el servidor busca el archivo correspondiente en esa carpeta y lo entrega sin necesidad de definir rutas específicas para cada archivo. 
Esto hace que el código sea más sencillo que cuando se usan rutas personalizadas con app.get(...).

#### Flujo de un mensaje táctil
Cuando el móvil detecta un toque, envía los datos con un evento llamado 'message'. El servidor recibe este mensaje mediante socket.on('message', ...), 
lo muestra en consola y luego lo reenvía a todos los otros clientes con socket.broadcast.emit('message', datos). Es importante usar broadcast.emit porque así el mensaje no vuelve al 
cliente que lo envió, solo a los demás.

#### ¿Quién recibe el mensaje si hay varios dispositivos conectados?
Si conectas dos escritorios y un móvil al servidor y el móvil envía un mensaje (como al mover el dedo en la pantalla), los dos escritorios lo recibirían, 
pero el móvil no. Esto pasa porque el servidor usa socket.broadcast.emit, que excluye al cliente que envió el mensaje, asegurando que solo los otros dispositivos lo reciban.

#### Utilidad de los mensajes en consola
Los mensajes que aparecen en la consola durante la ejecución del servidor son útiles para saber qué está pasando. Cuando un cliente se conecta, el servidor muestra su ID, cuando recibe un mensaje
lo imprime en consola, y cuando un cliente se desconecta también se informa. Además, cuando el servidor arranca, muestra un mensaje indicando que está listo y escuchando en un puerto específico.
