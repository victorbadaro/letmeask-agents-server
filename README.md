# NLW Agents

Project developed during the **Rocketseat** event to create a room management API with intelligent agent integration.

## 🚀 Technologies Used

- **Node.js** - JavaScript runtime
- **TypeScript** - Static typing
- **Fastify** - Fast and efficient web framework
- **Drizzle ORM** - Modern ORM for TypeScript
- **PostgreSQL** - Relational database
- **pgvector** - Extension for vector operations
- **Google Gemini AI** - AI for audio transcription and embedding generation
- **Zod** - Schema validation
- **Docker** - Containerization
- **Biome** - Code linter and formatter

## 🏢️ Architecture and Patterns

- **REST API** with Fastify
- **Type Safety** with TypeScript and Zod
- **Database First** with Drizzle ORM
- **Vector Database** with pgvector for embeddings
- **RAG (Retrieval-Augmented Generation)** for contextualized responses
- **Semantic Search** with vector similarity
- **AI Services** with Google Gemini for transcription, embeddings and response generation
- **Multipart Upload** for audio file processing
- **Environment Variables** for configuration
- **Containerization** with Docker Compose
- **Input validation** with fastify-type-provider-zod

## 📋 Prerequisites

- Node.js 18+ (with `--experimental-strip-types` support)
- Docker and Docker Compose
- pnpm (package manager)

## ⚙️ Configuration and Setup

### 1. Clone the repository
```bash
git clone https://github.com/victorbadaro/letmeask-agents-server
cd letmeask-agents-server
```

### 2. Install dependencies
```bash
pnpm install
```

### 3. Configure environment variables
Create a `.env` file in the project root:

```env
PORT=3333
DATABASE_URL=postgresql://docker:docker@localhost:5432/agents
GEMINI_API_KEY=your_gemini_api_key
```

> **Note:** To get the Gemini API key, visit [Google AI Studio](https://aistudio.google.com/) and generate a new API key.

### 4. Start the database
```bash
docker compose up -d
```

### 5. Run database migrations
```bash
pnpm db:migrate
```

### 6. (Optional) Populate the database with initial data
```bash
pnpm db:seed
```

### 7. Start the server
```bash
# Development mode
pnpm dev

# Production mode
pnpm start
```

## 🔧 Available Scripts

- `pnpm dev` - Starts the server in development mode
- `pnpm start` - Starts the server in production mode
- `pnpm db:studio` - Opens Drizzle Studio to view and edit the database
- `pnpm db:generate` - Generates database migration files
- `pnpm db:migrate` - Runs database migrations
- `pnpm db:seed` - Populates the database with initial data

## 📊 Endpoints

- `GET /health` - Application health check
- `GET /rooms` - Lists all available rooms
- `POST /rooms` - Creates a new room
- `GET /rooms/:roomId/questions` - Lists all questions from a specific room
- `POST /rooms/:roomId/questions` - Creates a new question and automatically generates an answer using RAG
- `POST /rooms/:roomId/audio` - Uploads audio for transcription and embedding generation

## 🤖 AI Features

The project integrates Google Gemini AI to provide advanced audio and text processing capabilities:

### Audio Transcription
- **Model used:** `gemini-2.5-flash`
- **Functionality:** Converts audio files to Brazilian Portuguese text
- **Features:** Accurate and natural transcription with proper punctuation

### Embedding Generation
- **Model used:** `text-embedding-004`
- **Functionality:** Converts text to 768-dimensional vectors
- **Purpose:** Enables semantic search and content similarity
- **Storage:** Uses pgvector for efficient vector operations

### Semantic Search and RAG
- **Cosine Similarity:** Uses pgvector's `<=>` operation for distance calculation
- **Similarity Threshold:** 0.7 (scores above indicate high relevance)
- **Result Limit:** Up to 3 most relevant chunks per query
- **Intelligent Context:** Combines multiple transcriptions for more accurate responses

### Contextualized Response Generation
- **Model used:** `gemini-2.5-flash`
- **Approach:** RAG (Retrieval-Augmented Generation)
- **Process:** Searches relevant content and generates context-based responses
- **Features:** Responses in Portuguese, educational tone, citations from "class content"

### Audio Processing Flow
1. **Upload:** Audio file is sent via multipart/form-data
2. **Transcription:** Gemini AI converts audio to text
3. **Embeddings:** Text is transformed into embedding vectors
4. **Storage:** Transcription and embeddings are saved to the database
5. **Response:** Returns the created chunk ID for future reference

### Question Processing Flow
1. **Question:** User sends a question via API
2. **Question Embedding:** Question is converted to embedding vectors
3. **Semantic Search:** System searches for similar audio chunks (similarity > 0.7)
4. **Context Selection:** Up to 3 most relevant transcriptions are selected
5. **Response Generation:** Gemini AI generates response based on found context
6. **Storage:** Question and answer are saved to the database
7. **Response:** Returns the question ID and generated answer

## 🗄️ Database

The project uses PostgreSQL with the pgvector extension for vector operations. The main structure includes:

- **rooms** - Table to store room information
  - `id` (UUID) - Unique identifier
  - `name` (TEXT) - Room name
  - `description` (TEXT) - Room description
  - `created_at` (TIMESTAMP) - Creation date

- **questions** - Table to store room questions
  - `id` (UUID) - Unique identifier
  - `roomId` (UUID) - Room reference (FK to rooms.id)
  - `question` (TEXT) - Question text
  - `answer` (TEXT) - Question answer (optional)
  - `created_at` (TIMESTAMP) - Creation date

- **audio_chunks** - Table to store audio transcriptions and embeddings
  - `id` (UUID) - Unique identifier
  - `room_id` (UUID) - Room reference (FK to rooms.id)
  - `transcription` (TEXT) - Audio transcription in text
  - `embeddings` (VECTOR(768)) - Embedding vectors generated by AI
  - `created_at` (TIMESTAMP) - Creation date

## 📝 Development

The project is configured to use Node.js's new experimental TypeScript support (`--experimental-strip-types`), eliminating the need for transpilation during development.

---

Developed with ❤️ during the Rocketseat NLW event
