#define greenLed  6
#define yellowLed  5
#define motionSensor  2
#define IRSeneor  A0

void setup()
{
  pinMode(greenLed, OUTPUT);
  pinMode(yellowLed, OUTPUT);
  pinMode(motionSensor, INPUT);
  Serial.begin(9600);
}

void loop()
{
  int IRdistance = 0;
  digitalWrite(greenLed, LOW);
  digitalWrite(yellowLed, LOW);
    
  if (digitalRead(motionSensor) == HIGH)
  {
    digitalWrite(greenLed, HIGH);
    digitalWrite(yellowLed, HIGH);
    IRdistance = analogRead(IRSeneor);
    while (IRdistance < 150)
    {
      digitalWrite(greenLed, LOW);
      digitalWrite(yellowLed, LOW);
      IRdistance = analogRead(IRSeneor);
      delay(100);
    }
  }
}
