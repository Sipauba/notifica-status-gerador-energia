# Hardware e segurança elétrica

| Função | Pino ou interface | Leitura no firmware |
| --- | --- | --- |
| Termistor e divisor resistivo | GPIO34 | Analógica, ADC |
| Sensor/microfone do gerador | GPIO15 | Digital; `HIGH` indica ruído |
| Detector de presença da rede elétrica | GPIO25 | Digital; `HIGH` indica rede presente |
| Botão de reset do Wi-Fi | GPIO2 | Digital, `INPUT_PULLUP` |
| Display LCD 16×2 | I2C, endereço `0x27` | `LiquidCrystal_I2C` |

O código não define os GPIOs de SDA/SCL. Eles dependem da configuração I2C padrão da placa/core utilizado. Confirme o pinout da sua placa e os níveis elétricos reais de cada módulo antes de montar o circuito. Os GPIOs devem receber níveis compatíveis com 3,3 V.

> **ATENÇÃO:** o ESP32 opera em baixa tensão. Nunca conecte GPIOs diretamente à rede elétrica de 127 V ou 220 V. A detecção da rede exige isolamento galvânico e um circuito apropriado, como optoacoplador, relé ou módulo isolado projetado para essa finalidade. O circuito de isolamento existente não é identificado pelo código.

O termistor usa um divisor resistivo com `R1 = 10000` e coeficientes Steinhart–Hart definidos no firmware. Esses valores foram mantidos, pois podem refletir calibração do hardware original. O sensor/microfone precisa fornecer uma saída digital; o firmware chama `digitalRead()`, apesar de um comentário original mencioná-lo como entrada analógica.

GPIO2 pode influenciar o modo de inicialização em algumas placas ESP32. Verifique também o circuito do botão. O código configura `INPUT_PULLUP`, mas testa `HIGH` para executar o reset; com um botão ligado ao GND, isso pode apagar a rede e reiniciar sem pressão. Esse comportamento deve ser conferido em hardware antes do uso.
