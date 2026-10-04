# Segurança

- O token do bot e o Chat ID são dados sensíveis. Nunca os inclua no código versionado, no README, em issues ou em logs publicados.
- Crie `include/config.h` localmente a partir de `include/config.example.h`. O primeiro está ignorado pelo Git; o segundo contém somente placeholders.
- O token encontrado no firmware original deve ser revogado no BotFather. Crie outro token e mantenha-o apenas no arquivo local de configuração.
- Senhas Wi-Fi devem ser informadas pelo portal do WiFiManager e nunca escritas no código.
- A conexão com o Telegram usa TLS via `WiFiClientSecure` e certificado raiz. O funcionamento depende de hora correta e da cadeia de certificados suportada pelo firmware.
- Um binário de firmware compilado com `config.h` real pode conter as credenciais. Não publique arquivos de build (`.pio/`, `*.bin`, `*.elf`) feitos com dados reais.
- Antes de um commit, revise `git diff --cached`, procure por segredos com `git grep -n -i -E 'token|password|passwd|secret|chat_id|authorization|bearer' -- .` e, se disponível, execute `gitleaks` sobre o conteúdo versionado. Avalie os resultados manualmente, pois nomes de configuração e placeholders são esperados.

O comando `/status` no firmware atual não confere o chat de origem; a resposta vai para o Chat ID configurado. Para uso em ambiente exposto, a verificação do remetente deve ser implementada e testada. Confira também a segurança do portal Wi-Fi e do hardware de detecção elétrica.
