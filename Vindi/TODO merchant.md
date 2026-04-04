
- [ ] Notificalçao por email para o merchant, atualizacao dos status
	- O fluxo ja existe, a notificacao por webhooks pode ser usada para isso. O job passar pelas disputas checando se houve uma mudanca no status da disputa.
	- Notificações são apenas no momento da criação da disputa e finalização do processo.
	- no momento de criação da disputa o merchant ainda não esta registrado.
	- Inicialmente implementar para notificar conclusão da disputa.
	- discutir como fazer para criação da disputa.
	- Get email com flag especifica, um por organization.

```
	Para garantir de verdade que o e-mail de conclusão seja enviado só uma vez, você precisa de idempotência persistida em banco, não só comparação em memória.

1. Não confie apenas no hasStatusChanged
Hoje essa checagem evita duplicidade em um fluxo simples, mas pode falhar com concorrência, retries, jobs e múltiplos workers.

2. Crie um marcador de envio único na disputa
Adicione na tabela de contract_dispute algo como:
- merchant_conclusion_notified_at (timestamp, null)
- opcional: merchant_conclusion_notification_key (string única)

3. Faça o “claim” atômico antes de enviar
No ponto de notificação de conclusão, execute um update condicional:
- where id = ? and dispute_status = closed and merchant_conclusion_notified_at is null
- set merchant_conclusion_notified_at = now()
Se afetar 0 linhas, alguém já enviou (ou já reservou envio). Então não envia de novo.

4. Para garantia forte (recomendado): Outbox pattern
Melhor abordagem:
- Na mesma transação da mudança para closed, insere evento em tabela outbox com chave única, por exemplo dispute:{id}:merchant_conclusion
- Worker envia e-mail e marca o evento como enviado
- Índice único na chave impede duplicação, mesmo com retry

5. Regra prática para seu caso agora
Como você quer começar rápido:
- Implementa marcador merchant_conclusion_notified_at + update condicional atômico
- Se quiser “garantia enterprise”, evolui para outbox no próximo passo

Se quiser, eu já te proponho o desenho exato de migration + método de repositório + ponto de chamada no serviço para plugar isso no fluxo atual.
```


- [ ] Area de cadastro de merchant
- [ ] cadastrar pvs vindi em dev


# Pontos importantes
- a parte de resposta pro adquirente esta funcionando com a rede?
- 