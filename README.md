# RTOS-Based-Fire-Detection-and-Alarm-System

https://github.com/user-attachments/assets/08b5148c-c7cf-4548-a716-c579079b0934

#define BLYNK_PRINT Serial

#define BLYNK_TEMPLATE_ID "TMPL3NAl55JHW"
#define BLYNK_TEMPLATE_NAME "fire detection system"
#define BLYNK_AUTH_TOKEN "03-QxU7-OBHKwYWB0sfxObXMRnUoO_le"

#include <WiFi.h>
#include <BlynkSimpleEsp32.h>

// WiFi Credentials
char ssid[] = "vivo ab";
char pass[] = "vaathupk2709";

// Pins
#define FLAME_SENSOR 27
#define SMOKE_SENSOR 34
#define LED 26
#define BUZZER 25

int threshold = 600;

void setup() {
  Serial.begin(115200);

  pinMode(FLAME_SENSOR, INPUT);
  pinMode(LED, OUTPUT);
  pinMode(BUZZER, OUTPUT);

  digitalWrite(LED, LOW);
  digitalWrite(BUZZER, LOW);

  // Connect to Blynk
  Blynk.begin(BLYNK_AUTH_TOKEN, ssid, pass);

  Serial.println("Fire Detection with Blynk Started");
}

void loop() {
  Blynk.run();

  int flameState = digitalRead(FLAME_SENSOR);
  int smokeValue = analogRead(SMOKE_SENSOR);

  Serial.print("Flame: ");
  Serial.print(flameState);

  Serial.print("  Smoke: ");
  Serial.println(smokeValue);

  // Send data to Blynk
  Blynk.virtualWrite(V0, smokeValue);   // Smoke Sensor
  Blynk.virtualWrite(V1, flameState);   // Flame Sensor

  if (flameState == LOW && smokeValue > threshold) {

    Serial.println("FIRE DETECTED!");

    digitalWrite(LED, HIGH);
    digitalWrite(BUZZER, HIGH);

    Blynk.virtualWrite(V2, 1);

    // Send notification
    Blynk.logEvent("fire_alert", "Fire Detected!");

  } 
  else 
  {

    Serial.println("No Fire");

    digitalWrite(LED, LOW);
    digitalWrite(BUZZER, LOW);

    Blynk.virtualWrite(V2, 0);
  }

  delay(500);
}
```
