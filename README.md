# 🚗 Proyecto 2do Parcial Codigo usado CAN BUS

## 📋 Resumen del Proyecto
El proyecto consiste en el diseño e implementación de un sistema electrónico de control automático de ventilación para un motor, basado en comunicación CAN, el cual permite monitorear y regular la temperatura de forma eficiente. El sistema utiliza un termistor NTC como sensor térmico, cuya señal es procesada por un Arduino encargado de calcular la temperatura y transmitirla mediante un módulo MCP2515 a través del bus CAN hacia un segundo nodo receptor.En el nodo receptor, otro microcontrolador interpreta los datos recibidos, muestra la temperatura en tiempo real en una pantalla OLED y compara el valor con un umbral establecido. Cuando la temperatura alcanza los 30 °C, el sistema activa automáticamente un relé de 5 V que enciende el electroventilador, simulando el funcionamiento de un sistema de enfriamiento automotriz real. Este proceso ocurre de manera inmediata gracias a la estabilidad y velocidad de la comunicación CAN. 


## 📸 Diagrama

<img width="708" height="896" alt="Captura de pantalla 2026-02-12 214512" src="https://github.com/user-attachments/assets/d6e9a8d6-c7e9-498c-a099-20da4d3d92b4" />

> **Nota:** En esta imagen se muestra el diagrama de conexión del sistema, donde se visualizan los módulos principales y sus enlaces eléctricos, permitiendo comprender de manera clara la estructura, funcionamiento y comunicación entre los componentes que conforman el proyecto.

## 🛠️ Componentes Utilizados
* **Hardware:** Arduino / MCP2515/ Pantalla Oled 128x64 I2c Display Lcd 0.96
* **Materiales:** Resistencia 10k / Ventilador 5V / Modulo Rele 5v / PROTOBOARD / Cables Dupont Macho Hembra/ Termistor NTC
  
## 🎥 Video Explicativo del proyecto 2do Parcial
https://www.youtube.com/watch?v=A4fW21p1sl4
