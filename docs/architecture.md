# 🏗️ Architecture

Este documento descreve as **decisões arquiteturais** do projeto.

Para visão geral consulte: `README.md`

Para regras do domínio consulte: `docs/domain.md`

---

## 🧭 Architectural Style

O backend segue os princípios de **Clean Architecture**, com dependências apontando **sempre para dentro** (domínio).

### Camadas (alto nível)

- **Domain**: entidades e regras de negócio puras (sem framework)
- **Application**: casos de uso (use cases) e portas (interfaces) que o domínio precisa
- **Interfaces**: adaptadores de entrada (HTTP controllers, DTOs, middlewares)
- **Infrastructure**: adaptadores de saída (DB/repositories, providers externos, logger, observability)
- **Main**: composição/bootstrapping (DI, rotas, wiring)

### Regras de dependência (obrigatórias)

```
interfaces      ─┐
infrastructure  ─┼──> application ───> domain
main            ─┘
```

**Permitido**
- `interfaces` pode importar `application` (para chamar use cases) e tipos do `domain` quando necessário
- `infrastructure` pode importar `application` (para implementar ports) e tipos do `domain` quando necessário
- `main` pode importar todos (apenas para compor)

**Proibido**
- `domain` importar qualquer coisa de `application`, `interfaces` ou `infrastructure`
- `application` importar `interfaces` ou `infrastructure`
- Controllers conterem regra de negócio
- Repositories conterem regra de negócio

---

## 🧩 Responsabilidades por camada

### Domain
- Entidades de negócio
- Regras invariantes do domínio
- Value Objects (se aplicável)

**Não contém**
- SQL, HTTP, DTOs de API, bibliotecas de framework, logging, New Relic

### Application
- Use cases (commands/queries) que implementam a lógica do negócio
- Ports (interfaces) para persistência e serviços externos
- Modelos de saída (quando necessário) que não são entidades HTTP

> **[PREENCHER]** Liste os use cases do projeto. Exemplos:
> - `RegisterUser`
> - `LoginUser`
> - `Create[Entity]`
> - `Get[Entity]Details`
> - `List[Entities]`

### Interfaces (Input Adapters)
- Rotas e controllers HTTP
- DTOs de request/response (alinhados com `docs/api-spec.md`)
- Middlewares (auth, requestId, error mapping)

**Regra**: controller transforma HTTP → input do use case e output → HTTP (sem negócio).

### Infrastructure (Output Adapters)
- Implementações concretas de ports (ex.: `UserRepositoryMySQL`)
- Conexão com DB e migrations
- Integrações externas (ex.: geração de PDF, serviços futuros)
- Logger e instrumentação (New Relic)

### Main (Composition Root)
- Cria instâncias concretas
- Faz wiring de dependências (DI manual)
- Define rotas e inicia o server

---

## 📦 Estrutura recomendada do repositório

### Backend

```
backend/
  src/
    domain/
      entities/
      value-objects/
      errors/

    application/
      use-cases/
      ports/
      dto/                # opcional: modelos de entrada/saída do application (não HTTP)

    interfaces/
      http/
        controllers/
        routes/
        middlewares/
        dto/              # request/response DTOs (HTTP)
      mappers/            # conversões DTO <-> modelos do application/domain

    infrastructure/
      db/
        connection/
        migrations/
      repositories/       # implementações MySQL dos ports
      observability/
      logging/
      providers/          # pdf generator, email, etc (futuro)

    main/
      server/
      container/          # composition root / wiring
      app.ts              # bootstrap
```

### Frontend (Next.js)

```
frontend/
  pages/
  components/
  services/              # client da API
  hooks/
  styles/
```

---

## 🔐 Autenticação

- Autenticação baseada em **JWT** (Bearer).
- Rotas protegidas devem validar token via middleware.
- Contratos e exemplos: `docs/api-spec.md`.

---

## 📈 Observability

- Instrumentar backend com **New Relic** (APM + erros).
- Padronizar `requestId` para correlação em logs e erros.
- Erros devem ser mapeados para o padrão especificado em `docs/api-spec.md`.

---

## 🔒 Segurança

Obrigatório:
- validação de input em endpoints
- queries parametrizadas / ORM para evitar SQL injection
- não logar tokens/senhas/dados sensíveis
- controles de acesso para rotas autenticadas

---

## 📈 Evolução Arquitetural

Possíveis melhorias futuras (quando necessidade aparecer):
- cache (Redis) para leituras intensas
- processamento assíncrono (filas) para operações pesadas
- containerização e escalabilidade horizontal
