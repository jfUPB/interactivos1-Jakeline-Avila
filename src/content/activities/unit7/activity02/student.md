### Mi solución a la actividad 2

#### ¿Por qué es necesario Dev Tunnels y cómo funciona?
Cuando corro el servidor con npm start, solo puedo acceder desde mi compu (localhost). Si intento entrar desde el celular, no funciona porque está buscando el servidor en el propio celular. Dev Tunnels soluciona esto creando una URL pública que redirige a mi compu como un tunel, así el celular puede conectarse aunque no esté en la misma red.

¿Por qué usamos JSON.stringify y JSON.parse?
Porque los objetos en JavaScript no se pueden enviar directamente por la red. Con JSON.stringify los convierto en texto para mandarlos, y con JSON.parse los vuelvo a convertir en objeto del otro lado. Así se pueden enviar datos completos como coordenadas.

¿Para qué sirve touchMoved y el threshold?
touchMoved se ejecuta cuando muevo el dedo sobre la pantalla. Lo uso para saber dónde está tocando el usuario. El threshold evita que se envíen muchos datos si el dedo se mueve solo un poquito, para no saturar la red.

¿Qué otros eventos táctiles hay y para qué sirven?
touchStarted (cuando tocas) y touchEnded (cuando levantas el dedo). Sirven, por ejemplo, para detectar taps o clicks en botones.

¿Dev Tunnels vs IP local?
La IP local solo sirve si estoy en la misma red. Dev Tunnels me da una URL pública que funciona desde cualquier red. Es más práctico aunque depende de Internet.

![image](https://github.com/user-attachments/assets/d6443738-85bb-4df6-8895-996ce39fecd7)
