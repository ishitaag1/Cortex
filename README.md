# Cortex

A production-oriented, multi-tenant **agentic RAG SaaS**. Organizations upload documents, which are processed into a vector index and an entity graph. A LangGraph reasoning agent then answers questions over that knowledge, streaming responses token by token.

## Features

- **Multi-tenant isolation**: per-organization data separation using Clerk and Prisma
- **Hybrid retrieval**: semantic search in Qdrant combined with entity relationships in Neo4j
- **Agentic reasoning**: LangGraph agent that plans, retrieves, and synthesizes answers
- **Document pipeline**: background ingestion, chunking, embedding, and entity extraction via Inngest
- **Streaming responses**: token streaming over Server-Sent Events (SSE)
- **API access**: API key authentication with SHA-256 hashed keys
- **Rate limiting**: Upstash Redis
- **File storage**: Cloudinary
- **Model access**: OpenRouter

## Tech Stack

| Layer | Technology |
|---|---|
| Frontend / API | Next.js, Tailwind CSS 4 |
| Auth | Clerk |
| Database / ORM | PostgreSQL, Prisma |
| Vector store | Qdrant |
| Graph store | Neo4j |
| Agent framework | LangGraph, LangChain |
| Background jobs | Inngest |
| Rate limiting | Upstash Redis |
| Media storage | Cloudinary |
| LLM gateway | OpenRouter |

## Architecture

```
Upload → Cloudinary → Inngest job → chunk + embed → Qdrant
                                  └→ entity extraction → Neo4j
Query  → Clerk / API key auth → rate limit → LangGraph agent
         → retrieve (Qdrant + Neo4j) → LLM (OpenRouter) → SSE stream
```

## Getting Started

### Prerequisites
- Node.js 20+
- PostgreSQL, Qdrant, and Neo4j instances
- Accounts for Clerk, Cloudinary, OpenRouter, Upstash, and Inngest

### Setup

```bash
git clone https://github.com/ishitaag1/Cortex.git
cd Cortex
npm install
cp .env.example .env   # fill in your keys
npx prisma migrate dev
npm run dev
```

### Environment Variables

| Variable | Purpose |
|---|---|
| `DATABASE_URL` | PostgreSQL connection |
| `CLERK_SECRET_KEY` / `NEXT_PUBLIC_CLERK_PUBLISHABLE_KEY` | Authentication |
| `QDRANT_URL` / `QDRANT_API_KEY` | Vector store |
| `NEO4J_URI` / `NEO4J_USER` / `NEO4J_PASSWORD` | Graph store |
| `OPENROUTER_API_KEY` | LLM access |
| `CLOUDINARY_URL` | File storage |
| `UPSTASH_REDIS_REST_URL` / `UPSTASH_REDIS_REST_TOKEN` | Rate limiting |
| `INNGEST_EVENT_KEY` / `INNGEST_SIGNING_KEY` | Background jobs |

## Roadmap

- [ ] Usage analytics per tenant
- [ ] Evaluation suite for retrieval quality
- [ ] Additional document formats

