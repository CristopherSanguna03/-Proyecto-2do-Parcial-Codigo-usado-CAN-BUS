            //EMISOR//
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

  float voltage = adc * (5.0 / 1023.0);

  float r_ntc = R_FIXED * (voltage / (5.0 - voltage));

  float tempK = 1.0 / ((1.0 / T0) + (1.0 / BETA) * log(r_ntc / R0));
  float tempC = tempK - 273.15;

  int tempSend = (int)(tempC * 10);

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
                         
