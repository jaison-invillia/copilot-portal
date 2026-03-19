# Educational Portal

Portal educacional com cursos online em videoaulas do YouTube.

O sistema oferece autenticação de alunos, catálogo de cursos, registro de progresso por aula e emissão de certificado digital após conclusão do curso.

## Objetivo e Escopo

O projeto resolve a jornada completa de aprendizagem em uma única plataforma:

- cadastro e login de alunos
- acesso a cursos e aulas
- acompanhamento de progresso em tempo real
- geração de certificado ao concluir 100% do curso

## Funcionalidades Principais

### 1. Cadastro e autenticação

- registro de conta com email único
- login com JWT
- acesso autenticado aos recursos de curso

### 2. Catálogo de cursos

- listagem paginada de cursos
- detalhe do curso com aulas ordenadas
- visualização de total de aulas por curso

### 3. Videoaulas

- aulas vinculadas a URLs do YouTube
- reprodução embutida no frontend
- organização por ordem de exibição dentro do curso

### 4. Progresso por aula

- marcação de conclusão por aula
- endpoint idempotente para concluir aula
- cálculo de percentual de conclusão do curso

### 5. Certificados

- emissão quando o curso atinge 100%
- unicidade por aluno e curso
- download em PDF sob demanda

## Visão Técnica Rápida

- frontend: Next.js com SSR
- backend: Node.js com Clean Architecture
- banco de dados: MySQL 8
- observabilidade: New Relic + logs estruturados com requestId

Referências técnicas:

- arquitetura: [docs/architecture.md](docs/architecture.md)
- domínio: [docs/domain.md](docs/domain.md)
- banco de dados: [docs/database.md](docs/database.md)
- contratos de API: [docs/api-spec.md](docs/api-spec.md)
- diretrizes de engenharia: [docs/engineer-guidelines.md](docs/engineer-guidelines.md)
- estrutura do projeto: [docs/project-structure.md](docs/project-structure.md)
- observabilidade: [docs/observability.md](docs/observability.md)
- segurança: [docs/security.md](docs/security.md)

## Como Começar

Use as instruções de setup local em [docs/local-setup.md](docs/local-setup.md).

## Desenvolvimento com IA

O repositório utiliza fluxo com agentes especializados para planejamento, implementação, revisão, testes, observabilidade e segurança.

Guias principais:

- fluxo de agentes: [docs/agent-task-flow.md](docs/agent-task-flow.md)
- definição de agentes: [AGENTS.md](AGENTS.md)
- contexto para IA: [docs/ai/ai-context.md](docs/ai/ai-context.md)

## Licença

A definir.
