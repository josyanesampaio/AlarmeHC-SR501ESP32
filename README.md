# AlarmeHC-SR501ESP32
Alarme com HC-SR501 e ESP32 Monitorado por WiFi

#include <WiFi.h>
#define BLYNK_PRINT Serial       
#include <BlynkSimpleEsp32.h>
char auth[] = "";  /*Autenticação do login pelo e-mail */

/* Login e senha para acesso ao aplicativo*/
char ssid[] = "";
char pass[] = "";
 
/* HC-SR501 – sensor de detecção de movimento*/
#define pirPin 32               // Input for HC-S501
int pirValue = 0;
 
bool movementDetected = false;
 
void setup()
{
   Serial.begin(115200);
   delay(10);
   Serial.println("Programa iniciado");
   WiFi.begin(ssid, pass);
   int wifi_ctr = 0;
   while (WiFi.status() != WL_CONNECTED)
 {
    delay(500);
    Serial.print(".");
 }
  
   Serial.println("WiFi conectado");  
   Blynk.begin(auth, ssid, pass);
   pinMode(pirPin, INPUT);
}
 
void loop()
{
   Blynk.run();
   Serial.println("Movimento detectado");
   Blynk.notify("CUIDADO! Invasor presente!");
 
   delay(1000);
   Blynk.run();
  
   while(digitalRead(pirPin) == HIGH)
   Serial.println("Aguarde");
 
   Serial.println("Desligando");
   esp_sleep_enable_ext0_wakeup(GPIO_NUM_32,1);
   esp_deep_sleep_start();
   }
