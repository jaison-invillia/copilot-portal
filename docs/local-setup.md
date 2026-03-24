# 🛠️ Local Setup

<!-- TEMPLATE: Preencha este documento com as instruções de setup do seu projeto. -->

Este documento descreve como preparar e executar o projeto localmente (backend + frontend).

Referências:
- Visão geral: `README.md`
- Estrutura do repositório: `docs/project-structure.md`
- API: `docs/api-spec.md`
- Banco: `docs/database.md`

---

## ✅ Pré-requisitos

### Ferramentas
- **Node.js** (LTS recomendado)
- **npm** ou **pnpm** (usar um padrão no repo)
- **[PREENCHER]** Banco de dados (ex.: MySQL 8, PostgreSQL, etc.)
- Git

### Opcional (recomendado)
- Docker / Docker Compose (para subir dependências e facilitar setup)

---

## 📦 Clonar o repositório

```bash
git clone <repo-url>
cd <repo-folder>
```

---

## 🗄️ Banco de dados

> **[PREENCHER]** Instruções para configurar o banco de dados do projeto.

### Opção A) Banco local instalado

1. Inicie o banco de dados
2. Crie o banco:

```sql
CREATE DATABASE [nome_do_banco] CHARACTER SET utf8mb4;
```

### Opção B) Docker (recomendado)

> **[PREENCHER]** Adicione ou referencie o `docker-compose.yml` do projeto.

```bash
docker compose up -d
```

---

## ⚙️ Variáveis de ambiente

### Backend

Crie o arquivo `backend/.env` baseado em `.env.example`:

> **[PREENCHER]** Liste as variáveis de ambiente necessárias.

```env
# Server
PORT=3001
NODE_ENV=development

# Database
DB_HOST=localhost
DB_PORT=3306
DB_NAME=[nome_do_banco]
DB_USER=root
DB_PASSWORD=root

# Auth
JWT_SECRET=change_me_dev_secret
JWT_EXPIRES_IN=3600

# Observability
NEW_RELIC_ENABLED=false
NEW_RELIC_LICENSE_KEY=
```

### Frontend

Crie o arquivo `frontend/.env.local`:

```env
NEXT_PUBLIC_API_BASE_URL=http://localhost:3001/api/v1
```

---

## 🔧 Backend

```bash
cd backend
npm install
```

### Rodar migrations

> A ferramenta de migration (ex.: Prisma, Knex, TypeORM, etc.) será definida na implementação.
> O schema de referência está em `docs/database.md`.

```bash
npm run migrate
```

### Rodar seeds (opcional)

> **[PREENCHER]** Adicione instruções de seed quando disponíveis.

```bash
npm run seed
```

### Iniciar

```bash
npm run dev
```

Backend disponível em `http://localhost:3001`.

---

## 🌐 Frontend

```bash
cd frontend
npm install
npm run dev
```

Frontend disponível em `http://localhost:3000`.

---

## ✅ Verificar

- Backend: `http://localhost:3001/health`
- Frontend: `http://localhost:3000`

---

## 🐛 Troubleshooting

| Problema | Solução |
|----------|---------|
| Erro de conexão com banco | Verificar se o banco está rodando e as credenciais em `.env` |
| Porta em uso | Alterar `PORT` no `.env` |
| Migrations falham | Verificar se o banco foi criado e credenciais estão corretas |

## 🧯 Troubleshooting

### Porta em uso
- Backend: ajuste `PORT` no `backend/.env`
- Frontend: ajuste porta do Next se necessário

### Erro de conexão com DB
Verifique:
- MySQL rodando
- credenciais em `backend/.env`
- porta `3306` liberada

### JWT inválido
Verifique:
- `JWT_SECRET` no backend
- token no header `Authorization`

---

## 📌 Onde salvar

Recomendado:
- `docs/local-setup.md`
