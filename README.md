<p>APK Android e código disponível</p>
<br>
<img width="798" height="396" alt="Image" src="https://github.com/user-attachments/assets/12a3f51a-1d1a-467b-b0ea-2c0d38f55b79" />
<br>

<details>
<summary>📄 Ver código Arduino</summary>

```cpp

/*
PARA ARDUINO COM APP UNITY FUNCIONA PERFEITAMENTE
Dev por "Michael" Trillobit3sGames
trillobit3s@gmail.com - estuartyy@gmail.com 

primeira atualização 05/02/2017
Ultima atualização 25/04/2026

USE ESTES CARACTERES EM SEU CÓDIGO

F = PARA FRENTE => S = PARADO  
B = PARA ATRÁS => S = PARADO
L = PARA ESQUERDA => S = PARADO
R = PARA DIREITA => S = PARADO

S = PARADO

V = BUZINA => v = buzina desligada
W = LED FRENTE => w = led frente desligado
U = LED TRÁS => u = led trás   desligado 
*/

// 2026.2
#include <SoftwareSerial.h>
#include <NewTone.h>

SoftwareSerial BT(2, 4);

// LEDs
#define LED_Verd_Esq A1
#define LED_Verd_Dir 7
#define LED_Verm_Esq 8
#define LED_Verm_Dir 12

#define BUZZER 13

// Motores
const int motorA1 = 3;
const int motorA2 = 5;
const int motorB1 = 6;
const int motorB2 = 9;

// Velocidade
const int ENA = 10;
const int ENB = 11;

// CONTROLE
int vSpeed = 255;
char currentState = 'S';
unsigned long lastReceive = 0;

void setup()
{
  BT.begin(9600);

  pinMode(LED_Verd_Esq, OUTPUT);
  pinMode(LED_Verd_Dir, OUTPUT);
  pinMode(LED_Verm_Esq, OUTPUT);
  pinMode(LED_Verm_Dir, OUTPUT);

  pinMode(motorA1, OUTPUT);
  pinMode(motorA2, OUTPUT);
  pinMode(motorB1, OUTPUT);
  pinMode(motorB2, OUTPUT);

  pinMode(ENA, OUTPUT);
  pinMode(ENB, OUTPUT);

  pinMode(BUZZER, OUTPUT);

  parar();
}

void loop()
{
  while (BT.available())
  {
    char c = BT.read();
    lastReceive = millis();

    switch (c)
    {
      // VELOCIDADE
      case '0': vSpeed = 100; break;
      case '4': vSpeed = 140; break;
      case '6': vSpeed = 180; break;
      case '7': vSpeed = 200; break;
      case '8': vSpeed = 240; break;
      case '9': vSpeed = 255; break;

      // MOVIMENTO
      case 'F': case 'B': case 'L': case 'R':
      case 'S': case 'G': case 'I': case 'H': case 'J':
        currentState = c;
        break;

      // LED FRENTE
      case 'W': digitalWrite(LED_Verd_Esq, HIGH); digitalWrite(LED_Verd_Dir, HIGH); break;
      case 'w': digitalWrite(LED_Verd_Esq, LOW); digitalWrite(LED_Verd_Dir, LOW); break;

      // LED TRAS
      case 'U': digitalWrite(LED_Verm_Esq, HIGH); digitalWrite(LED_Verm_Dir, HIGH); break;
      case 'u': digitalWrite(LED_Verm_Esq, LOW); digitalWrite(LED_Verm_Dir, LOW); break;

      // BUZINA
      case 'V': NewTone(BUZZER, 1000); break;
      case 'v': noNewTone(BUZZER); break;
    }
  }

  // FAILSAFE REAL (desconectou)
  if (millis() - lastReceive > 3000)
  {
    currentState = 'S';
  }

  executarMovimento();
}

void executarMovimento()
{
  analogWrite(ENA, vSpeed);
  analogWrite(ENB, vSpeed);

  switch (currentState)
  {
    case 'F': frente(); break;
    case 'B': tras(); break;
    case 'L': esquerda(); break;
    case 'R': direita(); break;

    case 'G': frenteEsq(); break;
    case 'I': frenteDir(); break;
    case 'H': trasEsq(); break;
    case 'J': trasDir(); break;

    default: parar(); break;
  }
}

// MOVIMENTOS
void frente() {
  digitalWrite(motorA1, LOW); digitalWrite(motorA2, HIGH);
  digitalWrite(motorB1, HIGH); digitalWrite(motorB2, LOW);
}

void tras() {
  digitalWrite(motorA1, HIGH); digitalWrite(motorA2, LOW);
  digitalWrite(motorB1, LOW); digitalWrite(motorB2, HIGH);
}

void direita() {
  digitalWrite(motorA1, HIGH); digitalWrite(motorA2, LOW);
  digitalWrite(motorB1, HIGH); digitalWrite(motorB2, LOW);
}

void esquerda() {
  digitalWrite(motorA1, LOW); digitalWrite(motorA2, HIGH);
  digitalWrite(motorB1, LOW); digitalWrite(motorB2, HIGH);
}

// DIAGONAIS
void frenteEsq() {
  analogWrite(ENA, vSpeed / 2);
  analogWrite(ENB, vSpeed);
  frente();
}

void frenteDir() {
  analogWrite(ENA, vSpeed);
  analogWrite(ENB, vSpeed / 2);
  frente();
}

void trasEsq() {
  analogWrite(ENA, vSpeed / 2);
  analogWrite(ENB, vSpeed);
  tras();
}

void trasDir() {
  analogWrite(ENA, vSpeed);
  analogWrite(ENB, vSpeed / 2);
  tras();
}

void parar() {
  digitalWrite(motorA1, LOW);
  digitalWrite(motorA2, LOW);
  digitalWrite(motorB1, LOW);
  digitalWrite(motorB2, LOW);
}
