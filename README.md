<div align="center">

# @omadia/plugin-llm-minimax

### Run your omadia agents on MiniMax, including the 1M-context MiniMax-M3.

A signed omadia plugin that adds MiniMax as a provider you pick on the admin Providers page. Declarative: no runtime code, the model catalog ships in the manifest.

[![License: MIT](https://img.shields.io/badge/License-MIT-black.svg)](LICENSE)
[![Built for omadia](https://img.shields.io/badge/built%20for-omadia-2496ED.svg)](https://github.com/byte5ai/omadia)
[![TypeScript](https://img.shields.io/badge/built%20with-TypeScript-3178C6.svg?logo=typescript&logoColor=white)](https://www.typescriptlang.org/)

[**Main repo**](https://github.com/byte5ai/omadia) · [**Website**](https://omadia.ai) · [**Plugin hub**](https://hub.omadia.ai) · [**Models**](#models) · [**Install**](#install)

🇩🇪 Diese Anleitung gibt es auch [auf Deutsch](./README.de.md).

</div>

---

omadia is a self-hostable agentic OS: compose multi-agent teams from signed plugins, run them on your own machine, and get an auditable trail for every action. This plugin makes MiniMax one of the LLM providers those agents can run on. Main repo: [byte5ai/omadia](https://github.com/byte5ai/omadia).

## Models

| Model | Class | Context |
| --- | --- | --- |
| MiniMax M3 | frontier | 1,048,576 |
| MiniMax M2.5 | balanced | 204,800 |
| MiniMax M2.5 (high-speed) | fast | 204,800 |

Agents ask for a class (`fast`, `balanced`, `frontier`). omadia maps the class to the model, so an agent never hard-codes one.

## How it works in omadia

A declarative provider plugin, so it ships no runtime provider code. The omadia kernel reads the `llm_provider` block from `manifest.yaml` when the plugin loads, before any agent activates, and registers it in the provider catalog. MiniMax speaks an OpenAI-compatible wire format, so omadia drives it through its built-in OpenAI-compatible adapter. The manifest declares MiniMax's wire quirks (the modern `max_completion_tokens` field, the reasoning split) so requests stay valid.

## Install

1. Install from the [plugin hub](https://hub.omadia.ai) in the omadia admin UI (Store, Upload), or drop the built ZIP in directly.
2. On the admin Providers page, paste your MiniMax API key. omadia stores it encrypted in the vault under `provider:minimax/api_key`.
3. Assign MiniMax and a model to an agent: the orchestrator, a sub-agent, or the verifier.

## Configuration

| Setup field | Required | Default | Notes |
| --- | :---: | --- | --- |
| `minimax_base_url` | no | `https://api.minimax.io/v1` | Operators in China use `https://api.minimaxi.com/v1`. |

The key is set centrally on the Providers page, not per plugin, so the orchestrator reads it from the shared vault scope.

## Build from source

```bash
npm install
npm run build   # tsc, emits dist/
npm test        # validates manifest.yaml against core's invariants
```

`@omadia/plugin-api` and `@omadia/llm-provider` are provided by the omadia host at runtime (optional peer deps). Link them from a local omadia checkout to build. See [byte5ai/omadia](https://github.com/byte5ai/omadia) for the layout.

## License

[MIT](LICENSE), byte5 GmbH