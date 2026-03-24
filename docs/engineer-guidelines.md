# 🧭 Engineering Guidelines

Este documento define padrões e regras de engenharia para o projeto.

Referências:
- Visão geral: `README.md`
- Arquitetura: `docs/architecture.md`
- Domínio: `docs/domain.md`
- Banco: `docs/database.md`
- API: `docs/api-spec.md`

---

## ✅ Objetivos

- Aumentar previsibilidade e qualidade do código.
- Reduzir retrabalho em PRs (padrões claros).
- Facilitar colaboração humano + IA (Copilot/agents).
- Manter o sistema testável e evolutivo.

---

## 🏗️ Arquitetura

As decisões arquiteturais (incluindo **Clean Architecture**, camadas e regras de dependência) estão definidas em:

➡ `docs/architecture.md`

**Regra:** todas as contribuições devem respeitar os limites e responsabilidades descritos nesse documento.

---

## 🧾 Padrões de Código

### Linguagem e padrão
- Backend: Node.js (TypeScript recomendado; se começar em JS, manter consistência)
- Frontend: Next.js (TypeScript recomendado)
- Preferir **código explícito** ao “mágico” (principalmente com IA).

### Nomes
- Pastas: `kebab-case` (ex.: `order-items`)
- Arquivos TS/JS: `kebab-case` (ex.: `order-service.ts`)
- Classes: `PascalCase`
- Funções/variáveis: `camelCase`
- Constantes: `UPPER_SNAKE_CASE`

### DTOs
- Requests/responses HTTP devem usar **DTOs**, não entidades do domínio diretamente.
- DTOs HTTP vivem em `interfaces/http/dto`.
- Não “vazar” detalhes de infraestrutura (ex.: campos de banco) para DTOs públicos.

---

## 🧪 Testes

### Pirâmide recomendada
- **Unit tests (principal foco):** domain + application
- **Integration tests:** repositories + API endpoints críticos
- **E2E (mínimo viável):** fluxo feliz principal

### Regras
- Cada Use Case deve ter testes de:
  - fluxo feliz
  - validação de entrada
  - casos de erro esperados (ex.: curso não existe)
- Use Cases devem ser testados com **mocks das ports** (interfaces), não com DB real.

---

## 🔐 Segurança

- JWT obrigatório para rotas protegidas (ver `docs/api-spec.md`).
- Nunca logar:
  - senha
  - token completo
  - dados sensíveis
- Validar input em todos endpoints.
- Proteger contra SQL injection (queries parametrizadas / ORM).
- Rate limit (futuro) para endpoints de auth.

---

## 📦 Persistência

- Migrations versionadas em `backend/src/infrastructure/db/migrations` (ou equivalente).
- Constraints do banco **devem refletir regras do domínio** quando possível.
- Unicidade conforme definido em `docs/database.md`.

---

## 🧰 Observabilidade & Logs

### New Relic
- Instrumentar API e capturar erros não tratados.
- Garantir correlação por `requestId` (ver padrão de erro no `docs/api-spec.md`).

### Logging (a definir)
Requisitos mínimos para o logger:
- níveis: `debug`, `info`, `warn`, `error`
- logs estruturados (JSON)
- incluir `requestId` sempre que possível
- não incluir dados sensíveis

---

## 🔁 Git Workflow

- Branches:
  - `main`: estável
  - `feat/<tema>`: features
  - `fix/<tema>`: correções
  - `chore/<tema>`: manutenção
- PR obrigatório para merge em `main`.
- PR deve incluir:
  - descrição do que mudou
  - checklist de testes
  - link de issue (se houver)

---

## ✅ Definition of Done (DoD)

Uma tarefa só é “done” quando:
- [ ] código implementado respeitando `docs/architecture.md`
- [ ] testes (unit/integration) adicionados quando aplicável
- [ ] lint/format passando
- [ ] endpoints aderentes a `docs/api-spec.md`
- [ ] migrations incluídas quando houver alteração de schema
- [ ] logs e tratamento de erro adequados
- [ ] documentação atualizada (se necessário)

---

## 🤖 Regras para IA (Copilot / Agents)

- Sempre ler antes de codar:
  - `README.md`
  - `docs/architecture.md`
  - `docs/domain.md`
  - `docs/database.md`
  - `docs/api-spec.md`
  - este documento `docs/engineering-guidelines.md`
- Não inventar endpoints nem tabelas fora da documentação.
- Em caso de dúvida: propor alteração na documentação **antes** de gerar código.
- Evitar “embelezamento” de código (emojis, comentários redundantes, etc.).
- Preferir pequenas mudanças com commits/PRs menores e revisáveis.
