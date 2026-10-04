# Melhorias futuras

Estas ideias não foram implementadas nesta organização do repositório:

- Corrigir e testar a polaridade do botão de reset; adicionar debounce e observar as restrições de inicialização de GPIO2.
- Tratar perda de Wi-Fi, reconexão ao Telegram, falhas da API e timeout da sincronização NTP.
- Validar o chat de origem do comando `/status`, autenticar o portal de configuração e permitir múltiplos destinatários ou Chat ID configurável sem recompilar.
- Adotar watchdog, logs estruturados, armazenamento de eventos e uma máquina de estados explícita para o gerador.
- Tornar configurável o tempo de detecção e cancelar corretamente a contagem de desligamento se o sinal retornar.
- Proteger o cálculo do termistor contra `voltage == 0` e leituras inválidas; calibrar `R1`, `c1`, `c2` e `c3` conforme o termistor real.
- Adicionar alertas de temperatura máxima e mínima, histórico de temperatura ou um sensor digital.
- Considerar atualização OTA, página web local, MQTT, Home Assistant, Grafana, Prometheus ou Zabbix conforme a necessidade do projeto.
