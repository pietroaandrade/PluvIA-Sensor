# Monitor de Inundação com ESP32

Este projeto implementa um sistema de monitoramento de inundação utilizando um microcontrolador ESP32. Ele mede o nível da água usando um sensor analógico, calcula o risco de inundação, exibe as informações em uma tela LCD, fornece indicação visual de risco usando LEDs e soa um alarme com um buzzer para situações de alto risco. O sistema também publica os dados de monitoramento em um broker MQTT.

## Funcionalidades

*   **Medição do Nível da Água:** Lê dados analógicos de um sensor de nível da água.
*   **Avaliação de Risco:** Determina o risco de inundação (BAIXO, MÉDIO, ALTO) com base no nível da água medido.
*   **Display LCD:** Mostra informações de monitoramento de inundação em tempo real (Localização, Nível, Risco).
*   **Indicadores LED:** LEDs Verde, Amarelo e Vermelho representam visualmente os níveis de risco BAIXO, MÉDIO e ALTO, respectivamente.
*   **Alarme Sonoro:** Ativa um buzzer quando o nível de risco é ALTO.
*   **Comunicação MQTT:** Publica dados de monitoramento de inundação (localização, nível, risco) em um tópico MQTT especificado.
*   **Configurável:** Altere facilmente a localização do sensor, credenciais de Wi-Fi e configurações do servidor MQTT no código.

## Requisitos de Hardware

*   Placa de Desenvolvimento ESP32
*   Sensor Analógico de Nível da Água
*   Display LCD I2C (20x4 ou compatível)
*   LEDs Verde, Amarelo e Vermelho
*   Buzzer
*   Resistores (para LEDs, se necessário)
*   Fios Jumper
*   Protoboard (opcional)

## Diagrama do Circuito

Conecte os componentes conforme mostrado no diagrama abaixo:

![Esquema do Circuito](ESP32scheme.png)

**Conexões dos Pinos:**

*   Pino de Sinal do Sensor de Nível da Água -> ESP32 GPIO 36 (VP)
*   LCD I2C SDA -> ESP32 GPIO 21
*   LCD I2C SCL -> ESP32 GPIO 22
*   LED Verde -> ESP32 GPIO 25
*   LED Amarelo -> ESP32 GPIO 26
*   LED Vermelho -> ESP32 GPIO 27
*   Buzzer -> ESP32 GPIO 13
*   VCC do Sensor de Nível da Água -> ESP32 3.3V/5V
*   GND do Sensor de Nível da Água -> ESP32 GND
*   VCC do LCD -> ESP32 5V
*   GND do LCD -> ESP32 GND
*   LEDs e Buzzer: Conecte o outro terminal através de um resistor ao GND (LEDs) ou diretamente ao GND (Buzzer, se for ativo).

## Requisitos de Software

*   Arduino IDE ou VS Code com extensão PlatformIO
*   Pacote de Suporte para Placas ESP32 para Arduino IDE
*   Bibliotecas Necessárias:
    *   `Wire.h` (geralmente integrada)
    *   `LiquidCrystal_I2C.h`
    *   `WiFi.h` (geralmente integrada)
    *   `PubSubClient.h`

Você pode instalar as bibliotecas necessárias através do Gerenciador de Bibliotecas do Arduino IDE ou do gerenciamento de bibliotecas do PlatformIO.

## Integração com Node-RED

Este projeto pode se integrar com o Node-RED para visualizar os dados e criar dashboards. Você precisará dos seguintes nós do Node-RED instalados:

*   `node-red-contrib-aedes` (MQTT Broker)
*   `node-red-contrib-mqtt-broker` (Outro nó de MQTT Broker, escolha um com base nas suas necessidades)
*   `node-red-dashboard`
*   `node-red-node-serialport` (Se você precisar ler dados seriais, embora este projeto use MQTT)

![Node-RED Libraries](Dashboard/Node-Red_libraries.png)

## Configuração e Uso

1.  **Abra o Projeto:** Abra o arquivo `main.ino` no seu Arduino IDE ou projeto PlatformIO.
2.  **Instale as Bibliotecas:** Certifique-se de ter instalado as bibliotecas `LiquidCrystal_I2C` e `PubSubClient`.
3.  **Configure as Definições:**
    *   Atualize a variável `sensorID` com o nome de localização desejado.
    *   Insira seu SSID e senha do Wi-Fi nas variáveis `ssid` e `password`.
    *   Verifique ou altere o `mqttServer` e `mqttPort` se estiver usando um broker MQTT diferente.
4.  **Carregue o Código:** Selecione a placa ESP32 e a porta corretas, então carregue o código para o seu ESP32.
5.  **Monitoramento:**
    *   Observe o nível da água, localização e nível de risco no LCD.
    *   Note os indicadores LED e o buzzer para os níveis de risco.
    *   Monitore a saída serial para informações detalhadas, incluindo os dados JSON sendo publicados.
    *   Assine o tópico MQTT `flood-monitor/data` no seu broker MQTT especificado para receber os dados publicados.

## Calibração

O mapeamento dos valores analógicos do sensor para centímetros (`level_cm = map(waterLevel, 0, 4095, 0, 30);`) assume uma relação linear e um nível máximo de 30 cm correspondendo à leitura analógica máxima (4095). Pode ser necessário ajustar os valores `0` e `30` na função `map()` com base no seu sensor específico e na faixa que você deseja medir.

Os limites de risco (30% e 70%) também podem ser ajustados na função `loop()` para corresponder aos seus níveis de risco desejados.

## Capturas de Tela

Aqui estão algumas capturas de tela mostrando o display LCD em diferentes níveis de água e risco:

**Risco Médio (13.0 cm)**
![Risco Médio](Dashboard/Medium-risk.png)

**Risco Alto (30.0 cm)**
![Risco Alto](Dashboard/High-risk.png)

**Risco Baixo (0.0 cm)**
![Risco Baixo](Dashboard/Low-risk.png)