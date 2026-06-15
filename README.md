# @omadia/plugin-llm-minimax

Adds MiniMax models (flagship MiniMax-M3, 1M-token context) as an LLM you can assign to any agent in omadia.

omadia is a self-hostable agentic OS: you build, run, and audit multi-agent AI teams from signed plugins, and you bring your own LLM key. Main repo: [byte5ai/omadia](https://github.com/byte5ai/omadia). This plugin makes MiniMax one of the providers an operator can pick on the admin Providers page.

## Models

| Model | Class | Context |
|-------|-------|--------:|
| MiniMax M3 | frontier | 1,048,576 |
| MiniMax M2.5 | balanced | 204,800 |
| MiniMax M2.5 (high-speed) | fast | 204,800 |

Agents request a class (`fast` / `balanced` / `frontier`). omadia resolves the class to the concrete model.

## How it works in omadia

This is a declarative provider, so it ships no runtime provider code. The `llm_provider` block in `manifest.yaml` (id, base URL, models, API quirks) is read by the omadia kernel when the plugin loads, before any agent activates, and registered into the kernel's provider catalog. MiniMax uses the OpenAI-compatible Chat Completions API, so omadia's built-in OpenAI-compatible adapter drives the calls. The manifest declares the MiniMax wire quirks (the modern `max_completion_tokens` field, reasoning split) so requests stay valid.

## Install

Install from the omadia hub at [hub.omadia.ai](https://hub.omadia.ai) (omadia admin, plugins, install), or upload the built ZIP directly.

After install:

1. On the admin Providers page, paste your MiniMax API key. It is stored encrypted under `provider:minimax/api_key`.
2. Assign MiniMax and a model to an agent.

## Configuration

| Setup field | Required | Default | Notes |
|-------------|:--------:|---------|-------|
| `minimax_base_url` | no | `https://api.minimax.io/v1` | International endpoint. Operators in China use `https://api.minimaxi.com/v1`. |

The API key is set centrally on the Providers page, not as a per-plugin secret.

## Build from source

```bash
npm install
npm run build   # tsc, emits dist/
npm test        # validates manifest.yaml against core's invariants
```

The plugin compiles against omadia workspace packages (`@omadia/plugin-api`, `@omadia/llm-provider`), declared as optional peer deps. Link them from a local omadia checkout before building. See [byte5ai/omadia](https://github.com/byte5ai/omadia).

## License

MIT, byte5 GmbH
