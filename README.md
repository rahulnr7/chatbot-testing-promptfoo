# Chatbot / Conversational AI Testing Demo

A small Promptfoo-based test suite demonstrating LLM/chatbot output evaluation and
API-level testing against a conversational AI model — built to speak to JD
requirements around "chatbot or conversational AI testing" and "API testing" that
existing test-automation background (Puppeteer/Jest/UI automation) doesn't directly
cover.

## What this practices

- Writing test cases (prompts + expected properties) for a conversational AI system,
  distinct from UI test automation.
- API testing against an LLM provider (Google Gemini, free tier) — auth, request/
  response shape, structured assertions on non-deterministic output.
- Promptfoo: config-driven eval suite (`promptfooconfig.yaml`), assertion types
  (contains, llm-rubric, etc.), running evals from the CLI, reading the results report.

## Setup

1. Copy `.env.example` to `.env` and fill in a real Gemini API key from
   [Google AI Studio](https://aistudio.google.com).
2. `npm install`
3. `npx promptfoo eval`
4. `npx promptfoo view` to browse results in the browser.

## Status

Scaffolding in progress — see commit history for what's been built so far.
