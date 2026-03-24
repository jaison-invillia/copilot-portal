# ADR-0001: Adopt Clean Architecture for the Backend

## Status
Accepted

## Context
O projeto precisa de uma arquitetura que:

- mantenha **separação clara de responsabilidades**
- facilite **testabilidade** (principalmente de regras de negócio)
- reduza acoplamento a frameworks e infraestrutura (DB, logging, observability)
- permita evolução do sistema sem reescritas grandes
- seja simples de orientar desenvolvimento humano e assistido por IA

O backend terá regras de negócio que tendem a crescer com o tempo. Uma arquitetura "framework-first" pode tornar mudanças mais custosas e dificultar manutenção.

## Decision
Adotaremos **Clean Architecture** no backend, com dependências apontando **sempre para dentro** (domínio).

Camadas e responsabilidades (alto nível):

- **Domain**
  - Entidades e regras de negócio puras (sem framework)
- **Application**
  - Use cases (commands/queries) e ports (interfaces) necessárias ao domínio
- **Interfaces**
  - Adaptadores de entrada (HTTP controllers, routes, DTOs, middlewares)
- **Infrastructure**
  - Adaptadores de saída (DB repositories, providers externos, logging, observability)
- **Main**
  - Composition root: wiring/DI, bootstrap e inicialização do server

Regra de dependências:

```
interfaces      ─┐
infrastructure  ─┼──> application ───> domain
main            ─┘
```

Regras obrigatórias:
- `domain` não importa `application`, `interfaces` ou `infrastructure`
- `application` não importa `interfaces` ou `infrastructure` (depende de ports)
- controllers não contêm regra de negócio
- repositories não contêm regra de negócio

A estrutura de pastas recomendada e detalhes completos estão em:
- `docs/architecture.md`

## Consequences

### Positive
- **Alta testabilidade**: use cases e domínio testáveis sem DB/HTTP.
- **Baixo acoplamento**: trocar framework, logger, ou camada de persistência é mais simples.
- **Evolução mais segura**: regras de negócio ficam centralizadas e consistentes.
- **Clareza para IA**: reduz alucinações e “mistura de camadas” durante geração de código.
- **Manutenibilidade**: facilita onboarding e revisões.

### Negative
- **Maior custo inicial**: mais pastas, interfaces e wiring.
- **Mais boilerplate**: ports, DTOs e mappers.
- **Disciplina necessária**: exige revisão para evitar violações (principalmente em PRs rápidos).

### Mitigations
- Reforçar regras em `docs/engineering-guidelines.md` e no checklist de PR.
- Adotar Code Review Agent focado em limites de camadas.
- Começar simples (DI manual no `main/`) e evoluir conforme necessidade.

## Notes
- Esta ADR define o **estilo arquitetural** do backend.
- Decisões complementares (auth, banco, etc.) devem ser registradas em ADRs separadas.
