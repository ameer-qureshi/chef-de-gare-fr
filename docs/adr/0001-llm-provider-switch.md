# ADR 0001: Switch between a local and a hosted LLM with one setting

- **Status:** Accepted
- **Date:** 2026-10-18

## Context

The project must run fully locally for its sovereignty story (GDPR, EU AI Act). The development laptop has an Intel Core i7 (11th gen), 16 GB RAM and no dedicated GPU, so a 7B model runs on CPU only and answers are slow. Development needs fast feedback, and the evals need a fair local-vs-hosted comparison.

## Decision

All model calls go through one LLM gateway module. The `LLM_PROVIDER` environment variable selects the backend:

- `ollama`: local open-weights Mistral 7B through Ollama, run natively on Windows (not in Docker).
- `mistral_api`: hosted Mistral API on the free Experiment plan.

Day-to-day development uses the API. Final evals and the "sovereign mode" demo use the local model.

## Alternatives considered

| Option | Why not |
| --- | --- |
| Local model only | Too slow on this CPU for daily development |
| Hosted API only | Loses the sovereignty story; Experiment-plan requests may be used for training |

## Consequences

- Application code never imports a provider SDK directly; it only calls the gateway.
- PII is masked before any call to the hosted API, not only before logging.
- The eval harness runs each configuration under both providers.
- Local inference speed must be measured in Week 1 (Ollama `--verbose` eval rate).
