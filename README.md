# @omadia/plugin-llm-minimax

Adds **MiniMax** as an admin-selectable LLM provider for [omadia](https://github.com/byte5ai/omadia). MiniMax exposes an OpenAI-compatible Chat Completions API, so this is a **declarative provider plugin**: the provider, its models and its API quirks are described in `manifest.yaml` and registered by the omadia kernel at load time — there is no runtime provider code to maintain.

> Requires omadia core with the LLM-provider-plugin seam (the manifest-driven
> `LlmProviderCatalog`). Older cores ignore the `llm_provider` manifest block.

## Models

| Model | Class | Context | Max output | Vision |
|-------|-------|--------:|-----------:|:------:|
| `MiniMax-M3` (flagship) | frontier | 1,048,576 | 131,072 | yes |
| `MiniMax-M2.5` | balanced | 204,800 | 65,536 | no |
| `MiniMax-M2.5-highspeed` | fast | 204,800 | 65,536 | no |

## Install

1. Build the plugin: `npm install && npm run build` (produces `dist/plugin.js`).
2. Package `manifest.yaml` + `dist/` and install it into omadia (admin → install plugin, or via the registry).
3. On the admin **Providers** page, set the MiniMax API key — it is stored in the vault under `provider:minimax/api_key` (same mechanism as the built-in OpenAI provider).
4. Assign MiniMax (and a model) to a plugin such as the orchestrator on the Providers page.

## Configuration

| Setup field | Required | Default | Notes |
|-------------|:--------:|---------|-------|
| `minimax_base_url` | no | `https://api.minimax.io/v1` | Use `https://api.minimaxi.com/v1` for the China platform. |

The API key is **not** a per-plugin setup secret — it is set centrally on the Providers page so the orchestrator (which reads the key from its own vault scope) can see it.

## Provider quirks handled

MiniMax is OpenAI-compatible but differs in a few ways; the plugin declares these so core's OpenAI-compatible adapter handles them:

- `max_completion_tokens` instead of the deprecated `max_tokens`.
- `tool_choice` / `parallel_tool_calls` are dropped (not in MiniMax's schema; M3 decides tool use autonomously).
- `base_resp.status_code` is checked even on HTTP 200.
- `reasoning_split: true` separates interleaved reasoning from the final text.

## Development

```bash
npm install
npm run typecheck   # tsc --noEmit (needs the omadia core contract built)
npm test            # validates the manifest + model invariants
npm run build
```
