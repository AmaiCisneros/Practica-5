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

const int DHT_PIN = 15;

DHTesp dhtSensor;

LiquidCrystal_I2C lcd(I2C_ADDR, LCD_COLUMNS, LCD_LINES);
const int Trigger =4; //Pin digital 4 para Triager del sensor
const int Echo=2;   //Pin digital 2 para el Echo del sensor

void setup()
 {
   Serial.begin(115200);
  dhtSensor.setup(DHT_PIN, DHTesp::DHT22);
  lcd.init();
  lcd.backlight();
  pinMode(Trigger, OUTPUT); //pin como salida
  pinMode(Echo, INPUT);  //pin como entrada
  digitalWrite(Trigger, LOW);//Inicializamos el pin con 0
}
void loop()
{

  TempAndHumidity  data = dhtSensor.getTempAndHumidity();
  Serial.println("Temp: " + String(data.temperature, 1) + "°C");
  Serial.println("Humidity: " + String(data.humidity, 1) + "%");
  Serial.println("---"); 
  long t; //timepo que demora en llegar el eco
  long d; //distancia en centimetros

  digitalWrite(Trigger, HIGH);
  delayMicroseconds(2000);          //Enviamos un pulso de 10us
  digitalWrite(Trigger, LOW);
  
  t = pulseIn(Echo, HIGH); //obtenemos el ancho del pulso
  d = t/59;

  lcd.setCursor(0, 0);
  lcd.print("  Temp: " + String(data.temperature, 1) + "\xDF"+"C  ");
  lcd.setCursor(0, 1);
  lcd.print(" Humidity: " + String(data.humidity, 1) + "% ");
  lcd.print("Wokwi Online IoT");
  lcd.clear();
  delay(2000);


  lcd.setCursor(0,0);
  lcd.print("Dis: " + String(d) + "cm  ");
  lcd.clear();
  delay(2000);

  lcd.clear();
  lcd.setCursor(0, 0);
  lcd.print("Ing.Cisneros");
  lcd.setCursor(0, 1);
  lcd.print("Industrial");
  delay(2000);
  lcd.clear();

}

2°-Instalar la libreria DHT sensor library for ESPx y LiquidCrystal I2C ;
![](https://github.com/AmaiCisneros/Practica-5/blob/main/1.png)

3°-Hacer la conexion correspondientes de los sensores Ultrasonico y DHT11 hacia la tarjeta ESP32 y  tambien conectar la LCD ;
![](https://github.com/AmaiCisneros/Practica-5/blob/main/Captura%20de%20pantalla%202024-01-24%20104851.png)


4° Inicia el simulador y observa los valores reflejados dentro de la pantalla LCD 
![](https://github.com/AmaiCisneros/Practica-5/blob/main/Captura%20de%20pantalla%202024-01-24%20104526.png)


