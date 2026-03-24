# 🗄️ Database

<!-- TEMPLATE: Preencha este documento com o schema do banco de dados do seu projeto. -->
<!-- Consulte docs/engineering-docs-recommendation.md para orientações detalhadas. -->

Este documento descreve o **modelo físico de dados** do projeto.

Para o modelo de domínio e regras de negócio consulte: `docs/domain.md`

Para decisões de arquitetura consulte: `docs/architecture.md`

---

## ✅ Princípios

- **[PREENCHER]** Banco de dados escolhido (ex.: MySQL 8, PostgreSQL 15, etc.)
- **FKs e constraints** habilitadas.
- **Chaves artificiais** (ex.: `BIGINT UNSIGNED AUTO_INCREMENT`) para entidades principais.
- **Auditoria mínima** com `created_at` / `updated_at`.
- **Unicidade** garantida por índices únicos conforme domínio.
- **Integridade referencial** com FKs e regras de `ON DELETE`/`ON UPDATE`.

---

## 🧱 Tabelas

<!-- Repita esta seção para cada tabela do sistema. -->

### [nome_da_tabela]

> **[PREENCHER]** Descreva cada tabela seguindo o modelo abaixo.

**Descrição:** Breve descrição do propósito da tabela.

#### Campos

- `id`: identificador interno
- `[campo]`: descrição
- `created_at`, `updated_at`

#### Regras/Constraints

- [Constraint 1, ex.: campo X **único**]
- [Constraint 2, ex.: FK para tabela Y]

---

## 🧠 Observações importantes (Domínio ↔ Banco)

<!-- Descreva cálculos, derivações ou regras que conectam domínio e banco. -->

> **[PREENCHER]** Documente regras de negócio que impactam o schema (ex.: cálculos de completude, validações no Service Layer).

---

## 🧾 Schema SQL

<!-- Cole o DDL de referência para migrations. -->

> **[PREENCHER]** Adicione o schema SQL base do projeto.

```sql
-- [Banco escolhido]
-- Engine/Charset recomendados

-- Exemplo:
-- CREATE TABLE [nome] (
--   id BIGINT UNSIGNED NOT NULL AUTO_INCREMENT,
--   ...
--   PRIMARY KEY (id)
-- );
```
