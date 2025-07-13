# NLW Agents

Projeto desenvolvido durante o evento da **Rocketseat** para criar uma API de gerenciamento de salas com integração de agentes inteligentes.

## 🚀 Tecnologias Utilizadas

- **Node.js** - Runtime JavaScript
- **TypeScript** - Tipagem estática
- **Fastify** - Framework web rápido e eficiente
- **Drizzle ORM** - ORM moderno para TypeScript
- **PostgreSQL** - Banco de dados relacional
- **pgvector** - Extensão para operações com vetores
- **Zod** - Validação de esquemas
- **Docker** - Containerização
- **Biome** - Linter e formatador de código

## 🏗️ Arquitetura e Padrões

- **API REST** com Fastify
- **Type Safety** com TypeScript e Zod
- **Database First** com Drizzle ORM
- **Environment Variables** para configuração
- **Containerização** com Docker Compose
- **Validação de entrada** com fastify-type-provider-zod

## 📋 Pré-requisitos

- Node.js 18+ (com suporte a `--experimental-strip-types`)
- Docker e Docker Compose
- pnpm (gerenciador de pacotes)

## ⚙️ Configuração e Setup

### 1. Clone o repositório
```bash
git clone https://github.com/victorbadaro/letmeask-agents-server
cd letmeask-agents-server
```

### 2. Instale as dependências
```bash
pnpm install
```

### 3. Configure as variáveis de ambiente
Crie um arquivo `.env` na raiz do projeto:

```env
PORT=3333
DATABASE_URL=postgresql://docker:docker@localhost:5432/agents
```

### 4. Inicie o banco de dados
```bash
docker compose up -d
```

### 5. Execute as migrações do banco
```bash
pnpm db:migrate
```

### 6. (Opcional) Popule o banco com dados iniciais
```bash
pnpm db:seed
```

### 7. Inicie o servidor
```bash
# Modo desenvolvimento
pnpm dev

# Modo produção
pnpm start
```

## 🔧 Scripts Disponíveis

- `pnpm dev` - Inicia o servidor em modo desenvolvimento
- `pnpm start` - Inicia o servidor em modo produção
- `pnpm db:studio` - Abre o Drizzle Studio para visualizar e editar o banco de dados
- `pnpm db:generate` - Gera arquivos de migração do banco de dados
- `pnpm db:migrate` - Executa as migrações do banco de dados
- `pnpm db:seed` - Popula o banco com dados iniciais

## 📊 Endpoints

- `GET /health` - Verificação de saúde da aplicação
- `GET /rooms` - Lista todas as salas disponíveis
- `POST /rooms` - Cria uma nova sala
- `GET /rooms/:roomId/questions` - Lista todas as perguntas de uma sala específica
- `POST /rooms/:roomId/questions` - Cria uma nova pergunta em uma sala específica

## 🗄️ Banco de Dados

O projeto utiliza PostgreSQL com a extensão pgvector para operações com vetores. A estrutura principal inclui:

- **rooms** - Tabela para armazenar informações das salas
  - `id` (UUID) - Identificador único
  - `name` (TEXT) - Nome da sala
  - `description` (TEXT) - Descrição da sala
  - `created_at` (TIMESTAMP) - Data de criação

- **questions** - Tabela para armazenar as perguntas das salas
  - `id` (UUID) - Identificador único
  - `roomId` (UUID) - Referência à sala (FK para rooms.id)
  - `question` (TEXT) - Texto da pergunta
  - `answer` (TEXT) - Resposta da pergunta (opcional)
  - `created_at` (TIMESTAMP) - Data de criação

## 📝 Desenvolvimento

O projeto está configurado para usar o novo suporte experimental do Node.js para TypeScript (`--experimental-strip-types`), eliminando a necessidade de transpilação durante o desenvolvimento.

---

Desenvolvido com ❤️ durante o evento NLW da Rocketseat
