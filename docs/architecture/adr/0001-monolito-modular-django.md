# ADR 0001 — Monólito modular em Django

Status: aceita

## Contexto
Time de uma pessoa, MVP a validar com um cliente real, depois replicar para outros.

## Decisão
Backend único em Django + PostgreSQL, organizado em apps por domínio (`cadastros`, `agenda`, `clinico`, `financeiro`, `fiscal`, `estoque`, `comunicacao`, `integracoes`). Jobs assíncronos com Celery + Redis. Frontend Next.js separado consumindo API REST.

## Consequências
+ Velocidade (admin, auth, ORM, migrações prontos); deploy simples.
− Async menos natural que FastAPI; fronteiras entre apps exigem disciplina (sem imports cruzados de models; comunicar via serviços/eventos).
Alternativa descartada por ora: Spring Boot (mais robusto, mais lento para validar).
