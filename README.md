# Chef de Gare FR

A bilingual (French/English) AI assistant for SNCF's open TGV punctuality data. It answers questions with a source and a definition for every number, says "I don't know" when the data can't answer, and refuses unsafe requests. It can also run tasks (charts, sourced reports, alerts), and a human approves any action with side effects.

> **Status:** Week 0, design phase. No application code yet.

## What makes it different

- **Sourced answers.** Numbers come only from validated SQL; definitions come from SNCF/AQST method notes.
- **Measured, not claimed.** Three designs (simple RAG, agentic RAG, text-to-SQL agent) are compared on one eval set.
- **Secure by design.** Prompt-injection defences, a SELECT-only SQL guard, PII masking, and human approval for actions, all tested in CI.
- **Sovereign option.** Runs fully on a local open-weights Mistral model, or on the Mistral API, switched by one setting.

## Planned stack

FastAPI · LangGraph · PostgreSQL + pgvector · Redis + Arq · Next.js · Ollama (Mistral) · Langfuse · Docker Compose · GitHub Actions

## Repository layout

```text
docs/adr/        Architecture Decision Records
```

The full monorepo layout (`apps/api`, `apps/web`, `evals/`, `infra/`) arrives in Week 1.

## Data and licences

- **Code:** [Apache License 2.0](LICENSE).
- **Data:** SNCF punctuality datasets from [data.gouv.fr](https://www.data.gouv.fr/fr/datasets/regularite-mensuelle-tgv-par-liaisons/), under the Open Database Licence (ODbL). Attribution: © SNCF. Data is downloaded by script, not committed.
