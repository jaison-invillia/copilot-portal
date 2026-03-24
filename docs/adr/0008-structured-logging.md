# ADR-0008: Use Structured JSON Logging with Request Correlation

## Status
Accepted

## Context
O projeto precisa de um padrão consistente de **logging** para:

- diagnosticar erros rapidamente
- correlacionar requisições entre logs, erros e traces
- facilitar análise em ferramentas de observabilidade
- permitir troubleshooting em produção

Sem um padrão claro de logging, problemas comuns incluem:

- logs inconsistentes
- dificuldade de correlacionar eventos
- ausência de contexto em erros
- dificuldade de indexação em ferramentas de observabilidade

Alternativas consideradas:

### 1. Logs simples em texto
- Fácil de implementar
- Difícil de indexar e consultar
- Baixa padronização

### 2. Logs estruturados (JSON)
- Facilmente indexáveis
- Melhor integração com ferramentas de observabilidade
- Permite filtros e dashboards

### 3. Logging distribuído completo (OpenTelemetry + tracing completo)
- Muito poderoso
- Complexidade maior para MVP

Para o MVP e evolução futura, **logs estruturados em JSON com correlação de request** são a melhor base.

## Decision

Adotar **logs estruturados em JSON** com **requestId** para correlação de requisições.

Cada requisição HTTP terá um identificador único.

Esse identificador será:

- recebido via header `X-Request-Id` (quando presente)
- ou gerado pela API quando ausente

Esse valor será incluído em:

- logs da aplicação
- payload de erros da API
- atributos de observabilidade

### Header padrão

Entrada:

```
X-Request-Id
```

Saída:

```
X-Request-Id
```

### Estrutura padrão de log

Exemplo de log:

```
{
  "timestamp": "2026-03-04T12:00:00Z",
  "level": "info",
  "service": "[nome-do-projeto]-api",
  "message": "request completed",
  "requestId": "req_12345",
  "http": {
    "method": "GET",
    "path": "/api/v1/resources",
    "status": 200,
    "durationMs": 32
  },
  "userId": 1
}
```

### Níveis de log

Os níveis utilizados serão:

- `debug`
- `info`
- `warn`
- `error`

### Regras importantes

- Nunca logar:
  - senha
  - hash de senha
  - JWT completo
  - dados sensíveis

- Logs devem conter **requestId sempre que possível**.

## Consequences

### Positive

- Logs facilmente indexáveis
- Melhor correlação entre erros e requisições
- Facilita integração com ferramentas de observabilidade
- Melhora debugging em produção

### Negative

- Logs estruturados podem ser mais verbosos
- Requer disciplina para manter padrão

### Mitigations

- Centralizar criação de logger
- Criar middleware de logging HTTP
- Documentar padrão em `docs/observability.md`

## Notes

Esta decisão impacta:

- Observabilidade (`docs/observability.md`)
- Segurança (`docs/security.md`)
- Implementação de middlewares HTTP

Ferramentas de observabilidade como **New Relic** poderão consumir esses logs e correlacioná-los com traces e métricas.
