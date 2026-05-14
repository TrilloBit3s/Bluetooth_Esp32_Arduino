<p>APK Android e código disponível</p>
<br>
<img width="798" height="396" alt="Image" src="https://github.com/user-attachments/assets/12a3f51a-1d1a-467b-b0ea-2c0d38f55b79" />
<br>

<details>
<summary>📄 Ver código Arduino</summary>

```cpp

// PARA ARDUINO COM APP UNITY FUNCIONA PERFEITAMENTE
// Dev por "Michael" Trillobit3sGames
// trillobit3s@gmail.com - estuartyy@gmail.com 

// Carrinho bluetooth Arduino sem slider
// primeira atualização 05/02/2017
// Ultima atualização 14/05/2026

// USE ESTES CARACTERES EM SEU CÓDIGO

// F = PARA FRENTE => S = PARADO  
// B = PARA ATRÁS => S = PARADO
// L = PARA ESQUERDA => S = PARADO
// R = PARA DIREITA => S = PARADO

// S = PARADO

// V = BUZINA => v = buzina desligada
// W = LED FRENTE => w = led frente desligado
// U = LED TRÁS => u = led trás   desligado 

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
  switch (currentState)
  {
    // FRENTE
    case 'F':
      analogWrite(ENA, vSpeed);
      analogWrite(ENB, vSpeed);
      frente();
    break;

    // TRÁS
    case 'B':
      analogWrite(ENA, vSpeed);
      analogWrite(ENB, vSpeed);
      tras();
    break;

    // ESQUERDA
    case 'L':
      analogWrite(ENA, vSpeed);
      analogWrite(ENB, vSpeed);
      esquerda();
    break;

    // DIREITA
    case 'R':
      analogWrite(ENA, vSpeed);
      analogWrite(ENB, vSpeed);
      direita();
    break;

    // DIAGONAL FRENTE ESQUERDA
    case 'G':
      frenteEsq();
    break;

    // DIAGONAL FRENTE DIREITA
    case 'I':
      frenteDir();
    break;

    // DIAGONAL TRÁS ESQUERDA
    case 'H':
      trasEsq();
    break;

    // DIAGONAL TRÁS DIREITA
    case 'J':
      trasDir();
    break;

    // PARAR
    default:
      parar();
    break;
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

  analogWrite(ENA, 0);
  analogWrite(ENB, 0);

  digitalWrite(motorA1, LOW);
  digitalWrite(motorA2, LOW);
  digitalWrite(motorB1, LOW);
  digitalWrite(motorB2, LOW);
}

-------------------------------------------------------------------------
/*
- CÓDIGO PARA USAR NO ESP32_38p COM APP UNITY
Algoritimo para Esp32 DOIT DEVKI V1
Dev por "Michael M.SABINO" Trillobit3sGames
trillobit3s@gmail.com - estuartyy@gmail.com 
primeira versão bluetooth arduino 05/02/2017
Ultima atualização bluetooth Esp32 14/05/2026

USE ESTES CARACTERES EM SEU CÓDIGO

F = PARA FRENTE => S = PARA  
B = PARA TRÁS => S = PARA
L = PARA ESQUERDA => S = PARA
R = PARA DIREITA => S = PARA
G = PARA Frente Esquerda => S = PARA
I = PARA Frente Direira => S = PARA
H = PARA Tras Esquerda => S = PARA
J = PARA Tras Direita => S = PARA

S = PARADO

V = BUZINA => v = buzina desligada
W = LED FRENTE => w = led frente desligado
U = LED TRÁS => u = led trás   desligado 
*/

//versao atualizada, caso nao funcione use o ultimo código
#include "BluetoothSerial.h"

BluetoothSerial SerialBT;

// ================= PINOS =================

// LEDs
#define LED_Verd_Esq 23
#define LED_Verd_Dir 22
#define LED_Verm_Esq 21
#define LED_Verm_Dir 19

// Buzzer
#define BUZZER 18

// Motores
const int MR1 = 14;
const int MR2 = 27;
const int ML1 = 26;
const int ML2 = 25;

// PWM velocidade
const int ENA = 13;
const int ENB = 12;

// ================= PWM =================
const int freq = 1000;
const int resolution = 8;

const int channelA = 0;
const int channelB = 1;

// ================= CONTROLE =================
int vSpeed = 255;
char currentState = 'S';
unsigned long lastReceive = 0;

bool buzzerLigado = false;

// ================= SETUP =================
void setup()
{
  Serial.begin(115200);
  SerialBT.begin("ESP32_Carro");
  SerialBT.setTimeout(20);

  pinMode(LED_Verd_Esq, OUTPUT);
  pinMode(LED_Verd_Dir, OUTPUT);
  pinMode(LED_Verm_Esq, OUTPUT);
  pinMode(LED_Verm_Dir, OUTPUT);

  pinMode(MR1, OUTPUT);
  pinMode(MR2, OUTPUT);
  pinMode(ML1, OUTPUT);
  pinMode(ML2, OUTPUT);

  pinMode(BUZZER, OUTPUT);
  noTone(BUZZER);

  ledcSetup(channelA, freq, resolution);
  ledcSetup(channelB, freq, resolution);

  ledcAttachPin(ENA, channelA);
  ledcAttachPin(ENB, channelB);
  parar();
}

// ================= LOOP =================
void loop()
{
  while (SerialBT.available())
  {
    char c = SerialBT.read();
    //Serial.println(c);

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

      // MOVIMENTO (ESTADO)
      case 'F': case 'B': case 'L': case 'R':
      case 'S': case 'G': case 'I': case 'H': case 'J':
        currentState = c;
        break;

      // LEDS
      case 'W': digitalWrite(LED_Verd_Esq, HIGH); digitalWrite(LED_Verd_Dir, HIGH); break;
      case 'w': digitalWrite(LED_Verd_Esq, LOW); digitalWrite(LED_Verd_Dir, LOW); break;

      case 'U': digitalWrite(LED_Verm_Esq, HIGH); digitalWrite(LED_Verm_Dir, HIGH); break;
      case 'u': digitalWrite(LED_Verm_Esq, LOW); digitalWrite(LED_Verm_Dir, LOW); break;

      // BUZZER (sem reiniciar toda hora)
      case 'V':
        if (!buzzerLigado)
        {
          tone(BUZZER, 1000);
          buzzerLigado = true;
        }
        break;

      case 'v':
        noTone(BUZZER);
        buzzerLigado = false;
        break;
    }
  }

  // FAILSAFE
  if (millis() - lastReceive > 1000)//testar 500 para resposta mais rápida em perda de conexão.
  {
    currentState = 'S';
  }

  executarMovimento();
}

// ================= EXECUÇÃO CONTÍNUA =================

void executarMovimento()
{
  switch (currentState)
  {
    case 'F':
      ledcWrite(channelA, vSpeed);
      ledcWrite(channelB, vSpeed);
      frente();
      break;

    case 'B':
      ledcWrite(channelA, vSpeed);
      ledcWrite(channelB, vSpeed);
      tras();
      break;

    case 'L':
      ledcWrite(channelA, vSpeed);
      ledcWrite(channelB, vSpeed);
      esquerda();
      break;

    case 'R':
      ledcWrite(channelA, vSpeed);
      ledcWrite(channelB, vSpeed);
      direita();
      break;

    case 'G':
      ledcWrite(channelA, vSpeed / 2);
      ledcWrite(channelB, vSpeed);
      frente();
      break;

    case 'I':
      ledcWrite(channelA, vSpeed);
      ledcWrite(channelB, vSpeed / 2);
      frente();
      break;

    case 'H':
      ledcWrite(channelA, vSpeed / 2);
      ledcWrite(channelB, vSpeed);
      tras();
      break;

    case 'J':
      ledcWrite(channelA, vSpeed);
      ledcWrite(channelB, vSpeed / 2);
      tras();
      break;

    default:
      parar();
      break;
  }
}

// ================= MOVIMENTOS =================

void frente()
{
  digitalWrite(MR1, LOW);
  digitalWrite(MR2, HIGH);

  digitalWrite(ML1, HIGH);
  digitalWrite(ML2, LOW);
}

void tras()
{
  digitalWrite(MR1, HIGH);
  digitalWrite(MR2, LOW);

  digitalWrite(ML1, LOW);
  digitalWrite(ML2, HIGH);
}

void direita()
{
  digitalWrite(MR1, HIGH);
  digitalWrite(MR2, LOW);

  digitalWrite(ML1, HIGH);
  digitalWrite(ML2, LOW);
}

void esquerda()
{
  digitalWrite(MR1, LOW);
  digitalWrite(MR2, HIGH);

  digitalWrite(ML1, LOW);
  digitalWrite(ML2, HIGH);
}

void parar()
{
  ledcWrite(channelA, 0);
  ledcWrite(channelB, 0);

  digitalWrite(MR1, LOW);
  digitalWrite(MR2, LOW);
  digitalWrite(ML1, LOW);
  digitalWrite(ML2, LOW);
}