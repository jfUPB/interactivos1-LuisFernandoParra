##  Actividad 01 – Ejemplo 1: Generative Design

###  Enlace original:
http://www.generative-gestaltung.de/2/sketches/?01_P/P_2_3_3_01

###  ¿Qué me llamó la atención?
Este sketch convierte una palabra en una tipografía generativa, donde las letras parecen hechas con hilos vibrantes que reaccionan al movimiento del mouse. Me pareció impactante cómo logra una interacción tan fluida con tan pocos elementos gráficos, haciendo que un texto estático cobre vida de forma elegante y dinámica.

###  Análisis técnico
El sketch fue hecho con *p5.js* y utiliza varias funciones importantes del entorno:

- createFont() y textFont() (en la versión original de Processing; en p5.js se reemplaza con loadFont() y textFont()).
- textToPoints(): convierte el texto en un conjunto de puntos que representan su contorno.
- noise() y map(): se usan para crear distorsiones suaves y naturales en los puntos.
- dist(): mide la distancia entre el mouse y cada punto, para aplicar un efecto en función de la proximidad.
- beginShape(), vertex(), endShape(): permiten construir las líneas conectando puntos distorsionados.

El sketch se basa en transformar texto en puntos y manipular esos puntos visualmente para que parezca que las letras se mueven o respiran.

###  Modificación realizada



*Cambios realizados:*
- Reemplacé las líneas rectas entre puntos por *curvas suaves*, para que las letras tengan un estilo más orgánico y artístico.
- Implementé un *gradiente de color* en los trazos, que cambia según la posición vertical del mouse (mouseY). Esto agrega más dinamismo visual y resalta la interactividad del sketch.

###  Justificación de los cambios:
Decidí usar curvas en lugar de líneas rectas para dar una sensación más fluida al contorno de las letras, similar a trazos hechos a mano. Además, el gradiente de color genera un vínculo directo entre el movimiento del usuario y el resultado visual, lo que aumenta la sensación de control e inmersión en la pieza.

##  Actividad 01 – Ejemplo 2: p5.js Example (Generative Design)

###  Enlace original:
https://editor.p5js.org/generative-design/sketches/P_1_0_01

###  ¿Qué me llamó la atención?
Este ejemplo muestra una cuadrícula de formas abstractas compuestas por líneas y círculos, que cambian su orientación y estilo en cada ejecución. Me gustó mucho porque recuerda a las composiciones del arte generativo moderno, donde patrones simples se combinan para formar algo visualmente complejo. Tiene un equilibrio entre orden y aleatoriedad que lo hace visualmente atractivo.

###  Análisis técnico
Este sketch utiliza conceptos fundamentales de p5.js:

- createCanvas() y background() para establecer el lienzo.
- random() para introducir variaciones aleatorias en cada forma.
- push() y pop() para manejar las transformaciones sin afectar el resto del dibujo.
- translate() y rotate() para colocar y orientar cada figura dentro de la cuadrícula.

El sketch trabaja con una cuadrícula de 10x10 y dentro de cada celda se dibuja una figura con distintos patrones.

---

### 🛠 Modificación realizada

*Cambios realizados:*
- Reemplacé algunas de las figuras básicas por una combinación de *arcos* y *líneas curvas*, para darle un aspecto más dinámico y visualmente complejo.
- Cambié el número de columnas y filas de 10x10 a 12x8 para experimentar con la proporción del lienzo.
- Añadí un botón para *generar una nueva variación* sin tener que recargar la página.



