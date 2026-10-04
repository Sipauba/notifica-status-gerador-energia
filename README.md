# ESP32 Generator & Power Monitor

Sistema embarcado com ESP32 para monitoramento de gerador, energia elétrica e temperatura com alertas via Telegram.

O projeto foi criado para acompanhar remotamente um ambiente com gerador de energia. Um sensor/microfone indica o ruído do motor; entradas adicionais informam a presença da rede elétrica e a temperatura. O ESP32 mostra os estados em um LCD 16×2 e envia mensagens quando detecta mudanças. O comando `/status` permite consultar o estado atual.

## Recursos

- Detecção digital do gerador por sensor/microfone, com confirmação de aproximadamente 5 segundos para ligar e desligar.
- Leitura digital da presença de energia elétrica.
- Leitura de termistor por ADC e cálculo de temperatura com coeficientes Steinhart–Hart do firmware original.
- LCD I2C 16×2 no endereço `0x27`.
- Configuração de Wi-Fi por portal do WiFiManager e botão físico para apagar a rede salva.
- Alertas e consulta de status via bot do Telegram, com conexão TLS.

## Componentes e dependências

Use um ESP32 compatível com o perfil genérico `esp32dev`, um sensor/microfone com **saída digital compatível com 3,3 V**, termistor com divisor resistivo, detector **isolado** para a rede elétrica, botão e LCD I2C 16×2. Confira o [mapa de GPIOs e os cuidados elétricos](docs/hardware.md) antes de conectar o circuito.

O projeto usa Arduino Framework e as bibliotecas WiFiManager, LiquidCrystal_I2C, UniversalTelegramBot e ArduinoJson. `WiFi` e `WiFiClientSecure` fazem parte do core Arduino para ESP32. As dependências externas estão em `platformio.ini`.

## Preparação e compilação

1. Instale o [PlatformIO](https://platformio.org/install) e abra este diretório como projeto.
2. Copie `include/config.example.h` para `include/config.h`.
3. Substitua os dois placeholders no arquivo local por um **novo** token de bot e pelo Chat ID de destino. Não envie `include/config.h` para o Git.
4. Execute `pio run` para compilar e `pio run -t upload` para gravar a placa. Use `pio device monitor` para acompanhar a serial a 115200 baud.

O modelo `esp32dev` é uma escolha genérica: confirme a placa, a alimentação e o módulo LCD antes da gravação. As instruções do [Telegram](docs/telegram.md) e do [Wi-Fi](docs/wifi.md) explicam a configuração em mais detalhe.

## Exemplo de mensagens

```text
Rede elétrica: DESLIGADA
Rede elétrica: LIGADA
Gerador: LIGADO
Temperatura da sala: 26.50 C
Gerador: DESLIGADO
Temperatura da sala: 27.10 C
```

Os exemplos são fictícios. O firmware acrescenta uma linha separadora a algumas mensagens.

## Documentação

- [Arquitetura](docs/arquitetura.md)
- [Hardware e segurança elétrica](docs/hardware.md)
- [Funcionamento e limitações atuais](docs/funcionamento.md)
- [Telegram](docs/telegram.md)
- [Wi-Fi e botão de reset](docs/wifi.md)
- [Segurança das credenciais](docs/seguranca.md)
- [Melhorias futuras](docs/melhorias-futuras.md)

O arquivo original fornecido pelo autor foi preservado fora deste repositório. `src/main.cpp` é uma cópia com as credenciais removidas e o comando de status sem referência a um usuário de bot específico.
