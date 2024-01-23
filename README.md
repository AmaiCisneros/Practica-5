# Practica-5
## Introducción:
-Dentro del simular WOKWI utilizaremos la tarjeta Esp32,ocuparemos un sensor Ultrasonico para medir la distancia, el sensor DHT11 (para medir temperatura y humedad) esto se vera reflejado el un LCD

°Para realizar esta practica necesitas lo siguiente

Tarjeta ESP32
Sensor DHT11
HC-SR04 Ultrasonic Distance Sensor
LCD 16x2 (I2C)

### Instrucciones
1°- Introducir el siguiente codigo dentro de la programacion de WOKWI;
#include "DHTesp.h"
#include <LiquidCrystal_I2C.h>
#define I2C_ADDR    0x27
#define LCD_COLUMNS 20
#define LCD_LINES   4

DHTesp dhtSensor;

const int Trigger = 4;   //Pin digital 2 para el Trigger del sensor
const int Echo = 2;   //Pin digital 3 para el Echo del sensor
const int DHT_PIN = 15;



LiquidCrystal_I2C lcd(I2C_ADDR, LCD_COLUMNS, LCD_LINES);

void setup() {
  Serial.begin(9600);//iniciailzamos la comunicación
  pinMode(Trigger, OUTPUT); //pin como salida
  pinMode(Echo, INPUT);  //pin como entrada
  digitalWrite(Trigger, LOW);//Inicializamos el pin con 0
   Serial.begin(115200);
  dhtSensor.setup(DHT_PIN, DHTesp::DHT22);
  lcd.init();
  lcd.backlight();
}

void loop()
{

  TempAndHumidity  data = dhtSensor.getTempAndHumidity();
  Serial.println("Temp: " + String(data.temperature, 1) + "°C");
  Serial.println("Humidity: " + String(data.humidity, 1) + "%");
  Serial.println("---"); 

  lcd.setCursor(0, 0);
  lcd.print("  Temp: " + String(data.temperature, 1) + "\xDF"+"C  ");
  lcd.setCursor(0, 1);
  lcd.print(" Humidity: " + String(data.humidity, 1) + "% ");
  lcd.print("Wokwi Online IoT");

  delay(2000);
  lcd.clear();

  long t; //timepo que demora en llegar el eco
  long d; //distancia en centimetros

  digitalWrite(Trigger, HIGH);
  delayMicroseconds(10);          //Enviamos un pulso de 10us
  digitalWrite(Trigger, LOW);
  
  t = pulseIn(Echo, HIGH); //obtenemos el ancho del pulso
  d = t/59;             //escalamos el tiempo a una distancia en cm

  lcd.setCursor(0, 0);
  lcd.print("Distancia: " +String(d) + "cm  ");
  lcd.print("Wokwi Online IoT");
  delay(2000);
  lcd.clear();

  
  

  lcd.clear();
  lcd.setCursor(0, 0);
  lcd.print("Ing Cisneros");
  lcd.setCursor(0, 1);
  lcd.print("INDUSTRIAL");
  delay(2000);
  lcd.clear();

}

2°-Instalar la libreria DHT sensor library for ESPx y LiquidCrystal I2C ;
(imagen librerias)

3°-Hacer la conexion correspondientes de los sensores Ultrasonico y DHT11 hacia la tarjeta ESP32 y  tambien conectar la LCD ;

(imagen conexiones sensores, tarjeta y lCD)

4° Inicia el simulador y observa los valores reflejados dentro de la pantalla LCD 
(imagen de resultados de temperatura,humedad,distancia y nombre y carrera



