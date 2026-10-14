# Arquitetura

```
[Web recepção/vet/admin]  [Área do tutor]  [WhatsApp bot]
              \                |                /
               ───── API REST (JWT + RBAC) ─────
                               |
      ┌──────── Monólito modular (domínios) ────────┐
      │ cadastros · agenda · clinico · financeiro   │
      │ fiscal · estoque · comunicacao · integracoes│
      └─────────────────────────────────────────────┘
   PostgreSQL (RLS)   Redis   Workers Celery   Storage S3
   Integrações: gateway de pagamento · WhatsApp · Google Agenda · emissor fiscal
```

## Princípios
- **Monólito modular**: fronteiras claras por domínio, extraível no futuro.
- **Assíncrono para tudo externo**: WhatsApp, Google Agenda, fiscal e webhooks via fila com retry.
- **Multi-tenant**: `tenant_id` em toda tabela + Row-Level Security.
- **Fonte da verdade**: o SaaS manda; Google Agenda é espelho (evita loops de sync).
- **Dados sensíveis**: prontuário imutável após fechado (correção por adendo), RBAC por perfil, auditoria.
- **Pagamentos**: webhook autenticado, idempotente, conciliação diária.

## Isolamento de tenants em duas camadas
1. **ORM:** manager escopado pelo tenant ativo; sem tenant, a consulta falha (fail-closed).
2. **Banco:** Row-Level Security com `FORCE`, policy lendo `app.tenant_id` da sessão; a aplicação conecta com papel sem superuser. Um teste-guarda reprova na CI qualquer modelo de tenant sem policy.
