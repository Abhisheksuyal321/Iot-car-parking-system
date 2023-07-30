#include<LiquidCrystal.h>
#include<ThingSpeak.h>
#include<WiFi.h>
LiquidCrystal lcd(23,19,26,25,33,32);
const char* ssid="vaishii";
const char* pass="meecha04";
unsigned long channelnumber=816129;
const char * ReadAPIKey="E52R2Q90UCNCTUKH";
int field=1;
WiFiClient client;
void setup()
{
  lcd.begin(16,2);
  Serial.begin(115200);
  WiFi.begin(ssid,pass);
  while(WiFi.status() != WL_CONNECTED)
  {
    delay(500);
Serial.println("connecting wifi");
lcd.print("connecting wifi");
delay(1000);
lcd.clear();
}
Serial.println("connected to wifi network");
lcd.print("wifi connnected");
ThingSpeak.begin(client);
}
void loop()
{
  int y= ThingSpeak.readIntField(channelnumber,field,ReadAPIKey);
  int x= ThingSpeak.getLastReadStatus();
  if (x==200)
  {
    Serial.println("channel connected");
    Serial.println("Value"+String(y));
    lcd.setCursor(0,0);
    lcd.clear();
    lcd.print("Data:"+String(y));
  }
  else
  {
    Serial.println("problem updating channel.HTTP error code"+String(x));
   
    lcd.clear();
    lcd.print("error");
  }
}
