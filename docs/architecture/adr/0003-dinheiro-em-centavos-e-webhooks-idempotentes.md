# ADR 0003 — Dinheiro em centavos e webhooks idempotentes

Status: aceita

## Decisão
Valores monetários como inteiros (centavos); cobrança guarda bruto, taxa e líquido; preço congelado no item do atendimento. Eventos de webhook são persistidos com o ID do provedor e processados no máximo uma vez. Registros financeiros e clínicos nunca são apagados (cancelamento/estorno com motivo).

## Consequências
Conciliação e auditoria simples; um pouco mais de código para conversão na borda de UI/API.
