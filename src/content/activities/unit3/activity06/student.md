# Vectores de Prueba 

---

### 🔹 Vector 1  
**Si estoy en** `ESTADO_config`  
**y recibo el evento** `letra = "A"` (presiono botón A)  
**entonces deben ocurrir estas acciones:**  
- Incrementar `time` en 1  
- Mostrar el nuevo valor en el display  
**y debo ir al** `ESTADO_config`  

---

### 🔹 Vector 2  
**Si estoy en** `ESTADO_config`  
**y recibo el evento** `letra = "B"` (presiono botón B)  
**entonces deben ocurrir estas acciones:**  
- Decrementar `time` en 1  
- Mostrar el nuevo valor en el display  
**y debo ir al** `ESTADO_config`  

---

### 🔹 Vector 3  
**Si estoy en** `ESTADO_config`  
**y recibo el evento** `letra = "S"` (agito el microbit o llega por UART)  
**entonces deben ocurrir estas acciones:**  
- Guardar el tiempo de inicio (`startTime`)  
- Cambiar a cuenta regresiva  
**y debo ir al** `ESTADO_LETS GO`  

---

### 🔹 Vector 4  
**Si estoy en** `ESTADO_LETS GO`  
**y recibo el evento** `letra = "A"`  
**entonces deben ocurrir estas acciones:**  
- Agregar `"A"` a `secuenciaUsuario`  
- Decrementar `time`  
- Mostrar nuevo tiempo  
**y debo ir al** `ESTADO_LETS GO`  

---

### 🔹 Vector 5  
**Si estoy en** `ESTADO_LETS GO`  
**y recibo el evento** `letra = "B"`  
**entonces deben ocurrir estas acciones:**  
- Agregar `"B"` a `secuenciaUsuario`  
- Decrementar `time`  
- Mostrar nuevo tiempo  
**y debo ir al** `ESTADO_LETS GO`  

---

### 🔹 Vector 6  
**Si estoy en** `ESTADO_LETS GO`  
**y recibo el evento** `letra = "S"`  
**entonces deben ocurrir estas acciones:**  
- Agregar `"SHAKE"` a `secuencia
