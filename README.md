# Agentic RAG System

A self-correcting Retrieval-Augmented Generation (RAG) system built with LangGraph that iteratively improves its answers through document relevance grading and hallucination detection.

## Architecture

The system uses a cyclic graph with the following nodes:

- **retrieve** — fetches relevant documents from the vector store
- **grade_documents** — filters retrieved documents by relevance to the question
- **rewrite_query** — rewrites the question for better retrieval if documents are insufficient
- **cannot_answer** — returns a failure response after max retrieval attempts
- **generate** — generates an answer from graded documents
- **grade_hallucination** — verifies the answer is grounded in the documents

## Self-Correction Loops

**Retrieval loop** — if fewer than 2 relevant documents are found, the query is rewritten and retrieval is retried. Max 3 attempts before giving up.

**Hallucination loop** — if the generation is not grounded in documents, it regenerates. Max 3 attempts before returning best generation.

## Stack

- LangGraph — agent orchestration
- LangChain — LLM and retriever abstractions
- OpenAI — LLM and embeddings
- FAISS — vector store
- Python dotenv — environment management

## Setup

```bash
pip install langgraph langchain langchain-openai langchain-community faiss-cpu python-dotenv
```

Add your API keys to a `.env` file:
```
OPENAI_API_KEY=your_key
```
