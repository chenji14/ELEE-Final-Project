#include <Wire.h>
#include <SPI.h>
#include <Adafruit_LSM9DS1.h>
#include <Adafruit_Sensor.h>
#define LSM9DS1_XGCS 8
#define LSM9DS1_MCS 7
#define greenLed  6
#define yellowLed  5

//  SPI communication
Adafruit_LSM9DS1 lsm = Adafruit_LSM9DS1(LSM9DS1_XGCS, LSM9DS1_MCS);

void setupSensor()
{
  lsm.setupAccel(lsm.LSM9DS1_ACCELRANGE_2G);
  lsm.setupMag(lsm.LSM9DS1_MAGGAIN_4GAUSS);
  lsm.setupGyro(lsm.LSM9DS1_GYROSCALE_245DPS);
}

void setup()
{
  pinMode(greenLed, OUTPUT);
  pinMode(yellowLed, OUTPUT);
  Serial.begin(9600);
  lsm.begin();
  setupSensor();
}

void loop()
{
  int pwmValue,averAcceleration_y=0,averAcceleration_x=0;
  lsm.read();  
  sensors_event_t a, m, g, temp;
  lsm.getEvent(&a, &m, &g, &temp); 

  for (int i=80;i>0;i--)
  {
    averAcceleration_x = a.acceleration.x + averAcceleration_x;
    averAcceleration_y = a.acceleration.y + averAcceleration_y;
  }
  averAcceleration_x = averAcceleration_x / 80;
  averAcceleration_y = averAcceleration_y / 80;

  Serial.print("averAcceleration_x: ");
  Serial.print(averAcceleration_x);
  Serial.print(" | ");
  Serial.print("averAcceleration_y: ");
  Serial.println(averAcceleration_y);
  
  pwmValue = map(averAcceleration_y,-9,10,0,255);

  Serial.print("pwmValue: ");
  Serial.println(pwmValue);

  if (averAcceleration_x>=0)
  {
    analogWrite(greenLed, pwmValue);
    digitalWrite(yellowLed, LOW);
  }
  else 
  {
    analogWrite(yellowLed, pwmValue);
    digitalWrite(greenLed, LOW);
  }
} 
