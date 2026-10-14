# Petshop SaaS — Showcase

Estudo de caso público de um SaaS multi-tenant de gestão para **pet shop + veterinária**
(banho/tosa de alto fluxo, clínica, pagamentos Pix/cartão com split, WhatsApp, fiscal).

O código-fonte e os dados de clientes são **privados**. Este repositório documenta as decisões de engenharia:

- [`docs/architecture/overview.md`](docs/architecture/overview.md) — arquitetura em camadas e assíncrono
- [`docs/architecture/adr/`](docs/architecture/adr) — decisões (monólito modular, multi-tenancy com RLS, dinheiro em centavos/idempotência)
- [`docs/domain/data-model.md`](docs/domain/data-model.md) — modelo de dados e máquinas de estado

**Stack:** Python/Django · PostgreSQL (RLS) · Celery + Redis · Next.js · Docker · GitHub Actions.
**Integrações:** Asaas (Pix/cartão/split), WhatsApp Cloud API, Google Calendar API.

© Matheus Souza. Conteúdo para fins de portfólio; todos os direitos reservados.
