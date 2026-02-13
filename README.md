# 🚗 Proyecto 2do Parcial Codigo usado CAN BUS

## 📋 Resumen del Proyecto
El diagrama muestra un sistema de control automático de temperatura para ventilación de motor basado en comunicación CAN. Está formado por dos placas Arduino conectadas mediante módulos MCP2515 que permiten el intercambio de datos entre un nodo transmisor y uno receptor. El nodo transmisor mide la temperatura utilizando un sensor termistor NTC, procesa la señal y envía el valor obtenido a través del bus CAN.
El nodo receptor recibe la información en tiempo real, la muestra en una pantalla OLED para su monitoreo y la compara con un valor límite programado. Cuando la temperatura alcanza los 30 °C, el sistema activa automáticamente un relé de 5 V que enciende un ventilador, simulando el funcionamiento de un electroventilador automotriz.
Este proyecto evidencia la integración de sensores, comunicación digital y actuadores para lograr un control térmico eficiente, confiable y de bajo costo, demostrando principios fundamentales de la electrónica automotriz aplicada.


## 📸 Diagrama

<img width="708" height="896" alt="Captura de pantalla 2026-02-12 214512" src="https://github.com/user-attachments/assets/d6e9a8d6-c7e9-498c-a099-20da4d3d92b4" />

> **Nota:** En esta imagen se muestra el diagrama de conexión del sistema, donde se visualizan los módulos principales y sus enlaces eléctricos, permitiendo comprender de manera clara la estructura, funcionamiento y comunicación entre los componentes que conforman el proyecto.

## 🛠️ Componentes Utilizados
* **Hardware:** Arduino / MCP2515/ Pantalla Oled 128x64 I2c Display Lcd 0.96
* **Materiales:** Resistencia 10k / Ventilador 5V / Modulo Rele 5v / PROTOBOARD / Cables Dupont Macho Hembra/ Termistor NTC
  
## 🎥 Video Explicativo Proyecto 2do Parcial
https://www.youtube.com/watch?v=A4fW21p1sl4
