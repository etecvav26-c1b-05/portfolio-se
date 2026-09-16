#include <Servo.h>

int ldr = A2;
int tmp = A1;
int servoPin = 8;
int ledVerde = 10;
int ledVermelho = 9;
float tempLimite = 28.0;
int luzLimite = 700;

Servo telhado;

void setup() {
  pinMode(ledVerde, OUTPUT);
  pinMode(ledVermelho, OUTPUT);

  telhado.attach(servoPin);
  telhado.write(0);
  digitalWrite(ledVermelho, HIGH);
}

void loop() {
  int valorLDR = analogRead(ldr);
  int valorTMP = analogRead(tmp);

  float tensao = valorTMP * (5.0 / 1024.0);
  float temperatura = (tensao - 0.5) * 100.0;

  bool fechar = (valorLDR < luzLimite) || (temperatura > tempLimite);

  if (fechar) {
    telhado.write(0);
    digitalWrite(ledVerde, LOW);
    digitalWrite(ledVermelho, HIGH);
  } else {
    telhado.write(90);
    digitalWrite(ledVerde, HIGH);
    digitalWrite(ledVermelho, LOW);
  }
  
  delay(500);
}