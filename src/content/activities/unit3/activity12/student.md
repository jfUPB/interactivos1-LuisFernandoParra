#  Autoevaluación – Unidad: Control de sistemas con máquina de estados (p5.js + Micro:bit)

##  ¿Qué tanto aprendí en esta unidad?

Considero que aprendí **bastante en esta unidad**, ya que logré comprender y aplicar conceptos fundamentales de diseño de software como el uso de **máquinas de estados**, **manejo de eventos**, y la **comunicación entre dispositivos (p5.js y micro:bit)**.

---

##  Conceptos que aprendí

### 1. **Máquina de estados**
**Aprendizaje:** Comprendí cómo diseñar e implementar una máquina de estados para controlar el comportamiento de un sistema de forma modular y clara.

**Ejemplo:** Implementé los tres estados (`config`, `LETS GO`, `bombaexplotada`) en una clase que actualizaba su comportamiento dependiendo del evento recibido. Esto facilitó el control de la bomba tanto en p5.js como en micro:bit.

---

### 2. **Manejo de múltiples fuentes de eventos**
**Aprendizaje:** Aprendí a gestionar eventos provenientes de diferentes orígenes como botones físicos del micro:bit, gestos (shake), entradas por UART y botones desde el entorno gráfico de p5.js.

**Ejemplo:** Diseñé un sistema que reaccionaba a eventos `A`, `B`, `S`, `T` desde ambas plataformas sin interferencia entre ellos, lo que me permitió simular interactividad real.

---

### 3. **Pruebas funcionales y pruebas de regresión**
**Aprendizaje:** Comprendí cómo estructurar y documentar pruebas que validan los diferentes estados de la aplicación. También entendí la importancia de repetir las pruebas tras hacer cambios.

**Ejemplo:** Después de detectar un error en la secuencia de usuario, documenté la falla, la solución aplicada y ejecuté nuevamente todas las pruebas como parte de una regresión.

---

##  Conceptos que no aprendí completamente

### 1. **Automatización de pruebas**
**Razón:** Aunque realicé las pruebas funcionales manualmente y documenté sus resultados, no supe cómo automatizarlas para que se ejecuten sin intervención manual cada vez.

**Ejemplo:** Las pruebas de estado se ejecutaron con inputs manuales desde p5.js o desde el micro:bit, lo cual podría ser más eficiente con pruebas automatizadas usando herramientas como `jest`, `mocha` o pruebas simuladas en consola.

---

### 2. **Manejo avanzado de concurrencia**
**Razón:** Aunque entendí los conceptos básicos de concurrencia al recibir eventos de múltiples fuentes, no exploré tema.

**Ejemplo:** No pude simular correctamente dos eventos sucediendo al mismo tiempo (por ejemplo, botón y sacudida), lo cual puede ser crítico en una aplicación en tiempo real.

---

##  Estrategias para mejorar

| Concepto                  | Estrategia                                                                 |
|---------------------------|---------------------------------------------------------------------------|
| Recursos que usaré        | - Documentación oficial de p5.js<br>- Libros/artículos sobre programación reactiva<br>- Videos en YouTube sobre testing en JS y manejo de eventos. |
| Tiempo dedicado           | 3 horas semanales fuera de clase                                 |
| Fecha de inicio           | Lunes próximo   |

---

