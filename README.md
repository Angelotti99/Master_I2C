# Master_I2C
#include <LiquidCrystal_I2C.h>

LiquidCrystal_I2C lcd(0x27,16,2);

const int Echo = 2; //Declaro variable de Echo//
const int Trigger = 3;  //Declaro variable de Trigger//

void setup ()

{
  Serial.begin (9600); //Velocidad de transmisión en baudios al puerto serie//
  pinMode(Trigger,OUTPUT); //Defino tipo de función//
  pinMode(Echo,INPUT); //Definio tipo de función//
}

void loop ()

{
  int cm = ping(Trigger,Echo);//Declaro variable que va a tomar valor de CM en función de la relación Trigger / Echo//
  Serial.print("Distancia: ");
  Serial.println(cm);
  delay(1000);

  if(cm < 20) //Condicón si CM es menor a 20, advierte encendiendo display//
  {
    Serial.print("Peligro de Choque\t");
    lcd.init();
    lcd.backlight();
    lcd.setCursor(0,0);
    lcd.print(" PELIGRO CHOQUE"); 
    lcd.setCursor(0,1);
    lcd.print("Distancia:");
    lcd.print(cm);
    lcd.display();
    delay(1000);
  }
  else
  {
    lcd.init();
    lcd.backlight();
    lcd.setCursor(0,0);
    lcd.print("    DESPACIO"); 
    lcd.setCursor(0,1);
    lcd.print("Distancia:");
    lcd.print(cm);
    lcd.display();
    delay(500);
  }
}

int ping(int Trigger, int Echo)
{
  long duration, distancecm;
  digitalWrite(Trigger,LOW);
  delayMicroseconds(4);
  digitalWrite(Trigger,HIGH);
  delayMicroseconds(10);
  digitalWrite(Trigger,LOW);

  duration = pulseIn(Echo, HIGH); //Duración del pulso ECHO//
  distancecm = duration *10 /340 /2; //Calculo de distancia en función de velocidad e ida y vuelta del pulso//
  return distancecm; //Devuelvo valor distancecm//
} 
