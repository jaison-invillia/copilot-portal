# 🗂️ Project Structure

Este documento define a **estrutura recomendada do repositório** e convenções de organização do código para o projeto.

Objetivo:
- reduzir ambiguidade na criação de arquivos/pastas
- orientar contribuições humanas e assistidas por IA
- reforçar limites de camadas (Clean Architecture)

Referências:
- `README.md`
- `docs/architecture.md`
- `docs/engineering-guidelines.md`
- `docs/ai/ai-context.md`
- `docs/agent-task-flow.md`

---

## 📦 Estrutura do repositório (top-level)

Recomendado:

```
project-root/
  README.md

  docs/
    architecture.md
    domain.md
    database.md
    api-spec.md
    engineering-guidelines.md
    observability.md
    security.md
    ai/ai-context.md
    agent-task-flow.md
    project-structure.md

    adr/
      0001-clean-architecture.md
      0008-structured-logging.md
      [outros ADRs do projeto]

  backend/
    package.json
    src/
      ...

  frontend/
    package.json
    pages/
      ...
```

---

## 🧱 Backend (Clean Architecture)

### Estrutura de pastas (recomendada)

```
backend/
  src/
    domain/
      entities/
      value-objects/          # opcional
      errors/

    application/
      use-cases/
      ports/
      dto/                    # opcional (modelos de entrada/saída do app, não HTTP)

    interfaces/
      http/
        controllers/
        routes/
        middlewares/
        dto/                  # request/response DTOs (HTTP)
      mappers/                # DTO <-> modelos app/domain

    infrastructure/
      db/
        connection/
        migrations/
      repositories/
      logging/
      observability/
      providers/              # pdf generator, email, etc (futuro)

    main/
      container/              # composition root / wiring
      server/                 # server bootstrap
      app.ts
```

### Regras rápidas
- **Domain**: zero dependência de framework/DB.
- **Application**: use cases + ports; não importa infra/interfaces.
- **Interfaces**: controllers/DTOs; sem regra de negócio.
- **Infrastructure**: DB/repos/providers/logging/New Relic.
- **Main**: única camada que “junta tudo”.

Detalhes completos em: `docs/architecture.md`.

---

## 🧪 Testes (backend)

Recomendação de organização:

```
backend/
  test/
    unit/
      domain/
      application/
    integration/
      repositories/
      http/
    e2e/                      # mínimo viável (fluxo feliz)
```

Regras:
- Unit tests focam em `domain` e `application`.
- Integration tests cobrem repos e endpoints críticos.
- E2E cobre o fluxo principal do sistema.

---

## 🌐 Frontend (Next.js)

### Estrutura de pastas (recomendada)

```
frontend/
  pages/
    index.tsx
    login.tsx
    [recurso]/
      index.tsx               # listagem
      [id].tsx                # detalhe (SSR)

  components/
  hooks/
  services/
    api-client.ts             # wrapper HTTP
    auth.ts                   # login/token helpers
    [recurso].ts              # clients por recurso

  styles/
```

### Regras
- Client da API deve refletir `docs/api-spec.md`.
- Não duplicar regras de domínio no frontend (backend é fonte de verdade).
- UI deve lidar com:
  - loading
  - erros (mensagens amigáveis)
  - token expirado (redirect para login)

---

## 📄 Convenções de arquivos

### Nomeação
- Pastas e arquivos: `kebab-case` (preferencial) **ou** padrão do framework (ex.: `pages/[id].tsx`).
- Classes: `PascalCase`
- Funções/variáveis: `camelCase`
- Constantes: `UPPER_SNAKE_CASE`

### Onde colocar o quê
- Contratos de API: `docs/api-spec.md`
- Regras de domínio: `docs/domain.md`
- Schema: `docs/database.md`
- Observabilidade: `docs/observability.md`
- Segurança: `docs/security.md`
- Decisões: `docs/adr/*`
- Contexto para IA: `docs/ai/ai-context.md`

---

## 🤖 Regras para IA

- Não criar arquivos fora desta estrutura sem justificar.
- Se uma nova pasta for necessária:
  - propor mudança em `docs/project-structure.md` antes.
- Manter mudanças pequenas e em camadas corretas.
