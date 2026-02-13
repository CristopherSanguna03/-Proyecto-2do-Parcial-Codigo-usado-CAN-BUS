# 🚗 Proyecto 2do Parcial Codigo usado CAN BUS

## 📋 Resumen del Proyecto
![oled_anim](https://github.com/user-attachments/assets/08aefe22-49eb-4fab-af71-c04eb1b10d52)

El proyecto consiste en el diseño e implementación de un sistema electrónico de control automático de ventilación para motores, basado en comunicación CAN, con el objetivo de monitorear y regular la temperatura de manera eficiente. El sistema emplea un sensor termistor NTC para medir la temperatura, un Arduino transmisor que procesa los datos y los envía mediante un módulo MCP2515 a través del bus CAN, y un Arduino receptor que interpreta la información recibida. Este último muestra la temperatura en tiempo real en una pantalla OLED y la compara con un valor umbral programado. Cuando la medición alcanza los 30 °C, se activa automáticamente un relé de 5 V que enciende un ventilador, simulando el funcionamiento de un electroventilador automotriz. El desarrollo demuestra la integración efectiva de sensores, comunicación digital y actuadores, logrando un sistema confiable, preciso y de bajo costo aplicable a soluciones de control térmico en el ámbito automotriz.


## 📸 Diagrama

<img width="708" height="896" alt="Captura de pantalla 2026-02-12 214512" src="https://github.com/user-attachments/assets/d6e9a8d6-c7e9-498c-a099-20da4d3d92b4" />

> **Nota:** En esta imagen se muestra el diagrama de conexión del sistema, donde se visualizan los módulos principales y sus enlaces eléctricos, permitiendo comprender de manera clara la estructura, funcionamiento y comunicación entre los componentes que conforman el proyecto.

## 🛠️ Componentes Utilizados
* **Hardware:** Arduino / MCP2515/ Pantalla Oled 128x64 I2c Display Lcd 0.96
* **Materiales:** Resistencia 10k / Ventilador 5V / Modulo Rele 5v / PROTOBOARD / Cables Dupont Macho Hembra/ Termistor NTC
  
## 🎥 Video Explicativo Proyecto 2do Parcial
https://www.youtube.com/watch?v=A4fW21p1sl4
