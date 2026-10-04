# Arquitetura

O ESP32 é o controlador central. Ele lê as entradas, calcula a temperatura, atualiza o LCD e consulta/envia mensagens pelo Telegram usando a conexão Wi-Fi.

```mermaid
flowchart TD
    TEMP[Termistor e divisor resistivo] --> ADC[ADC GPIO34]
    POWER[Detector isolado da rede elétrica] --> EN[Entrada GPIO25]
    GEN[Sensor/microfone digital] --> MIC[Entrada GPIO15]
    ADC --> ESP[ESP32]
    EN --> ESP
    MIC --> ESP
    ESP --> LCD[LCD I2C 16×2]
    ESP --> WIFI[Wi-Fi / WiFiManager]
    WIFI --> TLS[Conexão TLS]
    TLS --> TELEGRAM[Telegram Bot API]
    BTN[Botão GPIO2] --> ESP
```

O firmware usa `millis()`, `delay()` e flags booleanas para controlar as transições. A leitura do microfone é digital; o estado do gerador é inferido do nível detectado ao longo do tempo, não de uma análise espectral do som. O LCD e o Telegram apresentam a inferência feita pelo firmware.
