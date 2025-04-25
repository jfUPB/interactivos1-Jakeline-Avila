### Mi solucion a la actividad 5

| Aspecto            | Protocolo ASCII                                           | Protocolo Binario                                           |
|--------------------|-----------------------------------------------------------|-------------------------------------------------------------|
| **Eficiencia**     | Baja: envía texto plano, más bytes por dato               | Alta: cada dato ocupa solo los bytes necesarios             |
| **Velocidad**      | Lenta: requiere convertir strings a números               | Rápida: los datos llegan en formato binario directamente    |
| **Facilidad**      | Fácil de leer y depurar en consola                        | Más compleja: requiere interpretación (DataView, etc.)      |
| **Uso de recursos**| Mayor uso de memoria y CPU por conversiones de texto      | Uso optimizado de CPU y memoria                             |
| **Ejemplo**        | `"512,230,true,false\n"` → 18+ bytes                      | `\xAA + 2h2B + checksum` → 8 bytes                          |
