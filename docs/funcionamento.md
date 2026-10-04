# Funcionamento atual

No `setup()`, o ESP32 configura as entradas, inicia a serial e o LCD, executa o WiFiManager e associa o certificado raiz à conexão segura do Telegram.

No `loop()`, o firmware consulta novas mensagens do bot a cada aproximadamente 10 segundos, verifica o botão de reset, calcula a temperatura a partir do termistor, lê a entrada de energia e acompanha a saída digital do sensor/microfone. O LCD apresenta `G: ON` ou `G: OFF`, `E: ON` ou `E: OFF` e a temperatura.

## Estados e alertas

- A entrada de energia em `LOW` representa rede desligada; `HIGH`, rede ligada. O alerta é enviado quando a flag interna muda.
- A entrada do microfone em `HIGH` inicia a contagem para ligar o gerador. Depois de cerca de 5 segundos, o gerador é marcado como ligado e um alerta pode ser enviado.
- Uma leitura `LOW` durante a contagem de partida cancela essa contagem. Quando o gerador já está ligado, `LOW` inicia uma contagem de aproximadamente 5 segundos para desligar.
- A temperatura é calculada com a equação Steinhart–Hart e incluída nas mensagens do gerador e na resposta a `/status`.

## Limitações observadas no código

O código foi preservado com mudanças mínimas de segurança e documentação. Alguns comportamentos precisam de validação em bancada:

- O botão configurado como `INPUT_PULLUP` é acionado no código quando a leitura é `HIGH`. Isso pode provocar reinícios contínuos no circuito comum de botão para GND.
- Durante a contagem para desligar, o retorno do sinal `HIGH` não cancela explicitamente `gerador_desligando`; o temporizador pode concluir mesmo com o ruído de volta.
- A expressão `R2 = R1 * (3.3 / voltage - 1)` não trata `voltage == 0`; uma leitura ADC zero pode produzir valor inválido.
- `delay(1000)` e a espera pela hora via NTP bloqueiam temporariamente o loop. O envio Telegram depende da disponibilidade do Wi-Fi e de hora/certificado válidos.
- O estado inicial da rede é presumido ligado até uma leitura `LOW`; não há alerta inicial para rede presente.

As [melhorias futuras](melhorias-futuras.md) registram possíveis correções. Os tempos e coeficientes originais não foram alterados nesta organização.
