#  Pruebas y Vectores de Validación – Control de Bomba con p5.js + Micro:bit


---

##  1. Vectores de prueba

A continuación, se presenta una tabla con todos los vectores de prueba definidos, considerando los **estados del sistema**, **eventos esperados**, **fuentes de eventos** (micro:bit o p5.js) y el **resultado esperado**.

### Estado: `config`

| # | Evento | Fuente     | Acción esperada               | Cambio de estado |
|---|--------|------------|-------------------------------|------------------|
| 1 | `A`    | p5.js      | Aumenta tiempo en 1           | Sigue en config  |
| 2 | `B`    | p5.js      | Disminuye tiempo en 1         | Sigue en config  |
| 3 | `S`    | p5.js      | Inicia cuenta regresiva       | A "LETS GO"      |
| 4 | `T`    | p5.js      | No hace nada                  | Sigue en config  |
| 5 | `A`    | micro:bit  | Aumenta tiempo en 1           | Sigue en config  |
| 6 | `B`    | micro:bit  | Disminuye tiempo en 1         | Sigue en config  |
| 7 | `S`    | micro:bit  | Inicia cuenta regresiva       | A "LETS GO"      |
| 8 | `T`    | micro:bit  | No hace nada                  | Sigue en config  |

### Estado: `LETS GO`

| #  | Evento                        | Fuente     | Acción esperada                         | Cambio de estado |
|----|-------------------------------|------------|------------------------------------------|------------------|
| 9  | `A`                           | p5.js      | Agrega "A" a secuenciaUsuario           | Sigue en LETS GO |
| 10 | `B`                           | p5.js      | Agrega "B" a secuenciaUsuario           | Sigue en LETS GO |
| 11 | `S`                           | p5.js      | Agrega "SHAKE" a secuenciaUsuario       | Sigue en LETS GO |
| 12 | `T`                           | p5.js      | No hace nada                            | Sigue en LETS GO |
| 13 | `A`, `B`, `A`, `SHAKE`        | Ambos      | Coincide con secuencia esperada         | A "config"       |
| 14 | Secuencia incorrecta         | Ambos      | Limpia `secuenciaUsuario`               | Sigue en LETS GO |
| 15 | Tiempo llega a `0`           | -          | Explota bomba, muestra alarma visual    | A "bombaexplotada" |

### Estado: `bombaexplotada`

| #  | Evento    | Fuente     | Acción esperada               | Cambio de estado |
|----|-----------|------------|-------------------------------|------------------|
| 16 | `T`       | p5.js      | Reinicia tiempo y sistema     | A "config"       |
| 17 | `T`       | micro:bit  | Reinicia tiempo y sistema     | A "config"       |
| 18 | `A/B/S`   | Ambos      | No tienen efecto              | Sigue igual      |

---

##  2. Pruebas de regresión

###  Falla detectada:

Durante el vector de prueba **#14**, al introducir una **secuencia incorrecta**, el sistema no reiniciaba `secuenciaUsuario`, provocando errores en verificaciones posteriores.

###  Corrección aplicada:

Se agregó el siguiente bloque en la verificación de secuencia:

```javascript
if (secuenciaUsuario.length >= secuencia.length) {
  if (JSON.stringify(secuenciaUsuario) === JSON.stringify(secuencia)) {
    this.state = "config";
  } else {
    secuenciaUsuario = [];
  }
}
