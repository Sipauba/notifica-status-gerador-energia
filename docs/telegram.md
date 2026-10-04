# Telegram

O firmware usa `UniversalTelegramBot` e `WiFiClientSecure` para consultar a API do Telegram. O certificado raiz é configurado com `TELEGRAM_CERTIFICATE_ROOT`. Antes de enviar alertas, a função de envio tenta sincronizar o relógio por NTP para validar a conexão TLS.

## Credenciais locais

Copie `include/config.example.h` para `include/config.h`. No arquivo local, preencha `BOT_TOKEN` com um token **novo** obtido no BotFather e `CHAT_ID` com o destino das mensagens. O arquivo local está no `.gitignore` e nunca deve entrar em commits. Um token presente em uma versão anterior do firmware deve ser revogado no BotFather.

## Alertas e comando

O firmware envia alertas quando a rede elétrica muda de estado e quando o gerador passa a ligado ou desligado. A cada aproximadamente 10 segundos, consulta mensagens recebidas pelo bot. Uma mensagem cujo texto contém `/status` produz um resumo com o estado do gerador, a temperatura e o estado da rede elétrica.

O código atual envia respostas sempre para o `CHAT_ID` configurado, mesmo quando o comando chega de outro chat. Ele não valida o remetente do comando. Mantenha o bot privado e trate a verificação do chat de origem como melhoria antes de expor o bot publicamente. A primeira consulta pode ocorrer antes de o relógio ser sincronizado, o que também requer teste de conexão TLS na placa.
