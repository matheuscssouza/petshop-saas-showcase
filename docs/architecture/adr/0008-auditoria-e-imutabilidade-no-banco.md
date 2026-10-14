# ADR 0008 — Auditoria append-only e imutabilidade no banco

Status: aceita

## Decisão
Trilha `AuditoriaEvento` (tenant, usuário, IP, request id, ação, ids) escrita explicitamente por serviços e telas sensíveis, sem dados pessoais. Triggers PostgreSQL bloqueiam UPDATE/DELETE em pagamento, fechamento de caixa e auditoria, e DELETE em cobrança, itens, agendamento, atendimento, tutor e pet. Eliminação a pedido do titular = anonimização, não exclusão.

## Consequências
+ A garantia vale mesmo para SQL direto ou outro sistema com acesso ao banco.
− Correção de erro exige lançamento compensatório (estorno/ajuste), nunca edição — desejado em dados financeiros.
