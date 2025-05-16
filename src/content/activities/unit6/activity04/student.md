### Mi solución a la actividad 4

1. Detener el servidor y refrescar page2.html

¿Ves algún error relacionado con la conexión? ¿Qué indica?

Sí, al refrescar la página con el servidor detenido, aparece un error en la consola como:
```

GET http://localhost:3000/socket.io/?EIO=4&transport=polling... net::ERR_CONNECTION_REFUSED

```

Esto indica que el cliente intenta conectarse al servidor, pero no puede encontrarlo porque está apagado.

¿Desaparecen los errores al reiniciar el servidor y refrescar?

Sí. Una vez que el servidor se reinicia y la página se vuelve a cargar, la conexión se establece correctamente y desaparecen los errores. En consola aparece:

```

Connected to server - My ID: <ID>

```

---

2. Comentar socket.emit('win2update', ...) al conectarse

¿Qué pasó en page1 antes de que movieras page2? ¿Tenía la información correcta sobre page2 desde el principio?

No. Page1 no tenía información sobre page2 hasta que esta última se movió. Eso se debe a que sin el emit inicial, page2 no envió sus datos al conectarse, por lo que el servidor y page1 no sabían nada de su estado.

¿Por qué es útil enviar el estado inicial al conectarse?

Enviar el estado inicial garantiza que los demás clientes tengan inmediatamente los datos correctos de esta ventana. Si no lo hacemos, no habrá información hasta que ocurra una acción (como mover la ventana), lo que retrasa la sincronización visual.


---

3. Recibir el evento getdata

¿Se dispara el log "Page 2 received..." al mover page1? ¿Qué datos muestra?

Sí. Cada vez que se mueve page1, la consola de page2 muestra:

Page 2 received data about the other window: {x: ..., y: ..., width: ..., height: ...}

Los datos corresponden al estado actualizado de la ventana page1.

¿Y al mover page2?

No. No aparece ese mensaje porque getdata se dispara cuando el servidor recibe datos de la otra ventana (page1) y los reenvía a page2. Si page2 se mueve, está emitiendo su propio estado, no recibiendo información.


---

4. Detectar cambios y eficiencia

¿Cuándo aparece el mensaje "Page 2 detected change..."?

Solo aparece cuando realmente cambia la posición o tamaño de la ventana page2.

¿Aparece si la ventana está quieta?

No. La función checkWindowPosition() detecta cambios comparando el estado actual con el anterior. Si no hay cambios, no emite nada ni imprime nada.

¿Por qué es eficiente esta estrategia?

Evita enviar mensajes innecesarios por la red. Al comparar el estado actual con el anterior, solo se envían datos si realmente hubo un cambio, lo que ahorra ancho de banda y mejora el rendimiento.


---

5. Personalización creativa (experimentos visuales)

Ideas probadas:

1. Fondo según distancia:


```
let distancia = resultingVector.mag();
background(map(distancia, 0, 1000, 255, 0));
```

Resultado: el fondo se oscurece a medida que las ventanas se alejan, creando una sensación de proximidad visual.

2. Tamaño del círculo local según tamaño de ventana remota:


```
drawCircle(point2[0], point2[1], remotePageData.width / 4);
```

(se modificó drawCircle para aceptar un parámetro de tamaño).

Resultado: el círculo en la ventana actual crece o se reduce dependiendo del ancho de la otra ventana, representando visualmente su tamaño.

3. Color de línea según posición:


```
if (resultingVector.x < 0) {
    stroke(255, 0, 0); // rojo si la ventana remota está a la izquierda
} else {
    stroke(0, 0, 255); // azul si está a la derecha
}
```

Resultado: ayuda a ubicar rápidamente la dirección relativa de la otra ventana con solo ver el color de la línea.


---
