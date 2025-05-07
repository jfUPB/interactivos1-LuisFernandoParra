#  Análisis: Máquina de Estados y Pruebas en la Implementación de la Bomba

##  ¿Por qué es poderosa la técnica de máquina de estados para la escalabilidad de la aplicación?

La técnica de **máquina de estados** es una herramienta poderosa en el desarrollo de aplicaciones reactivas como esta porque:

###  Ventajas en **concurrencia**:
- **Aisla comportamientos por estado**: Cada estado define claramente qué eventos acepta y cómo reacciona, lo que evita condiciones de carrera o respuestas no deseadas.
- **Simplifica el manejo de múltiples fuentes de eventos** (botones, sensores, p5.js, etc.): al identificar claramente el estado actual, es fácil decidir cómo debe actuar el sistema ante cualquier evento recibido.
- **Permite un ciclo de actualización simple** (`update()`), que verifica el estado y reacciona, facilitando su integración con otras máquinas o sistemas concurrentes.

###  Ventajas en **escalabilidad**:
- **Fácil extensión**: Agregar nuevos estados o transiciones no afecta los ya existentes si están bien encapsulados.
- **Mantenibilidad**: Al separar la lógica por estados, el código es más modular y comprensible.
- **Posibilidad de anidar o componer máquinas de estados**: lo cual permite modelar sistemas más complejos a futuro, como múltiples bombas o bombas con configuraciones avanzadas.

---

##  Ventajas y desventajas del tipo de pruebas realizadas

###  Ventajas:
- **Cobertura completa de estados**: Se probaron todos los caminos posibles entre estados (`config`, `LETS GO`, `bombaexplotada`).
- **Detección temprana de errores**: Especialmente errores lógicos, como secuencias incorrectas o reinicios fallidos.
- **Inclusión de múltiples fuentes de eventos**: Asegurando compatibilidad tanto desde `p5.js` como desde `micro:bit`.

###  Desventajas:
- **Pruebas manuales o semiautomáticas**: No se automatizaron todos los vectores de prueba, lo que deja espacio a errores humanos.
- **Falta de pruebas con múltiples eventos simultáneos**: No se simularon condiciones más extremas de concurrencia.
- **Dependen del seguimiento manual de estados y salidas**: lo que podría no ser eficiente a escala.

---

##  Importancia de las pruebas de regresión

Las pruebas de regresión son **fundamentales** porque permiten verificar que los **cambios realizados no rompen funcionalidades que ya estaban funcionando correctamente**.

###  ¿Qué sucede si no haces pruebas de regresión?
- **Se pueden introducir errores silenciosos** que afecten funcionalidades previas sin que te des cuenta.
- **El sistema se vuelve menos confiable** con el tiempo.
- **Los bugs acumulados aumentan la dificultad de mantenimiento** y afectan la experiencia del usuario.

Al realizar pruebas de regresión después de cada corrección, se garantiza la **estabilidad y robustez** del sistema.

---

