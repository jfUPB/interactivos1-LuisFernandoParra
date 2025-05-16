# Actividad 7

## Autoevaluación de Objetivos de Aprendizaje

| Objetivo de Aprendizaje                                                                 | Puntuación (1–5) |
|------------------------------------------------------------------------------------------|------------------|
| Configurar y usar VS Code Dev Tunnels                                                    | 4                |
| Implementar arquitectura cliente-servidor (móvil -> servidor)                           | 5                |
| Usar Socket.IO para retransmitir datos (servidor -> escritorio)                         | 5                |
| Capturar y procesar eventos en el móvil (p5.js)                                          | 4                |
| Modificar sistema interactivo para crear la experiencia                                 | 4                |
| Analizar y explicar flujo de datos completo (móvil -> servidor -> escritorio)           | 5                |

---


###  ¿Qué concepto o actividad fue más fácil de entender o realizar?

Lo que me resultó más fácil fue *usar Socket.IO para conectar los clientes* y retransmitir mensajes. 

---

###  ¿Qué fue lo más difícil?

Lo que más me costó fue *configurar correctamente los Dev Tunnels*. Al principio no entendía bien cómo enlazar el servidor local con el móvil, y tuve problemas de conexión hasta que confirmé que los puertos estaban correctamente expuestos y que estaba usando la dirección correcta en el cliente móvil.  
Para resolverlo:

- Revisé dos veces la documentación de Dev Tunnels.
- Probé con diferentes puertos hasta que encontré una configuración estable.
- Usé console.log() en diferentes puntos del código para verificar si los mensajes llegaban correctamente.


---

###  Explicación del flujo de datos (como si lo contara a alguien externo al curso)

Imagina que tienes dos pantallas: una es tu celular y la otra es tu computador. En tu celular tocas la pantalla, eliges un color y decides si quieres que el dibujo tenga borde o no.  
Esa información (posición, color y si lleva borde) se empaqueta como un mensaje y se manda por internet usando una tecnología llamada *Socket.IO*.

Ese mensaje viaja hasta un *servidor* creado con *Node.js*, que actúa como intermediario: no hace nada con la información, solo la toma y se la pasa al otro cliente (el computador).  
El computador recibe el mensaje y, usando la librería *p5.js*, dibuja una partícula con esas características (posición, color, borde). Es como si el celular fuera un control remoto que le dice al computador qué dibujar, y este lo hace al instante.

Para que ambos dispositivos se puedan comunicar aunque estén en redes distintas, usamos una herramienta llamada *Dev Tunnels*, que abre una especie de “puerta pública” a nuestro servidor local.

---

###  ¿Cómo aplicaría esto en otros proyectos?

Puedo imaginar varios usos para lo aprendido en esta unidad:

- Crear *experiencias interactivas en museos o eventos*, donde los asistentes controlan una pantalla grande desde su celular sin tener que instalar nada.
- Juegos multijugador simples donde el celular se convierte en el control (por ejemplo, para mover un objeto en la pantalla principal).
- Aplicaciones educativas donde los estudiantes envían respuestas o controlan animaciones desde sus celulares en tiempo real.
- Usar sensores del móvil (como el acelerómetro) para enviar datos a una visualización en tiempo real en el computador o proyector.

Me gustó mucho ver cómo diferentes tecnologías se conectan para lograr una experiencia fluida entre dispositivos.

---

