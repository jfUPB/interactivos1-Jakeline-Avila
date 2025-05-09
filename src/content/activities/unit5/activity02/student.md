### Mi solucion a la actividad 2
![image](https://github.com/user-attachments/assets/a535d4f2-d9bf-45e4-8e27-7cf362ab30d7)

-Al usar el protocolo binario y visualizar los datos como texto en SerialTerminal, los resultados se ven como una serie de caracteres extraños, símbolos no imprimibles o sin sentido. Esto ocurre porque los datos binarios no están pensados para ser interpretados como texto. Cada byte puede representar cualquier valor entre 0 y 255, y no todos esos valores tienen una representación textual legible.

![image](https://github.com/user-attachments/assets/9a8b8922-f843-4aff-b0ed-9e55e3d338f8)

Cuando cambiamos a la vista "Todo en Hex", los datos aparecen en formato hexadecimal, como por ejemplo: FF 9C 00 2A 01 00. Esto se relaciona directamente con la línea:

``` js
data = struct.pack('>2h2B', xValue, yValue, int(aState), int(bState))

```
Esto significa:

2h: dos enteros de 16 bits (2 bytes cada uno, total 4 bytes).

2B: dos enteros sin signo de 8 bits (1 byte cada uno, total 2 bytes).
En total: 6 bytes por mensaje.

Cada grupo de dos bytes representa un número (positivo o negativo, según su codificación), y los últimos dos bytes indican si los botones A y B están presionados (00 o 01).

- ¿Qué ventajas y desventajas ves en usar un formato binario en lugar de texto ASCII?

1. Ventajas del binario:

Más compacto: cada mensaje ocupa solo 6 bytes en lugar de decenas de caracteres.

Más eficiente: ideal para sistemas con recursos limitados o transmisión rápida.

Evita ambigüedades con separadores como comas, saltos de línea, etc.

2. Desventajas del binario:

No es legible fácilmente sin herramientas adecuadas.

Puede ser más difícil de depurar o visualizar directamente sin conversión.

Requiere una estructura fija en ambos extremos para interpretación correcta.

- shake:
![image](https://github.com/user-attachments/assets/6de2bf92-7815-49a9-9f8d-1eb6c3699714)

- Los números negativos se representan en complemento a dos, como cualquier entero con signo en binario. Por ejemplo, el número -1 se representa como FF FF. Por eso, si xValue es negativo, veremos bytes mayores a 7F (127 en decimal), como FF, FE, etc.


En este experimento se envían ambos formatos: primero en binario, luego como texto. En el terminal, los datos se ven así:

```
FF 9C 00 2A 01 00 ASCII:
-100,42,True,False

```
- Diferencias observadas:

El bloque binario es más compacto y difícil de leer.

El bloque ASCII es más largo pero completamente legible y fácil de interpretar.

- Ventajas del binario:

Ahorra espacio, ideal para envíos frecuentes o datos pesados.

Transmisión más rápida.

- Desventajas del binario:

Más difícil de depurar.

Requiere lógica extra para decodificar.

- Ventajas del ASCII:

Legibilidad humana.

Fácil de probar con herramientas simples como SerialTerminal o PuTTY.

- Desventajas del ASCII:

Ocupa más espacio.

Puede ser más lento y más propenso a errores por el formato (comas, saltos de línea).
