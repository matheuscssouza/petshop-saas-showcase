# ADR 0011 — Caixa de saída (outbox) no PostgreSQL em vez de Celery/Redis

Status: aceita · substitui a parte de "fila" do ADR 0009

## Contexto
Ao entrar o WhatsApp, o envio de mensagens passou a precisar de fila (reenvio, janela de horário, limite). O ADR 0009 previa Celery/Redis nesse momento.

## Decisão
**Outbox transacional no PostgreSQL**: a mensagem é gravada na mesma transação do fato que a originou e um worker a envia com `FOR UPDATE SKIP LOCKED`, lease contra queda do processo e espera crescente entre tentativas. Sem broker novo.

## Por quê
- A mensagem **só existe se o fato foi confirmado** (e vice-versa): sem dupla escrita nem perda entre banco e broker.
- Menos peças para operar e proteger (sem Redis); o isolamento por tenant (RLS) e a auditoria já cobrem a tabela.
- O volume é pequeno (dezenas a poucos milhares de mensagens/dia por estabelecimento).

## Consequências
+ Simplicidade e consistência. − Escala menor que um broker dedicado; se o volume exigir, o contrato (`enfileirar` / `processar_pendentes`) permite trocar o transporte sem mudar as regras. Revisar quando houver milhares de mensagens/minuto.
