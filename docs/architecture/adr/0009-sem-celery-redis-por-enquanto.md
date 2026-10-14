# ADR 0009 — Sem fila (Celery/Redis) por enquanto

Status: parcialmente substituída pelo ADR 0011 (fila de mensagens = outbox no PostgreSQL)

## Contexto
O panorama recomenda fila para integrações externas. Hoje a única chamada externa é a criação do Pix (síncrona, timeout de 15 s) e o webhook (processamento curto).

## Decisão
Não carregar Celery/Redis sem uso (superfície de ataque e operação). Introduzir quando entrarem WhatsApp, Google Agenda e emissão fiscal — o contrato de eventos internos (ADR 0006) permite migrar reações para tarefas assíncronas.

## Consequências
Criar Pix pode bloquear a requisição se o gateway estiver lento (risco aceito no piloto).
