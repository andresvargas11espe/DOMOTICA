# DOMOTICA
# Casa domótica con alexa
# este programa esta hecho para controlar luces de una casa con alexa mediante tarjetas <ESP32 

#include <Arduino.h>
#ifdef ESP32
#include <WiFi.h>
#else
#include <ESP8266WiFi.h>
#endif
#include "fauxmoESP.h"


//CREDENCIALES CONEXION A TU WIFI
#define WIFI_SSID "NETLIFE-latmschiluisab2"

#define WIFI_PASS "0501742951"

//VELOCIDAD SERIAL
#define SERIAL_BAUDRATE 9600

// DISPOSITIVOS
#define PIN_DISPOSITIVO_1 15
#define ID_1 "Foco 1"

#define PIN_DISPOSITIVO_2 2
#define ID_2 "Foco 2"

#define PIN_DISPOSITIVO_3 0
#define ID_3 "Foco 3"
#define PIN_DISPOSITIVO_4 16
#define ID_4 "Foco 4"
#define PIN_DISPOSITIVO_5 17
#define ID_5 "Foco 5"
#define PIN_DISPOSITIVO_6 5
#define ID_6 "Foco 6"
#define PIN_DISPOSITIVO_7 18
#define ID_7 "Foco 7"
#define PIN_DISPOSITIVO_8 19
#define ID_8 "Foco 8"
#define PIN_DISPOSITIVO_9 21
#define ID_9 "Foco 9"
#define PIN_DISPOSITIVO_10 22
#define ID_10 "Foco 10"


#define PIN_DISPOSITIVO_11 23
#define ID_11 "Foco 11"


/////////////////////////
#define TODO "Todo"
#define LUCES "Luces"
/////////////////////////
 
fauxmoESP fauxmo;
// -----------------------------------------------------------------------------
// Wifi
// -----------------------------------------------------------------------------

void wifiSetup() {

  //Inicializamos el WIFI en modo estacion
  WiFi.mode(WIFI_STA);

  //Conecta al WIFI
  Serial.println("---------------------------------------------");
  Serial.printf("[WIFI] Conectando a red %s ", WIFI_SSID);
  WiFi.begin(WIFI_SSID, WIFI_PASS);

  //Esperamos si es que no conecta
  while (WiFi.status() != WL_CONNECTED) {
    Serial.print("NO CONECTA");
    delay(100);
  }
  Serial.println();
  Serial.printf("[WIFI] Conectado al WIFI %s ", WIFI_SSID);
  Serial.println();
  //Conexion Exitosa
  Serial.printf("[WIFI] MODO ESTACION, SSID: %s, Direccion IP: %s\n", WiFi.SSID().c_str(), WiFi.localIP().toString().c_str());
  Serial.println("---------------------------------------------");
  Serial.println("A la espera de instrucciones de alexa...");
  Serial.println("---------------------------------------------");
}

void setup() {

  //INICIALIZAMOS EL SERIAL
  Serial.begin(SERIAL_BAUDRATE);

  //ESTABLECEMOS EL ESTADO DE LOS PINES
  pinMode(PIN_DISPOSITIVO_1, OUTPUT);
  pinMode(PIN_DISPOSITIVO_2, OUTPUT);
  pinMode(PIN_DISPOSITIVO_3, OUTPUT);
  pinMode(PIN_DISPOSITIVO_4, OUTPUT);
  pinMode(PIN_DISPOSITIVO_5, OUTPUT);
  pinMode(PIN_DISPOSITIVO_6, OUTPUT);
  pinMode(PIN_DISPOSITIVO_7, OUTPUT);
  pinMode(PIN_DISPOSITIVO_8, OUTPUT);
  pinMode(PIN_DISPOSITIVO_9, OUTPUT);
  pinMode(PIN_DISPOSITIVO_10, OUTPUT);
  pinMode(PIN_DISPOSITIVO_11, OUTPUT);
  
  //ESTABLECEMOS QUE ETEN APAGADOS LOS DISPOSITIVOS
  digitalWrite(PIN_DISPOSITIVO_1, LOW);
  digitalWrite(PIN_DISPOSITIVO_2, LOW);
  digitalWrite(PIN_DISPOSITIVO_3, LOW);
  digitalWrite(PIN_DISPOSITIVO_4, LOW);
  digitalWrite(PIN_DISPOSITIVO_5, LOW);
  digitalWrite(PIN_DISPOSITIVO_6, LOW);
  digitalWrite(PIN_DISPOSITIVO_7, LOW);
  digitalWrite(PIN_DISPOSITIVO_8, LOW);
  digitalWrite(PIN_DISPOSITIVO_9, LOW);
  digitalWrite(PIN_DISPOSITIVO_10, LOW);
  digitalWrite(PIN_DISPOSITIVO_11, LOW);

  //INICIA EL METODO WIFI
  wifiSetup();

  fauxmo.createServer(true); // not needed, this is the default value
  fauxmo.setPort(80); // This is required for gen3 devices
  fauxmo.enable(true);

  //AÑADA O CREA LOS DISPISITIVOS ANTERIORMENTE ESTABLECIDOS EN LAS VARIABLES
  fauxmo.addDevice(ID_1);
  fauxmo.addDevice(ID_2);
  fauxmo.addDevice(ID_3);
  fauxmo.addDevice(ID_4);
  fauxmo.addDevice(ID_5);
  fauxmo.addDevice(ID_6);
  fauxmo.addDevice(ID_7);
  fauxmo.addDevice(ID_8);
  fauxmo.addDevice(ID_9);
  fauxmo.addDevice(ID_10);
  fauxmo.addDevice(ID_11);

  fauxmo.addDevice(TODO);
  fauxmo.addDevice(LUCES);
  fauxmo.onSetState([](unsigned char device_id, const char * device_name, bool state, unsigned char value) {

    Serial.printf("[INFORMACION] Dispositivo #%d (%s) Estado: %s Valor: %d\n", device_id, device_name, state ? "ON" : "OFF", value);

    if (strcmp(device_name, ID_1) == 0) {
      digitalWrite(PIN_DISPOSITIVO_1, state ? HIGH : LOW);
    }
    else if (strcmp(device_name, ID_2) == 0) {
      digitalWrite(PIN_DISPOSITIVO_2, state ? HIGH : LOW);
    }
    else if (strcmp(device_name, ID_3) == 0) {
      digitalWrite(PIN_DISPOSITIVO_3, state ? HIGH : LOW);
    }
    else if (strcmp(device_name, ID_4) == 0) {
      digitalWrite(PIN_DISPOSITIVO_4, state ? HIGH : LOW);
    }
    else if (strcmp(device_name, ID_5) == 0) {
      digitalWrite(PIN_DISPOSITIVO_5, state ? HIGH : LOW);
    }
    else if (strcmp(device_name, ID_6) == 0) {
      digitalWrite(PIN_DISPOSITIVO_6, state ? HIGH : LOW);
    }
    else if (strcmp(device_name, ID_7) == 0) {
      digitalWrite(PIN_DISPOSITIVO_7, state ? HIGH : LOW);
    }
    else if (strcmp(device_name, ID_8) == 0) {
      digitalWrite(PIN_DISPOSITIVO_8, state ? HIGH : LOW);
    }
    else if (strcmp(device_name, ID_9) == 0) {
      digitalWrite(PIN_DISPOSITIVO_9, state ? HIGH : LOW);
    }
    else if (strcmp(device_name, ID_10) == 0) {
      digitalWrite(PIN_DISPOSITIVO_10, state ? HIGH : LOW);
    }
    else if (strcmp(device_name, ID_11) == 0) {
      digitalWrite(PIN_DISPOSITIVO_11, state ? HIGH : LOW);
    }
    else if (strcmp(device_name, TODO) == 0) {
      digitalWrite(PIN_DISPOSITIVO_1, state ? HIGH : LOW);
      digitalWrite(PIN_DISPOSITIVO_2, state ? HIGH : LOW);
      digitalWrite(PIN_DISPOSITIVO_3, state ? HIGH : LOW);
      digitalWrite(PIN_DISPOSITIVO_4, state ? HIGH : LOW);
      digitalWrite(PIN_DISPOSITIVO_5, state ? HIGH : LOW);
      digitalWrite(PIN_DISPOSITIVO_6, state ? HIGH : LOW);
      digitalWrite(PIN_DISPOSITIVO_7, state ? HIGH : LOW);
      digitalWrite(PIN_DISPOSITIVO_8, state ? HIGH : LOW);
      digitalWrite(PIN_DISPOSITIVO_9, state ? HIGH : LOW);
      digitalWrite(PIN_DISPOSITIVO_10, state ? HIGH : LOW);
      digitalWrite(PIN_DISPOSITIVO_11, state ? HIGH : LOW);
    }
    else if (strcmp(device_name, LUCES) == 0) {
      digitalWrite(PIN_DISPOSITIVO_1, state ? HIGH : LOW);
      digitalWrite(PIN_DISPOSITIVO_2, state ? HIGH : LOW);
      digitalWrite(PIN_DISPOSITIVO_3, state ? HIGH : LOW);
      digitalWrite(PIN_DISPOSITIVO_4, state ? HIGH : LOW);
      digitalWrite(PIN_DISPOSITIVO_5, state ? HIGH : LOW);
      digitalWrite(PIN_DISPOSITIVO_6, state ? HIGH : LOW);
      digitalWrite(PIN_DISPOSITIVO_7, state ? HIGH : LOW);
      digitalWrite(PIN_DISPOSITIVO_8, state ? HIGH : LOW);
      digitalWrite(PIN_DISPOSITIVO_9, state ? HIGH : LOW);
      digitalWrite(PIN_DISPOSITIVO_10, state ? HIGH : LOW);
      digitalWrite(PIN_DISPOSITIVO_11, state ? HIGH : LOW);
    }


  });

}

void loop() {
  fauxmo.handle();
}
