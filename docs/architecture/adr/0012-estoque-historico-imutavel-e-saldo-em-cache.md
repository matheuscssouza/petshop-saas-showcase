# ADR 0012 — Estoque: histórico imutável como verdade, saldo em cache protegido pelo banco

Status: aceita

## Decisão
O histórico de movimentos é append-only (trigger bloqueia UPDATE/DELETE). O saldo por produto e por lote é um **cache** atualizado sob `SELECT … FOR UPDATE` na mesma transação do movimento, protegido por `CHECK (saldo >= 0)`. Saída segue FEFO e ignora lote vencido. Uma rotina de **conferência** compara cache e histórico e falha alto se divergirem.

## Por quê
Somar o histórico a cada venda seria lento em produtos com muito giro; só guardar o saldo perderia a trilha. O cache é barato, e o histórico imutável + conferência permitem **provar** que o saldo está certo (ou detectar quando alguém mexeu por fora).

## Consequências
+ Estoque negativo e adulteração silenciosa viram impossíveis/detectáveis. − Correção de erro é sempre um novo movimento (ajuste/estorno), nunca edição — desejado.
