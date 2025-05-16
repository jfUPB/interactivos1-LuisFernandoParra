### Comparativa entre ASCII y Binario

| Aspecto         | ASCII                                                                                     | Binario                                                                                     | Ejemplo                                                                                                                           |
|-----------------|--------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------|
| **Eficiencia**  | ✖ Es un formato textual, así que la máquina necesita convertirlo para entenderlo.         | ✔ Es más eficiente porque ya está en formato comprensible por la máquina.                  | Al usar ASCII en el puerto serial, p5.js debía aplicar `trim()` y `split()`, y luego convertir el resultado a `int`.               |
| **Velocidad**   | ✖ Usa más bytes para representar el mismo mensaje, por lo tanto es más lento.             | ✔ Usa menos bytes, por lo que transmite los datos más rápido.                              | En p5.js se nota que con ASCII los datos demoran más en aparecer en consola, mientras que con binario la respuesta es inmediata.  |
| **Facilidad**   | ✔ Es más amigable para humanos gracias a las tablas de referencia.                        | ✖ Es más complejo de interpretar manualmente.                                               | Consultando la tabla, era más fácil entender los paquetes de datos enviados en ASCII.                                             |
| **Recursos**    | ✖ El procesamiento adicional implica mayor consumo de recursos.                           | ✔ Al ser directo, requiere menos esfuerzo del sistema.                                     | Con ASCII hay pasos extras para asegurar 4 datos, en cambio el binario ofrece paquetes más confiables y automáticos.              |

### ¿Por qué se necesita `framing` en binario?

El `framing` sirve para delimitar claramente el inicio y fin de un paquete de datos. Aunque sabemos que los paquetes tienen un tamaño fijo (por ejemplo, 8 bytes), es importante tener una marca clara para evitar errores. Esto permite al receptor identificar correctamente dónde comienza y termina cada paquete, especialmente cuando se reciben muchos datos seguidos.

### ¿Cómo funciona el `framing`?

- `serialBuffer.length >= 8`: se verifica que haya al menos 8 bytes en el buffer.
- `0xaa`: actúa como **header** o marcador de inicio de paquete.
- Se hace un `slice` para extraer 8 bytes y luego `splice` para eliminarlos del buffer.
- Dentro de esos 8 bytes, se separan 6 datos útiles y 1 byte para el checksum.
- Se calcula el checksum y se compara con el recibido para validar la integridad del paquete.

### ¿Qué es un carácter de sincronización?

Es un byte especial (como `0xaa`) que marca el inicio de un paquete. Su función es ayudar a emisor y receptor a estar sincronizados respecto a dónde empieza cada conjunto de datos.

### ¿Qué es el checksum y cuál es su propósito?

El **checksum** es un byte adicional al final del paquete que valida que todos los datos hayan llegado correctamente. Se calcula así:
1. Se suman todos los bytes del paquete.
2. Se aplica la operación `MOD 256` para mantener el resultado en un byte (8 bits).
3. Se compara este resultado con el checksum recibido para verificar integridad.

### ¿Qué hace la función `concat` en `readSerialData()`?

```js
let newData = port.readBytes(available);
serialBuffer = serialBuffer.concat(newData);
```
`concat` agrega los nuevos datos (almacenados en `newData`) al final del array `serialBuffer`, permitiendo acumular los datos recibidos.

### ¿Por qué se revisa si el buffer tiene al menos 8 bytes?

Porque ese es el tamaño mínimo necesario para procesar un paquete completo. Si no hay suficientes datos, no tiene sentido intentar leer.

### ¿Qué representa `0xaa` en el código?

Es el **header**, un valor binario (`10101010`) fácil de identificar que indica el comienzo de un paquete.

### ¿Qué hacen `shift` y `continue`?

Si el primer byte no es `0xaa`, `shift` lo elimina del buffer. Luego `continue` hace que el ciclo vuelva a comenzar desde el inicio, sin ejecutar el resto del código.

### ¿Qué hace `break` si hay menos de 8 bytes?

Interrumpe el ciclo completamente, ya que no hay suficientes datos para formar un paquete. Es una medida de seguridad.

### Diferencia entre `slice` y `splice`

- `slice`: extrae una porción del array sin modificar el original.
- `splice`: elimina elementos del array original.
Se usa primero `slice` para trabajar con los datos, luego `splice` para eliminarlos del buffer y evitar procesarlos de nuevo.

### ¿Cómo funciona `reduce`?

`reduce` toma dos valores:
- `acc` (acumulador): almacena el resultado parcial.
- `val`: es cada elemento del array.
La función suma todos los valores del array reduciéndolos a un solo valor total.

```js
const sum = array.reduce((acc, val) => acc + val, 0);
```

### ¿Por qué comparar el checksum enviado con el calculado?

Para asegurarse de que los datos no se corrompieron durante la transmisión. Si coinciden, el paquete fue recibido correctamente.

### ¿Qué hace `continue` en la verificación del checksum?

Si el checksum no coincide, `continue` hace que el ciclo ignore el resto de las instrucciones y vuelva a comenzar, descartando el paquete inválido.

### ¿Qué es un `DataView` y para qué sirve?

`DataView` es una herramienta para leer datos binarios desde un `ArrayBuffer` de forma estructurada. Permite interpretar los bytes como enteros, flotantes, etc., dependiendo del tipo de datos esperado.

```js
const view = new DataView(buffer);
const value = view.getUint8(0); // Lee el primer byte como un entero sin signo de 8 bits
```

### ¿Por qué convertir los datos en vez de usarlos directamente?

Porque los bytes puros no nos dicen nada sin contexto. Necesitamos saber cómo interpretarlos (como enteros, floats, etc.) para poder usarlos correctamente. Las conversiones permiten darle sentido a esos datos crudos del buffer.
