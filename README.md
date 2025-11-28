#include <DHT.h>

#define DHTPIN 2        // DHT sensor pin
#define DHTTYPE DHT11   // Or DHT22
DHT dht(DHTPIN, DHTTYPE);

int smokeSensor = A0;   // MQ-2 analog pin
int buzzer = 8;         // Buzzer pin
int led = 7;            // LED pin

void setup() {
  Serial.begin(9600);
  dht.begin();
  pinMode(smokeSensor, INPUT);
  pinMode(buzzer, OUTPUT);
  pinMode(led, OUTPUT);
}

void loop() {
  float temp = dht.readTemperature();
  int smokeLevel = analogRead(smokeSensor);

  Serial.print("Temperature: ");
  Serial.print(temp);
  Serial.print(" °C | Smoke Level: ");
  Serial.println(smokeLevel);

  // Fire detection condition
  if (temp > 45 || smokeLevel > 300) {
    digitalWrite(buzzer, HIGH);
    digitalWrite(led, HIGH);
    Serial.println("🔥 FIRE ALERT!");
  } else {
    digitalWrite(buzzer, LOW);
    digitalWrite(led, LOW);
  }

  delay(1000);
}