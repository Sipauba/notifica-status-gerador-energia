# Wi-Fi e portal de configuração

O WiFiManager tenta usar uma rede já salva. Se não conseguir, abre um ponto de acesso chamado `ESP32` e um portal para informar SSID e senha. O firmware primeiro configura um timeout de 10 segundos para o portal; se `autoConnect()` falhar, exibe no LCD o nome do AP e o endereço `192.168.4.1`, desativa esse timeout e executa o portal novamente. Após a configuração, o WiFiManager salva a rede e a conexão prossegue. O código não força um reinício nesse fluxo.

Para configurar a rede:

1. Ligue o ESP32 e aguarde o portal se não houver rede utilizável.
2. Conecte seu dispositivo ao AP `ESP32`.
3. Acesse o portal de configuração; se necessário, abra `192.168.4.1`.
4. Escolha a rede e informe a senha no portal. As credenciais ficam armazenadas no ESP32 pelo WiFiManager.

O botão físico em GPIO2 chama `resetSettings()` e reinicia o ESP32 para permitir uma nova configuração. **Confira antes o comportamento do botão na bancada:** o firmware usa `INPUT_PULLUP`, mas testa `HIGH` para apagar as configurações. Com um botão comum conectado ao GND, `HIGH` ocorre quando ele está solto, o que pode causar reinícios repetidos. Nenhuma senha Wi-Fi é incluída neste repositório.
