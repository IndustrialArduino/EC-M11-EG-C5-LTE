#include <Wire.h>
#define MODEM_RESET 32
#define MODEM_FLIGHT 22
#define MODEM_RX 26
#define MODEM_TX 25

#define DEVICE_ADDRESS 0x49

long timer1;

void setup() {
  // initialize both serial ports:
  Serial.begin(115200);
  pinMode(MODEM_FLIGHT, OUTPUT); // FLIGHT MODE ENABLE
  pinMode(MODEM_RESET, OUTPUT);    // MODEM RESET PIN
  digitalWrite(MODEM_FLIGHT, HIGH); // FLIGHT MODE

  MODEM_RESET_CYC();
  delay(2000);
  Serial2.begin(115200, SERIAL_8N1, MODEM_RX, MODEM_TX);
  Serial.println("SIM AT START >>>>>>>>>>>>>>");
  delay(2000);
  Serial2.println("AT");
  delay(2000);
  Serial2.println("AT+CPIN?");
  delay(2000);
  Serial2.println("AT+CNMP?");

  Wire.begin(16, 17);  // SDA on GPIO16, SCL on GPIO17
  Serial.begin(115200);
}

void loop() {
  delay(3000);

  timer1 = millis();
  Serial2.println("AT");
  while (millis() < timer1 + 10000) {
    while (Serial2.available()) {
      int inByte = Serial2.read();
      Serial.write(inByte);
    }
  }

  timer1 = millis();
  Serial2.println("AT+CPIN?");
  while (millis() < timer1 + 10000) {
    while (Serial2.available()) {
      int inByte = Serial2.read();
      Serial.write(inByte);
    }
  }

  Serial.println("AT SCAN DONE");

  Wire.beginTransmission(DEVICE_ADDRESS);
  Wire.write(0x01);  // Replace with your data
  Wire.write(0x02);
  Wire.write(0x03);
  Wire.endTransmission();
  delay(100);

  Wire.requestFrom(DEVICE_ADDRESS, 3);  // 3 bytes of data
  if (Wire.available() >= 3) {
    byte data1 = Wire.read();
    byte data2 = Wire.read();
    byte data3 = Wire.read();
    Serial.print("Read Data: ");
    Serial.print(data1, HEX);
    Serial.print(" ");
    Serial.print(data2, HEX);
    Serial.print(" ");
    Serial.println(data3, HEX);
  }

  delay(1000);
}

void MODEM_RESET_CYC() {
  digitalWrite(MODEM_RESET, HIGH);
  delay(1000);
  digitalWrite(MODEM_RESET, LOW);
  delay(1000);
  digitalWrite(MODEM_RESET, HIGH);
}
