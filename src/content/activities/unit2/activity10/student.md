# Explicación de la Máquina de Estados y Concurrencia

## Máquina de Estados para los Botones  
El programa utiliza una **máquina de estados** para gestionar las transiciones entre diferentes comportamientos. Cada estado representa una acción específica (ej: `STATE_HAPPY`, `STATE_SAD`), y las transiciones se activan por eventos temporales o interrupciones de botones.

## Concurrencia en el Programa  
La simulación de concurrencia se logra combinando:  
1. **Temporizadores**: Mediante `utime.ticks_ms()`, se mide el tiempo transcurrido para cambiar entre estados después de intervalos predefinidos.  
2. **Evaluación Secuencial**: En cada iteración del ciclo principal (`while True:`), se verifica el estado actual y se decide si se debe cambiar a otro estado, ya sea por tiempo o por pulsación del botón A.  
3. **No Bloqueo**: Al alternar rápidamente entre estados, el sistema evita bloqueos, permitiendo múltiples "procesos" aparentemente simultáneos.

---

# Vectores de Prueba

## Caso 1: Cambio de Estado por Temporización  
- **Condiciones Iniciales**:  
  - Estado: `STATE_HAPPY`.  
  - Pantalla: Muestra `Image.HAPPY`.  
- **Evento**: Esperar **1500 ms** sin interactuar.  
- **Resultado Esperado**: Transición a `STATE_SMILE` con `Image.SMILE`.  
- **Resultado Obtenido**: ✔️ Éxito.  

## Caso 2: Interrupción por Botón en `STATE_HAPPY`  
- **Condiciones Iniciales**:  
  - Estado: `STATE_HAPPY`.  
- **Evento**: Presionar botón **A** antes de los 1500 ms.  
- **Resultado Esperado**: Cambio inmediato a `STATE_SAD` (ignora temporizador).  
- **Resultado Obtenido**: ✔️ Transición instantánea.  

## Caso 3: Secuencia Completa de Estados  
- **Condiciones Iniciales**:  
  - Estado: `STATE_INIT`.  
- **Evento**: No presionar botones.  
- **Resultado Esperado**: Secuencia cíclica:  
  `STATE_HAPPY` → `STATE_SMILE` → `STATE_SAD` → `STATE_HAPPY`.  
- **Resultado Obtenido**: ✔️ Ciclo ejecutado sin errores.  

---
