### Mi solucion a la actividad 5

| Aspecto            | Protocolo ASCII                                           | Protocolo Binario                                           |
|--------------------|-----------------------------------------------------------|-------------------------------------------------------------|
| **Eficiencia**     | Baja: envía texto plano, más bytes por dato               | Alta: cada dato ocupa solo los bytes necesarios             |
| **Velocidad**      | Lenta: requiere convertir strings a números               | Rápida: los datos llegan en formato binario directamente    |
| **Facilidad**      | Fácil de leer y depurar en consola                        | Más compleja: requiere interpretación (DataView, etc.)      |
| **Uso de recursos**| Mayor uso de memoria y CPU por conversiones de texto      | Uso optimizado de CPU y memoria                             |
| **Ejemplo**        | `"512,230,true,false\n"` → 18+ bytes                      | `\xAA + 2h2B + checksum` → 8 bytes                          |


¿Por qué fue necesario introducir framing en el protocolo binario?
Porque los datos binarios no tienen separadores naturales (como \n en texto). Sin framing, sería imposible saber dónde empieza y termina cada paquete.


---

¿Cómo funciona el framing?
Usa un carácter especial al inicio del paquete (como 0xAA) y una estructura de longitud fija para detectar el principio del paquete y procesarlo correctamente.


---

¿Qué es un carácter de sincronización?
Es un byte específico (en este caso 0xAA) que indica el inicio de un paquete de datos, ayudando al receptor a sincronizarse con el flujo.


---

¿Qué es el checksum y para qué sirve?
Es una suma de los datos que permite verificar la integridad del paquete. Si el checksum recibido no coincide con el calculado, el paquete está dañado.


---

¿Qué hace la función concat en readSerialData()? ¿Por qué?
Une el contenido actual de serialBuffer con los nuevos datos (newData) para mantener todos los datos que van llegando sin perderlos.


---

¿Por qué el bucle solo corre si hay 8 o más bytes?
Porque cada paquete binario completo tiene 8 bytes. Si hay menos, no hay suficiente información para procesar un paquete válido.


---

¿Qué significa 0xAA?
Es el valor hexadecimal 170, usado como marcador de inicio de paquete (carácter de sincronización).


---

¿Qué hace shift y continue? ¿Por qué?
shift elimina el primer byte del buffer si no es 0xAA. continue salta a la siguiente iteración para seguir buscando el inicio de un paquete válido.


---

¿Qué hace break si hay menos de 8 bytes? ¿Por qué?
Sale del bucle porque no hay suficiente información para procesar un paquete completo.


---

¿Diferencia entre slice y splice? ¿Por qué usar ambos?

slice(0, 8) copia los primeros 8 bytes sin alterar el buffer.

splice(0, 8) elimina esos 8 bytes del buffer.
Se usa slice para trabajar con los datos y luego splice para limpiar el buffer.



---

¿Cómo opera reduce en este contexto?
Suma todos los valores del arreglo dataBytes para calcular el checksum. % 256 garantiza que el valor esté en un byte (0–255).


---

¿Por qué se compara el checksum enviado con el calculado? ¿Para qué sirve esto?
Para verificar que los datos llegaron sin errores. Si no coinciden, se descarta el paquete.


---

¿Qué hace continue en este contexto? ¿Por qué?
Salta al siguiente ciclo del bucle sin procesar el paquete, porque el checksum indica que está dañado.


---

¿Qué es un DataView? ¿Para qué se usa?
Es una vista sobre un buffer binario que permite leer valores con distintos formatos (int16, uint8, etc.) de forma precisa.


---

¿Por qué es necesario convertir los datos con DataView y no leerlos directamente?
Porque los datos están codificados en formato binario. DataView permite interpretarlos correctamente como enteros con el tamaño y signo adecuados.
