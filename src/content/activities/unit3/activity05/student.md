# Modelo de la Aplicación: Control de Bomba con p5.js y micro:bit

```plaintext
+---------------------------+
|   Interfaz en p5.js       |
|---------------------------|
|                           |
|  [Connect to micro:bit]   | ---> Establece / Cierra conexión serial (115200 baudios)
|  [A]                      | ---> Envia carácter 'A' por serial
|  [B]                      | ---> Envia carácter 'B' por serial
|  [S]                      | ---> Envia carácter 'S' por serial (Start Pump)
|  [T]                      | ---> Envia carácter 'T' por serial (Stop Pump)
+---------------------------+
              |
              v
+---------------------------+
|        micro:bit          |
|---------------------------|
|  Recibe caracteres por    |
|  puerto serial (ASCII)    |
|                           |
|  Si recibe 'A' -> Acción A|
|  Si recibe 'B' -> Acción B|
|  Si recibe 'S' -> Encender|
|                   bomba   |
|  Si recibe 'T' -> Apagar  |
|                   bomba   |
+---------------------------+
              |
              v
+---------------------------+
|          Bomba            |
|---------------------------|
|  Encendida / Apagada por  |
|  comandos recibidos desde |
|  la micro:bit             |
+---------------------------+
