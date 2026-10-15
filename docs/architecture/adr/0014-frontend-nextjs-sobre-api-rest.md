# ADR 0014 — Frontend em Next.js sobre uma API REST do próprio Django

Status: aceito (14/10/2026) · Substitui parcialmente: telas em templates Django (migração gradual)

## Contexto
As telas funcionais eram templates Django com CSP sem JavaScript inline. Ficaram corretas, mas sem a interação que o
cliente pediu (calendário dia/semana/mês/ano) e sem espaço para componentes de interface modernos (shadcn/ui,
motion-primitives). Duas alternativas foram descartadas: replicar os componentes em JS puro (muito retrabalho
e sem fidelidade) e ilhas React dentro do Django (duas formas de fazer tela e um build paralelo).

## Decisão
- **API REST** em `apps/api/apps/rest` (Django REST Framework), prefixo `/api/v1/`. Mesma sessão e mesmo CSRF do Django;
  nenhum token em JavaScript (o cookie de sessão é `HttpOnly`). A API **reaproveita** o RBAC (`tem_capacidade`), o
  contexto de tenant e o RLS: o front não decide nada de segurança.
- **Contrato** em `apps/api/openapi.yml`, gerado por drf-spectacular e versionado. Um teste falha se o arquivo estiver
  desatualizado, e o CI falha se `schema.d.ts` (tipos do front) divergir do contrato.
- **Frontend** em `apps/web`: Next.js, React, Tailwind, shadcn/ui e motion-primitives. O navegador fala só com o Next
  (`/api/v1/*` é reescrito para o Django), o que dispensa CORS e mantém cookies de mesma origem.
- **CSP estrita no Next** (`src/proxy.ts`): nonce por requisição, sem `unsafe-inline` para scripts nem para elementos
  `<style>` em produção. Rotas sem sessão redirecionam ao login; a autorização real é sempre da API.
- **Migração tela a tela** (estrangulamento): o menu vem da API e cada item aponta para a tela do Next quando já migrou,
  ou para a tela Django equivalente (mesma sessão) quando não.
- **Privacidade por construção**: serializers explícitos, sem `fields = "__all__"`. O calendário devolve pet, serviço e
  horário, nunca tutor, telefone ou CPF; a visão anual devolve só contagens.

## Consequências
- Mais uma peça (Node) no build e no deploy; o ganho é UI interativa e componentes reaproveitáveis.
- Cada tela nova exige endpoint, teste de permissão por unidade e teste de que não vaza dados pessoais.
- E2E (Playwright, Chrome real) valida login, visões e ausência de erros de CSP e de hidratação.
- Pendente: job de E2E no CI (precisa de Django, banco e seed), headers de segurança do Next em produção atrás do proxy
  reverso e telas restantes (painel, agenda, tutores, clínico, financeiro, estoque, comunicação).

## Origem dos componentes
shadcn/ui (MIT) e motion-primitives (MIT, ibelick/motion-primitives, copiados do repositório porque o registro do CLI
não respondeu). Foram descartados efeitos de marketing (texto embaralhado, tilt, cursor, spotlight): o critério é servir à
operação da recepção e do caixa.
