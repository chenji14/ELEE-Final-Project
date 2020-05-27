#include <SPI.h>
#include <WiFiNINA.h>
#include "arduino_secrets.h"
#define greenLed 9
#define yellowled 3

// Configure the pins used for the ESP32 connection
#define SPIWIFI       SPI  // The SPI port
#define SPIWIFI_SS    10   // Chip select pin
#define ESP32_RESETN  5   // Reset pin
#define SPIWIFI_ACK   7   // a.k.a BUSY or READY pin
#define ESP32_GPIO0   6

///////please enter your sensitive data in the Secret tab/arduino_secrets.h
char ssid[] = SECRET_SSID;        // your network SSID (name)
char pass[] = SECRET_PASS;        // your network password (use for WPA, or use as key for WEP)
int status = WL_IDLE_STATUS;     // the Wifi radio's status
int keyIndex = 0;
WiFiServer server(80);
String readString;

void setup()
{
  pinMode(greenLed, OUTPUT);
  pinMode(yellowled, OUTPUT);
  Serial.begin(9600);

    analogWrite(yellowled,0);
  analogWrite(greenLed,0);
  
  while (status != WL_CONNECTED)
  {
    Serial.print("Attempting to connect to Network named: ");
    Serial.println(ssid);
    status = WiFi.begin(ssid, pass);
    delay(3000);
  }
  server.begin();
  Serial.print("You're connected to the network with ");
  Serial.print("SSID: ");
  Serial.println(WiFi.SSID());
  IPAddress ip = WiFi.localIP();
  Serial.print("IP Address: ");
  Serial.println(ip);
}

void loop()
{
  
  WiFiClient client = server.available();
  if (client)
  {
    Serial.println("new client");
    boolean currentLineIsBlank = true;
    while (client.connected())
    {
      if (client.available())
      {
        char c = client.read();
        if (readString.length() < 500)
        {
          readString += c;
          Serial.write(c);
          if (c == '\n' && currentLineIsBlank)
          {
            client.println("HTTP/1.1 200 OK"); //send new page
            client.println("Content-Type: text/html");
            client.println("Connection: close");  // the connection will be closed after completion of the response
            client.println("Refresh: 60");
            client.println();
            
            client.println("<hr />");
            client.println("<HTML>");
            client.println("<HEAD>");
            client.println("<TITLE>LED CONTROL</TITLE>");
            client.println("</HEAD>");
            client.println("<BODY>");
            client.println("</HEAD>");
            client.println("<BODY>");
            client.println("<center><p><H1><H1 style=color:red>Arduino Control Using WiFi</H1></p><center>");
            client.println("<H2 style=color:blue> University of Detroit Mercy</H2>");
            client.println("<hr />");
            client.println("<br />");
            client.println("<br />");
            client.println("<br />");
            client.println("<br />");

            client.println("<H3 style=color:green>Choose the LED you want to be lighted</H3>");
            client.println("<br>");
            client.println("<br>");
            client.println("<form action=\"http://192.168.0.2/\">");
            client.println("<input type=\"radio\" id=\"greenLed\" name=\"greenLED\" value=\"G\">");
            client.println("<label for=\"LED\">INTENSITY OF GREENLED</label>");
            client.println("<br>");
            client.println("<br>");
            client.println("<input type=\"radio\" id=\"yellowLed\" name=\"yellowLED\" value=\"Y\" >");
            client.println("<label for=\"PWMA\">INTENSITY OF YELLOWLED</label>");
            
            client.println("</label>");
            client.println("</div>");
            client.println("<br><br>");
            client.println("<input type = \"number\" id=\"VAL-LED\" name = \"VAL-LED\" min=\"0\" max=\"255\">");
            client.println("<br><br>");
            client.println("</label>");
            
            client.println("<input type = \"submit\" value = \"Submit\">");
            client.println("</form>");
            client.println("<p style=color:tomato>Created by Group 1.</p>");
            client.println("<br />");
            client.println("</BODY>");
            client.println("</HTML>");
            client.println("<meta http-equiv=\"refresh\" content=\"4\">");

            delay(1);

            int valGreenLed = (readString.substring(readString.indexOf("greenLED")+19, readString.indexOf("HTTP")-1)).toInt();
            int valYellowLed = (readString.substring(readString.indexOf("yellowLED")+20, readString.indexOf("HTTP")-1)).toInt();
            Serial.print("valGreenLed: "); Serial.print(valGreenLed);
            Serial.print(" | valYellowLed: "); Serial.println(valYellowLed);

            client.println("<br>");

              analogWrite(greenLed,valGreenLed);
              analogWrite(yellowled,valYellowLed);
//              client.println("Current status of LED is ");
//              client.println(valYellowLed);

            delay(3);
            client.println("<br>");
            delay(3);
            readString = "";
            delay(5);
            client.stop();
            Serial.println("client disonnected");
          }
        }
      }
    }
  }
}
