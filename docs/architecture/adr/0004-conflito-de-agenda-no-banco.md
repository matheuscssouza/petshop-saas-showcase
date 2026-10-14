# ADR 0004 — Conflito de agenda garantido pelo banco

Status: aceita

## Contexto
Em dia de alto fluxo, duas pessoas podem agendar o mesmo profissional ou a mesma sala ao mesmo tempo. Validar só na aplicação deixa uma janela de corrida.

## Decisão
`EXCLUDE USING gist` (PostgreSQL, extensão `btree_gist`) sobre o intervalo `[inicio, fim)` por profissional e por sala, restrito aos status que ocupam agenda. Cancelado, falta e remarcado liberam o horário; horários encostados (fim = início) não conflitam. A camada de serviço traduz a violação em erro de domínio.

## Consequências
+ Sem corrida, independentemente de quantas instâncias da aplicação existam.
− Acopla ao PostgreSQL (aceito: já é premissa por causa do RLS).
