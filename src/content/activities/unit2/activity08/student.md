# **Explicación de la Máquina de Estados**  
El programa controla el parpadeo de dos píxeles en la matriz LED de un **Micro:bit** utilizando una **máquina de estados**.  
Cada píxel alterna entre **encendido y apagado** con un intervalo de tiempo específico.  

---

## **Identificación de Estados**  
Los estados representan momentos en los que el programa espera una condición antes de avanzar:  

- **`Init` (Inicialización)**  
  - Se configura el tiempo de inicio.    
  - Se cambia al estado **"WaitTimeout"**.  

- **`WaitTimeout` (Espera de tiempo)**  
  - Se espera hasta que transcurra el intervalo definido (`interval`).  
  - Una vez que el tiempo ha pasado, se alterna el estado del píxel (de apagado a encendido o viceversa).  

---

## **Identificación de Eventos**  
Los eventos son las condiciones que el programa evalúa en cada estado:  

1. **Si `state == "Init"`:**  
   - Se registra el tiempo actual (`startTime`).  
   - Se cambia el estado a **"WaitTimeout"**.  

2. **Si `state == "WaitTimeout"` y el tiempo transcurrido supera `interval`:**  
   - Se cambia el estado del píxel (si estaba encendido, se apaga y viceversa).  
   - Se actualiza la pantalla LED.  
   - Se reinicia el tiempo (`startTime`).  

---

## **Identificación de Acciones**  
Las acciones son las ejecuciones que ocurren como respuesta a los eventos:  

### **Acción al iniciar (`Init`)**  
- Se registra el tiempo de inicio.  
- Se cambia el estado a `"WaitTimeout"`.  
- Se muestra el píxel en la pantalla LED usando `display.set_pixel()`.  

### **Acción al cumplirse el tiempo de espera (`WaitTimeout`)**  
- Se reinicia el contador de tiempo.
- Se alterna el estado del píxel entre **encendido (9)** y **apagado (0)**.  
- Se actualiza la pantalla LED con el nuevo estado.  

---


