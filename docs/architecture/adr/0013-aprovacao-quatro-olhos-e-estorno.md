# ADR 0013 — Aprovação "4 olhos" e estorno rastreável

Status: aceita

## Contexto
Duas ações movem dinheiro sem passar pelo caminho normal de pagamento: **estornar** e **deixar a receber**. Em pet shop pequeno a mesma pessoa que atende também cobra, o que facilita erro e fraude; ao mesmo tempo, travar tudo no dono inviabiliza o sábado.

## Decisão
- **Capacidade decide o caminho:** quem pode (`financeiro.estornar`/`aprovar`) executa direto, com seu nome na trilha; quem não pode **solicita**, e outra pessoa autorizada decide. Ninguém decide o próprio pedido.
- **Política por estabelecimento** (limite de "a receber", padrão R$ 100): abaixo, fluxo livre e auditado; acima, aprovação. Zero = sempre.
- **Pedido tem prazo (24 h) e é definitivo** depois de decidido (trigger). Aprovar revalida o estado: aprovação nunca aplica uma ação que virou impossível.
- **Estorno**: valor limitado ao ainda não devolvido sob trava de linha; Pix/cartão passam pelo gateway e, se ele falhar, **nada muda no caixa** (a falha fica registrada; nova tentativa = novo estorno). Estorno e decisão não se editam nem se apagam.
- **Caixa**: o estorno entra no dia em que o dinheiro saiu.

## Consequências
+ Controle proporcional ao risco, sem travar o balcão; trilha de quem pediu e quem autorizou. − Depende de haver uma segunda pessoa autorizada disponível (mitigação: o limite configurável e o prazo). Ainda sem aviso ativo ao aprovador (WhatsApp/e-mail) — o selo no menu é o único alerta.
