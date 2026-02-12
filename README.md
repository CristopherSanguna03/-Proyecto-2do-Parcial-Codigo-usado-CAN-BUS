# -Proyecto-2do-Parcial-Codigo-usado-CAN-BUS
 Proyecto 2do Parcial Codigo usado CAN BUS Control de Ventilación del Motor
#include <SPI.h>
#include <mcp2515.h>
#include <math.h>

#define NTC_PIN A0
#define CS_PIN 10

// Parámetros del NTC
#define R_FIXED 10000.0
#define R0 10000.0
#define BETA 3950.0
#define T0 298.15   // 25°C en Kelvin

struct can_frame canMsg;
MCP2515 mcp2515(CS_PIN);

void setup() {
  Serial.begin(9600);
  SPI.begin();

  mcp2515.reset();
  mcp2515.setBitrate(CAN_500KBPS, MCP_8MHZ);
  mcp2515.setNormalMode();

  Serial.println("CAN EMISOR listo (NTC invertido)");
}

void loop() {
  int adc = analogRead(NTC_PIN);

  // 🔁 LOGICA INVERTIDA
  float voltage = adc * (5.0 / 1023.0);

  // NTC ARRIBA, resistencia fija abajo
  float r_ntc = R_FIXED * (voltage / (5.0 - voltage));

  float tempK = 1.0 / ((1.0 / T0) + (1.0 / BETA) * log(r_ntc / R0));
  float tempC = tempK - 273.15;

  int tempSend = (int)(tempC * 10); // ej 27.5°C → 275

  canMsg.can_id  = 0x100;
  canMsg.can_dlc = 2;
  canMsg.data[0] = highByte(tempSend);
  canMsg.data[1] = lowByte(tempSend);

  mcp2515.sendMessage(&canMsg);

  Serial.print("ADC: ");
  Serial.print(adc);
  Serial.print("  Temp enviada: ");
  Serial.print(tempC);
  Serial.println(" C");

  delay(1000);
}
                    //RECEPTOR//
#include <SPI.h>
#include <mcp2515.h>
#include <Wire.h>
#include <Adafruit_GFX.h>
#include <Adafruit_SSD1306.h>

#define CS_PIN 10
#define RELAY_PIN 8

// Dimensiones típicas de la OLED 0.98" (ajusta si la tuya es distinta)
#define SCREEN_WIDTH 128
#define SCREEN_HEIGHT 64

#define BLACK 0
#define WHITE 1

#define TEMP_MIN 0.0
#define TEMP_MAX 50.0
#define TEMP_ALERT 30.0

struct can_frame canMsg;
MCP2515 mcp2515(CS_PIN);
Adafruit_SSD1306 display(SCREEN_WIDTH, SCREEN_HEIGHT, &Wire, -1);

void drawThermometer(int x, int y, float temp) {
  // Termómetro ajustado para tamaño de pantalla
  display.drawRect(x + 1, y, 5, 22, WHITE);
  display.fillCircle(x + 3, y + 25, 5, WHITE);
  // Línea central variable
  int lineHeight = map(constrain(temp, TEMP_MIN, TEMP_MAX), TEMP_MIN, TEMP_MAX, 2, 18);
  display.drawLine(x + 3, y + 23 - lineHeight, x + 3, y + 18, WHITE);
}

void drawProgressBar(int x, int y, int width, int height, float temp) {
  display.drawRect(x, y, width, height, WHITE);
  int barWidth = map(constrain(temp, TEMP_MIN, TEMP_MAX), TEMP_MIN, TEMP_MAX, 0, width);
  barWidth = constrain(barWidth, 0, width);
  display.fillRect(x + 1, y + 1, barWidth, height - 2, WHITE);
  // Resalte en alerta: doble relleno
  if (temp >= TEMP_ALERT) display.fillRect(x + 3, y + 3, barWidth - 4, height - 6, WHITE);
}

void setup() {
  Serial.begin(9600);
  SPI.begin();
  pinMode(RELAY_PIN, OUTPUT);
  digitalWrite(RELAY_PIN, HIGH);

  mcp2515.reset();
  mcp2515.setBitrate(CAN_500KBPS, MCP_8MHZ);
  mcp2515.setNormalMode();

  if (!display.begin(SSD1306_SWITCHCAPVCC, 0x3C)) while (1);
  display.clearDisplay();
  display.display();
}

void loop() {
  if (mcp2515.readMessage(&canMsg) == MCP2515::ERROR_OK) {
    if (canMsg.can_id == 0x100 && canMsg.can_dlc == 2) {
      int tempRaw = (canMsg.data[0] << 8) | canMsg.data[1];
      float tempC = tempRaw / 10.0;

      display.clearDisplay();

      // Título ajustado para no ocupar demasiado espacio
      display.setTextSize(1);
      display.setTextColor(WHITE);
      display.setCursor(38, 2);
      display.print("TEMPERATURA");

      // Termómetro en posición óptima para la pantalla
      drawThermometer(10, 15, tempC);

      // Valor de temperatura con resalte en alerta (optimizado para 0.98")
      display.setTextSize(2);
      String tempText = String(tempC, 1) + (char)247 + "C";
      int xPos = 42;
      int yPos = 25;

      if (tempC >= TEMP_ALERT) {
        // Simulación rojo: fondo blanco completo + texto negro
        // Calculo preciso del ancho para que quepa perfectamente
        int textWidth = tempText.length() * 11;
        display.fillRect(xPos - 2, yPos - 2, textWidth + 4, 15, WHITE);
        display.setTextColor(BLACK, WHITE);
      } else {
        display.setTextColor(WHITE);
      }
      display.setCursor(xPos, yPos);
      display.print(tempText);

      // Barra de progreso en la parte inferior (sin desbordar)
      drawProgressBar(10, 55, 108, 6, tempC);

      display.display();

      // Control del relé
      digitalWrite(RELAY_PIN, (tempC >= TEMP_ALERT) ? LOW : HIGH);
    }
  }
}                 
