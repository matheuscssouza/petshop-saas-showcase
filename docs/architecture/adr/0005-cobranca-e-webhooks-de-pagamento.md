# ADR 0005 — Cobrança com bruto/taxa/líquido e webhooks idempotentes

Status: aceita

## Decisão
- Cada atendimento gera no máximo uma cobrança; o preço do serviço é congelado no item no check-in.
- A cobrança guarda **bruto, taxa e líquido**; pagamentos parciais são aceitos, excedente é recusado, e a quitação acontece quando o saldo zera.
- Eventos do gateway entram numa tabela de eventos única por `(origem, evento_id)` e o pagamento é idempotente por `(método, id da transação)` — reentrega não paga duas vezes.
- O endpoint de webhook é autenticado por token (fail-closed) e distingue erro de dados (registra e responde 200, evitando retry infinito) de falha inesperada (5xx, o gateway tenta de novo).
- A entrega do pet só é liberada com cobrança paga ou "a receber" com responsável identificado.
- Fechamento de caixa é um snapshot imutável por unidade e dia; recebimentos posteriores aparecem como divergência.

## Consequências
Conciliação e auditoria simples; um pouco mais de código que um modelo "valor + status".
