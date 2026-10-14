# ADR 0007 — Baseline de segurança e privacidade

Status: aceita

## Decisão
- **Configuração que falha fechada:** fora de DEBUG a aplicação recusa subir com chave fraca, host `*` ou gateway fake.
- **Defesa em profundidade para dados:** filtro no ORM + Row-Level Security + papel de banco sem superuser.
- **Mínimo necessário no front:** contato/CPF só para quem precisa; busca por POST; histórico filtrado por unidade; CSP sem JS inline; `no-store` em toda resposta; logs só com ids.
- **Autenticação:** Argon2, senha ≥10, bloqueio por usuário+IP, sessão de 10 h.
- **Cadeia de suprimentos:** lock com hashes gerado no Python mais antigo suportado, `pip-audit`, bandit, gitleaks e Dependabot na CI; imagem não-root, somente leitura.
- **Exceção deliberada:** `contas_usuario` e `cadastros_tenant` sem RLS (login ocorre antes de conhecer o tenant); compensada por não haver listagem de usuários para papéis de tenant.

## Consequências
Algumas conveniências são perdidas (busca por link compartilhável, cache do navegador). Aceito: privacidade acima de conforto.
