# ADR 0010 — Prontuário imutável após fechado, correção por adendo

Status: aceita

## Contexto
O prontuário é registro profissional com responsabilidade do veterinário; alterá-lo depois de fechado compromete a defesa jurídica de todos os envolvidos.

## Decisão
Ciclo aberto → fechado. Fechar exige diagnóstico e conduta, registra data e responsável e libera o atendimento para cobrança. Depois de fechado, o prontuário não aceita UPDATE nem DELETE (trigger no PostgreSQL, além do serviço); itens (prescrições) só mudam com o prontuário aberto. Correções e complementos entram como **adendo** append-only, com autor e data congelados. Vacina errada é **anulada com motivo**, nunca apagada. Dados clínicos não aparecem em logs, trilha de auditoria nem no admin da plataforma.

## Consequências
+ Garantia verificável (testes batem direto no banco). − Erro de digitação antes de fechar é corrigível; depois, só adendo — o que é o comportamento desejado.
