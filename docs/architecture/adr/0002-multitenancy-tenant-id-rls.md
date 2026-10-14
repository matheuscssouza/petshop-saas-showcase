# ADR 0002 — Multi-tenancy por `tenant_id` + Row-Level Security

Status: aceita

## Decisão
Banco e schema únicos; `tenant_id` em toda tabela de negócio, filtro obrigatório na camada de acesso e RLS do PostgreSQL como segunda barreira. Teste automatizado de isolamento entre tenants na CI.

## Consequências
+ Barato de operar, onboarding de cliente = linha nova.
− Risco de vazamento se um filtro for esquecido (mitigado por RLS + testes). Schema-por-tenant fica como opção futura para clientes grandes.
