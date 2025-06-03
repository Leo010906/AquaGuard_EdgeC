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
// Projeto: Monitoramento de nível de água (enchentes)
// Aluno: Leandro Simoneli da Silva - RM 566539
// Disciplina: Edge Computing

// Bibliotecas necessárias para usar o display LCD
#include <Wire.h>               
#include <LiquidCrystal_I2C.h> 

// Configuração do LCD (endereço 0x27, 16 colunas e 2 linhas)
LiquidCrystal_I2C lcd(0x27, 16, 2);

// Definindo os pinos do sensor ultrassônico
const int trigPin = 6;  // Pino que envia o sinal
const int echoPin = 7;  // Pino que recebe o sinal

// Definindo os pinos dos LEDs
const int ledVerde = 2;     // LED para status normal
const int ledAmarelo = 3;   // LED para status de atenção
const int ledVermelho = 4;  // LED para status crítico

// Definindo o pino do buzzer
const int buzzer = 5;       // Buzzer para alarme sonoro

// Variáveis para armazenar os dados do sensor
long duracao;     // Armazena o tempo que o sinal leva para ir e voltar
float distancia;  // Calcula a distância em cm

// Função que roda uma única vez ao iniciar
void setup() {
  // Inicia o LCD
  lcd.init();
  lcd.backlight();  // Liga a luz de fundo do LCD

  // Inicia o monitor serial para ver dados no computador
  Serial.begin(9600);

  // Define os modos dos pinos
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);

  pinMode(ledVerde, OUTPUT);
  pinMode(ledAmarelo, OUTPUT);
  pinMode(ledVermelho, OUTPUT);
  pinMode(buzzer, OUTPUT);

  // Mensagem inicial no LCD
  lcd.setCursor(0, 0);
  lcd.print("AquaGuard Ready");
  lcd.setCursor(0, 1);
  lcd.print("Monitorando...");
  delay(2000);  // Espera 2 segundos
  lcd.clear();  // Limpa a tela do LCD
}

// Função que roda em loop
void loop() {
  // Envia um sinal ultrassônico
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);

  digitalWrite(trigPin, HIGH);  // Envia pulso
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);   // Para o pulso

  // Mede o tempo que o sinal levou para retornar
  duracao = pulseIn(echoPin, HIGH);

  // Calcula a distância com base no tempo
  distancia = duracao * 0.034 / 2;

  // Mostra no monitor serial
  Serial.print("Distancia: ");
  Serial.print(distancia);
  Serial.println(" cm");

  // Mostra a distância no LCD
  lcd.clear();
  lcd.setCursor(0, 0);
  lcd.print("Distancia:");
  lcd.print(distancia);
  lcd.print("cm");

  // Verifica a distância e acende os LEDs conforme o risco

  if (distancia > 200) {
    // Situação normal
    digitalWrite(ledVerde, HIGH);
    digitalWrite(ledAmarelo, LOW);
    digitalWrite(ledVermelho, LOW);
    noTone(buzzer); // Desliga o buzzer

    lcd.setCursor(0, 1);
    lcd.print("Status: Normal");
  }

  else if (distancia <= 200 && distancia > 100) {
    // Atenção
    digitalWrite(ledVerde, LOW);
    digitalWrite(ledAmarelo, HIGH);
    digitalWrite(ledVermelho, LOW);
    noTone(buzzer); // Desliga o buzzer

    lcd.setCursor(0, 1);
    lcd.print("Status: Atencao");
  }

  else {
    // Crítico - risco de enchente
    digitalWrite(ledVerde, LOW);
    digitalWrite(ledAmarelo, LOW);
    digitalWrite(ledVermelho, HIGH);
    tone(buzzer, 1000); // Para ligar o alarme

    lcd.setCursor(0, 1);
    lcd.print("Status: Critico");
  }

  // Espera 1,5 segundos antes de fazer uma nova leitura
  delay(1500);
}

```

# 🧑‍💻 Desenvolvedor
# Leandro Simoneli da Silva
# RM: 566539

