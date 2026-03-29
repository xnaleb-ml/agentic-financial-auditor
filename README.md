# agentic-financial-auditor
A dual-architecture agentic RAG pipeline for auditing SEC filings. Version 1 features a FastAPI backend powered by cloud LLMs. Version 2 is fully localized, serving open-weight models via vLLM. Built with LangGraph and evaluated on FinanceBench.

## Project Structure

```text
C:.
└───agentic-financial-auditor
    │   .env
    │   .gitignore
    │   .python-version
    │   pyproject.toml
    │   README.md
    │   uv.lock
    │
    └───app
        ├───api
        │       routes.py
        │       __init__.py
        │
        ├───core
        │   │   graph.py
        │   │   nodes.py
        │   │   state.py
        │   │   __init__.py
        │   │
        │   └───prompts
        │           prompts.yaml
        │
        └───data
                ingest.py
                __init__.py
```

## Roadmap & To-Do List

### Phase 1: Cloud API Architecture (FastAPI + Managed LLMs)
**Foundation & Data Engineering**
- [x] Initialize project using `uv` with Python 3.13 and flat directory structure
- [x] Configure `.gitignore`, `pyproject.toml`, and environment templates
- [ ] Write Python script to fetch `PatronusAI/financebench` from Hugging Face
- [ ] Implement text chunking strategy 
- [ ] Choose local Vector Database
- [ ] Spin up local Vector Database instance (via Docker)
- [ ] Embed SEC filing chunks and load into VDB

**Agentic Core (LangGraph)**
- [ ] Define Pydantic schemas for the Graph State
- [ ] Build **Query Rewriter Node**: Optimize user queries for dense vector search
- [ ] Build **Retriever Node**: Execute vector similarity search on VDB
- [ ] Build **Factual Grader Node**: LLM-as-a-judge to verify retrieved context contains the exact financial metrics needed
- [ ] Build **Generator Node**: Synthesize final answer with strict citation constraints
- [ ] Wire edges, define conditional routing (loop back if context is insufficient), and compile graph

**API Layer & Evaluation**
- [ ] Wrap the compiled graph in a FastAPI `/chat` endpoint
- [ ] Write automated evaluation script to test graph outputs against FinanceBench golden answers
- [ ] Log baseline accuracy metrics (Hallucination rate, retrieval success rate)
- [ ] Implement simple API Key authentication for the FastAPI endpoints
- [ ] Integrate Agentic Observability (e.g., LangSmith or Langfuse) to trace graph execution and LLM latency

**Deployment & CI/CD**
- [ ] Write a `Dockerfile` for the FastAPI application
- [ ] Create a `docker-compose.yml` to orchestrate the API and VDB together
- [ ] Write **Terraform** scripts (`main.tf`, `variables.tf`) to provision cloud infrastructure (e.g., AWS EC2/ECS or GCP Cloud Run)
- [ ] Set up GitHub Actions CI/CD pipeline to run linting and FinanceBench evals automatically on Pull Requests
---

### Phase 2: Local Enterprise Architecture (vLLM)
**Air-gapped Deployment**
- [ ] Configure WSL2 / CUDA environment for local RTX 3090 Ti inference
- [ ] Pull and serve an open-weight instruct model (e.g., Qwen2.5-7B) via vLLM
- [ ] Update API configuration to point LangChain/LangGraph to the local vLLM OpenAI-compatible endpoint
- [ ] Swap cloud embedding model for a local HuggingFace embedding model (e.g., `bge-large-en-v1.5`)
- [ ] Re-run FinanceBench evaluation pipeline and benchmark accuracy/latency against Phase 1

---

### Icebox / Additional Tasks
*(Future enhancements, bug fixes, and optimization ideas discovered during development)*
- [ ] 
- [ ] 

## Quickstart  
## License