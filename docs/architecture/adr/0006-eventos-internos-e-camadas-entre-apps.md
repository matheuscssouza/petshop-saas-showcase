# ADR 0006 — Eventos internos e camadas entre apps

Status: aceita

## Contexto
`agenda` precisava reagir ao financeiro (itens no check-in, bloqueio de entrega sem pagamento) e o financeiro precisava da agenda; a solução inicial usava imports tardios e criava ciclos.

## Decisão
Dependências seguem camadas: `devtools → financeiro → agenda → cadastros → contas → integracoes → core` (acima importa abaixo, nunca o contrário), verificadas na CI com import-linter. Quem origina o evento não conhece quem reage: `core.hooks` (síncrono, na mesma transação) com veto por `VetoError`. O financeiro registra suas reações no `ready()`.

## Consequências
+ Domínios testáveis isoladamente; novo módulo (clínico, fiscal) pluga sem tocar na agenda.
− Fluxo menos "óbvio" ao ler o código (mitigado: nomes de eventos são constantes em um só arquivo). Se virar assíncrono no futuro, o contrato dos eventos já está definido.
