# 🚀 AquaGuard Monitor — Sistema de Monitoramento de Enchentes

## 💡 Descrição do Problema
As enchentes impactam diretamente a vida de milhares de pessoas, causando prejuízos materiais e colocando vidas em risco. A falta de monitoramento adequado dificulta a prevenção e a resposta rápida. Este projeto surge como uma solução de apoio ao monitoramento de nível de água.

---

## 🛠️ Visão Geral da Solução
O **AquaGuard Monitor** é um sistema desenvolvido com Arduino que permite:

- 📟 Monitorar o nível de água em tempo real.
- 🟢 🟡 🔴 Alertar visualmente (LEDs) e sonoramente (buzzer) conforme o risco.
- 💧 Exibir as informações no display LCD.
- 🖥️ Mostrar dados também no monitor serial.

---

## 🔗 Acesse o Projeto no Simulador
- ▶️ **Wokwi:** [Link do projeto no Wokwi](https://wokwi.com/projects/432700736901896193)
- 🎥 **Vídeo Demonstração:** [Link para o vídeo no YouTube](https://youtu.be/SEU_LINK_AQUI)

---

## 📦 Componentes Utilizados

| Componente       | Descrição                           |
|------------------|--------------------------------------|
| Arduino Uno      | Microcontrolador principal          |
| Sensor Ultrassônico (HC-SR04) | Mede nível de água     |
| Display LCD 16x2 I2C | Mostra informações              |
| LED Verde        | Status normal                       |
| LED Amarelo      | Status de atenção                   |
| LED Vermelho     | Status crítico                      |
| Buzzer           | Alerta sonoro em risco crítico      |
| Jumpers + Resistores | Conexões dos LEDs               |

---

## 🔌 Esquema de Conexões

| Componente        | Pinos Arduino      |
|-------------------|---------------------|
| Trigger (HC-SR04) | 6                   |
| Echo (HC-SR04)    | 7                   |
| LED Verde         | 2                   |
| LED Amarelo       | 3                   |
| LED Vermelho      | 4                   |
| Buzzer            | 5                   |
| LCD SDA           | A4                  |
| LCD SCL           | A5                  |

---

## 🏗️ Como Simular no Wokwi
1. Acesse o link do projeto no Wokwi.
2. Clique em **Start Simulation**.
3. Use o controle deslizante do sensor HC-SR04 para simular o nível da água.
4. Veja o status mudar no LCD, nos LEDs e ouvirá o buzzer em nível crítico.

---

## 🧠 Funcionamento do Sistema
- 🔸 **Distância > 200cm:** Status **Normal** — LED Verde aceso.
- 🔸 **100cm < Distância ≤ 200cm:** Status **Atenção** — LED Amarelo aceso.
- 🔸 **Distância ≤ 100cm:** Status **Crítico** — LED Vermelho e Buzzer ativos.

---

## 📜 Exemplo de Código

```cpp
// Projeto: Monitoramento de nível de água
// Leandro Simoneli da Silva - RM 566539

#include <Wire.h>
#include <LiquidCrystal_I2C.h>

LiquidCrystal_I2C lcd(0x27, 16, 2);

const int trigPin = 6;
const int echoPin = 7;
const int ledVerde = 2;
const int ledAmarelo = 3;
const int ledVermelho = 4;
const int buzzer = 5;

long duracao;
float distancia;

void setup() {
  lcd.init();
  lcd.backlight();
  Serial.begin(9600);

  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);
  pinMode(ledVerde, OUTPUT);
  pinMode(ledAmarelo, OUTPUT);
  pinMode(ledVermelho, OUTPUT);
  pinMode(buzzer, OUTPUT);

  lcd.setCursor(0, 0);
  lcd.print("AquaGuard Ready");
  lcd.setCursor(0, 1);
  lcd.print("Monitorando...");
  delay(2000);
  lcd.clear();
}

void loop() {
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);

  duracao = pulseIn(echoPin, HIGH);
  distancia = duracao * 0.034 / 2;

  Serial.print("Distancia: ");
  Serial.print(distancia);
  Serial.println(" cm");

  lcd.clear();
  lcd.setCursor(0, 0);
  lcd.print("Distancia:");
  lcd.print(distancia);
  lcd.print("cm");

  if (distancia > 200) {
    digitalWrite(ledVerde, HIGH);
    digitalWrite(ledAmarelo, LOW);
    digitalWrite(ledVermelho, LOW);
    digitalWrite(buzzer, LOW);
    lcd.setCursor(0, 1);
    lcd.print("Status: Normal");
  }
  else if (distancia <= 200 && distancia > 100) {
    digitalWrite(ledVerde, LOW);
    digitalWrite(ledAmarelo, HIGH);
    digitalWrite(ledVermelho, LOW);
    digitalWrite(buzzer, LOW);
    lcd.setCursor(0, 1);
    lcd.print("Status: Atencao");
  }
  else {
    digitalWrite(ledVerde, LOW);
    digitalWrite(ledAmarelo, LOW);
    digitalWrite(ledVermelho, HIGH);
    digitalWrite(buzzer, HIGH);
    lcd.setCursor(0, 1);
    lcd.print("Status: Critico");
  }

  delay(1500);
}
```

# 🧑‍💻 Desenvolvedor
# Leandro Simoneli da Silva
# RM: 566539

