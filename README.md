# AI Agent with LangChain + RAG over GitHub Issues

A retrieval-augmented agent that indexes a GitHub repository's issues into a vector store and answers natural-language questions about them, using LangChain's tool-calling agent framework.

## How it works

- `github.py` pulls issues from a GitHub repository via the REST API and converts them into LangChain `Document` objects (title, body, author, comments, labels).
- `main.py` embeds those documents with OpenAI embeddings and stores them in [AstraDB](https://www.datastax.com/products/datastax-astra) as a vector store, then wraps the store in a retriever tool.
- The agent (`create_tool_calling_agent`, OpenAI functions-style) is given two tools: the issue retriever (`github_search`) and a `note_tool` that appends notes to a local file. It answers questions about the indexed issues from the command line.

## Stack

Python, LangChain, LangChain-OpenAI, LangChain-AstraDB, OpenAI API, AstraDB (vector store).

## Setup

```bash
pip install -r requirements.txt
cp .env.example .env   # fill in your own keys — never commit this file
python main.py
```

Required environment variables (see `.env.example`):

| Variable | Purpose |
|---|---|
| `GITHUB_TOKEN` | Authenticates GitHub API requests when fetching issues |
| `ASTRA_DB_API_ENDPOINT` | AstraDB vector store endpoint |
| `ASTRA_DB_APPLICATION_TOKEN` | AstraDB auth token |
| `ASTRA_DB_KEYSPACE` | Optional AstraDB keyspace |
| `OPENAI_API_KEY` | OpenAI API key for embeddings + chat model |

On first run you'll be prompted whether to (re)index issues into the vector store before you can start asking questions.

## Note

An earlier version of this repo had a `.env` file committed with real API keys. That file has been removed from the entire git history and all associated credentials were rotated. Never commit `.env` — use `.env.example` as the template.
