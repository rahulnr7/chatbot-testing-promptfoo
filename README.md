# Chatbot / Conversational AI Testing Demo

A small Promptfoo-based test suite demonstrating LLM/chatbot output evaluation,
multi-turn context testing, adversarial/red-team testing, and API-level testing
against conversational AI models — built to speak to JD requirements around
"chatbot or conversational AI testing" and "API testing" that existing
test-automation background (Puppeteer/Jest/UI automation) doesn't directly cover.

## What this practices

- Writing test cases (prompts + expected properties) for a conversational AI system,
  distinct from UI test automation.
- API testing against LLM providers (Google Gemini, free tier) — auth, request/
  response shape, structured assertions on non-deterministic output.
- Promptfoo: config-driven eval suites, assertion types (`contains`, `llm-rubric`,
  `javascript`, etc.), model-graded ("LLM-as-judge") assertions, running evals from
  the CLI, reading the HTML/web results report.
- Multi-turn conversation testing — verifying the chatbot retains context (names,
  order numbers, stated preferences) across turns, and doesn't get confused by a
  user contradicting earlier information.
- Comparing multiple models side by side against the same test suite.
- Adversarial / red-team testing — automatically generating attack prompts
  (jailbreaks, prompt injection, PII exfiltration, hallucination, unauthorized
  contractual commitments, etc.) and grading whether the bot holds up.
- CI integration — running evals automatically on push, and redteam scans on
  demand via a manually-triggered GitHub Actions workflow.

## Project structure

| File | Purpose |
| --- | --- |
| `prompts/support-bot.txt` | Single-turn support bot system prompt |
| `prompts/support-bot-multiturn.txt` | Support bot prompt with conversation history |
| `promptfooconfig.yaml` | Main eval: FAQ handling, tone, hallucination avoidance, prompt-injection resistance, conciseness — run against two Gemini models side by side |
| `promptfooconfig.multiturn.yaml` | Multi-turn eval: context retention across conversation turns |
| `promptfooconfig.redteam.yaml` | Redteam source config: attack plugins, strategies, target purpose |
| `promptfooconfig.redteam.generated.yaml` | Generated adversarial test cases (produced by `redteam:generate`) |
| `.github/workflows/eval.yml` | Runs both evals on every push; uploads HTML reports as artifacts |
| `.github/workflows/redteam.yml` | Manually-triggered redteam scan with configurable `temperature`/`num_tests` inputs |

## Setup

1. Copy `.env.example` to `.env` and fill in a real Gemini API key from
   [Google AI Studio](https://aistudio.google.com).
2. `npm install`

## Running evals

- `npm run eval` — single-turn eval, graded by `google:gemini-3-flash-preview`,
  run against `gemini-flash-lite-latest` and `gemini-3.1-flash-lite`.
- `npm run eval:multiturn` — multi-turn context-retention eval.
- `npm run view` — browse results in the local web UI.

## Running a redteam scan

1. `npm run redteam:generate` — synthesizes adversarial test cases from
   `promptfooconfig.redteam.yaml` (requires one-time email verification with
   Promptfoo's free remote generation service, or pass `--provider` to use your
   own model instead — see the CI workflow for an example).
2. `npm run redteam:eval` — runs the generated attacks against the target and
   writes `redteam-report.html`. Throttled (`-j 2 --delay 1000`) to stay under
   free-tier rate limits.

In CI, trigger **Promptfoo Redteam Eval** manually from the Actions tab — it
generates and evaluates in one run, with `temperature` and `num_tests` as
configurable inputs, and uploads the report + generated attack corpus as a
build artifact.

## Status

Scaffolding in progress — see commit history for what's been built so far.
