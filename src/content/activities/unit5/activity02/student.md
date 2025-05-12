# Comparación entre envío de datos en ASCII y binario

## 1. Visualización en la app web

![image]("../../../../assets/A.png")

---

## 2. ¿Esto sigue siendo ASCII?

Sí, los datos que se observan aún están codificados en ASCII.  
Si fueran binarios, el formato cambiaría considerablemente. Un posible desglose sería:

- 2 bytes para valueX = 4 bits  
- 2 bytes para valueY = 4 bits  
- 1 byte para el botón A = 2 bits  
- 1 byte para el botón B = 2 bits  
- **Total: 12 bits (6 bytes)**

---

## 3. Comparativa de ventajas y desventajas

### Ventajas del formato binario:
- Requiere menos espacio que el formato ASCII, por lo tanto es más eficiente para transmitir datos.
- El binario puede ser usado directamente por la máquina sin necesidad de convertir desde texto.

### Desventajas del formato binario:
- Puede resultar más complejo de interpretar, especialmente al hacer pruebas o depuración, en comparación con el texto en ASCII.

---

## 4. Análisis de los bits enviados

Actualmente se están transmitiendo **20 bits**, lo cual equivale a **10 bytes por segundo**, y se representa en formato hexadecimal como:

```
2d 35 36 2c 31 36 34 2c 46 61 6c 73 65 2c 46 61 6c 73 65 0a
```

Esto corresponde a los valores:

```
- 5 6 , 1 6 4 , F a l s e , F a l s e
```

Aún no se evidencia una conversión al formato `'>2h2B'`, ya que los datos siguen apareciendo en hexadecimal con codificación ASCII. Bajo el formato `'>2h2B'`, se esperaría observar 12 bits, no 20, lo cual indicaría un cambio en la forma de envío.

---

## 5. Sobre el formato ‘>2h2B’

En este formato, el **primer byte** (debido al uso de big endian) es el que determina si el número es positivo o negativo.  
- Si comienza en `0`, el valor es positivo.  
- Si comienza en `1`, el valor es negativo.

---

## 6. Comparativa de binario vs ASCII

### Ventajas del uso de binario:
- Es el método más adecuado para la comunicación entre máquinas.
- Un número representado en binario puede ocupar tan solo 2 bytes, mientras que en ASCII ese mismo número siempre utilizará más espacio.

### Desventajas del uso de binario:
- Es necesario utilizar estructuras como `struct` para manejar estos datos correctamente, en lugar de simplemente manipular una cadena de texto.
- Puede resultar más confuso que usar directamente el formato ASCII, especialmente para principiantes.

### Ventajas del uso de ASCII:
- No es necesario conocer las reglas del binario, como las que se estudiaron en ensamblador.
- Mucho más legible para los humanos.

### Desventajas del uso de ASCII:
- Se transmite una mayor cantidad de datos (más bytes) en comparación con el formato binario para representar la misma información.
- La computadora debe realizar una conversión adicional para poder interpretar y trabajar con los datos en lenguaje de máquina.
