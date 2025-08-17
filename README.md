# NLW Agents

Projeto desenvolvido durante o evento da **Rocketseat** para criar uma API de gerenciamento de salas com integração de agentes inteligentes.

## 🚀 Tecnologias Utilizadas

- **Node.js** - Runtime JavaScript
- **TypeScript** - Tipagem estática
- **Fastify** - Framework web rápido e eficiente
- **Drizzle ORM** - ORM moderno para TypeScript
- **PostgreSQL** - Banco de dados relacional
- **pgvector** - Extensão para operações com vetores
- **Google Gemini AI** - IA para transcrição de áudio e geração de embeddings
- **Zod** - Validação de esquemas
- **Docker** - Containerização
- **Biome** - Linter e formatador de código

## 🏢️ Arquitetura e Padrões

- **API REST** com Fastify
- **Type Safety** com TypeScript e Zod
- **Database First** com Drizzle ORM
- **Vector Database** com pgvector para embeddings
- **RAG (Retrieval-Augmented Generation)** para respostas contextualizadas
- **Semantic Search** com similaridade de vetores
- **AI Services** com Google Gemini para transcrição, embeddings e geração de respostas
- **Multipart Upload** para processamento de arquivos de áudio
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
GEMINI_API_KEY=sua_chave_da_api_do_gemini
```

> **Nota:** Para obter a chave da API do Gemini, acesse [Google AI Studio](https://aistudio.google.com/) e gere uma nova chave de API.

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
- `POST /rooms/:roomId/questions` - Cria uma nova pergunta e gera resposta automaticamente usando RAG
- `POST /rooms/:roomId/audio` - Faz upload de áudio para transcrição e geração de embeddings

## 🤖 Funcionalidades de IA

O projeto integra o Google Gemini AI para fornecer recursos avançados de processamento de áudio e texto:

### Transcrição de Áudio
- **Modelo utilizado:** `gemini-2.5-flash`
- **Funcionalidade:** Converte arquivos de áudio para texto em português brasileiro
- **Características:** Transcrição precisa e natural com pontuação adequada

### Geração de Embeddings
- **Modelo utilizado:** `text-embedding-004`
- **Funcionalidade:** Converte texto em vetores de 768 dimensões
- **Propósito:** Permite busca semântica e similaridade entre conteúdos
- **Armazenamento:** Utiliza pgvector para operações eficientes com vetores

### Busca Semântica e RAG
- **Similaridade por Cosseno:** Utiliza operação `<=>` do pgvector para cálculo de distância
- **Threshold de Similaridade:** 0.7 (scores acima indicam alta relevância)
- **Limite de Resultados:** Até 3 chunks mais relevantes por consulta
- **Contexto Inteligente:** Combina múltiplas transcrições para respostas mais precisas

### Geração de Respostas Contextualizadas
- **Modelo utilizado:** `gemini-2.5-flash`
- **Abordagem:** RAG (Retrieval-Augmented Generation)
- **Processo:** Busca conteúdo relevante e gera resposta baseada no contexto
- **Características:** Respostas em português, ton educativo, citações do "conteúdo da aula"

### Fluxo de Processamento de Áudio
1. **Upload:** Arquivo de áudio é enviado via multipart/form-data
2. **Transcrição:** Gemini AI converte o áudio em texto
3. **Embeddings:** Texto é transformado em vetor de embeddings
4. **Armazenamento:** Transcrição e embeddings são salvos no banco de dados
5. **Resposta:** Retorna o ID do chunk criado para referência futura

### Fluxo de Processamento de Perguntas
1. **Pergunta:** Usuário envia uma pergunta via API
2. **Embedding da Pergunta:** Pergunta é convertida em vetor de embeddings
3. **Busca Semântica:** Sistema busca chunks de áudio similares (similaridade > 0.7)
4. **Seleção de Contexto:** Até 3 transcrições mais relevantes são selecionadas
5. **Geração de Resposta:** Gemini AI gera resposta baseada no contexto encontrado
6. **Armazenamento:** Pergunta e resposta são salvas no banco de dados
7. **Resposta:** Retorna o ID da pergunta e a resposta gerada

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

- **audio_chunks** - Tabela para armazenar transcrições de áudio e embeddings
  - `id` (UUID) - Identificador único
  - `room_id` (UUID) - Referência à sala (FK para rooms.id)
  - `transcription` (TEXT) - Transcrição do áudio em texto
  - `embeddings` (VECTOR(768)) - Vetor de embeddings gerado pela IA
  - `created_at` (TIMESTAMP) - Data de criação

## 📝 Desenvolvimento

O projeto está configurado para usar o novo suporte experimental do Node.js para TypeScript (`--experimental-strip-types`), eliminando a necessidade de transpilação durante o desenvolvimento.

---

Desenvolvido com ❤️ durante o evento NLW da Rocketseat
