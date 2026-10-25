# DeepSeek harness setup notes (draft)

These notes cover connecting the DeepSeek harness (`dsh`) to OpenRouter and to a local model server, and giving subagents their own model. They are assembled from the harness's published npm package READMEs and third-party write-ups, and **have not yet been tested end to end**. The harness is in developer preview, so check every item below against the version you install.

Anything marked **TODO(verify)** needs a test run before the workshop.

## Run the harness

```bash
npx @deepseek-ai/dsh web
```

This starts the web UI at `http://127.0.0.1:3080`. Node.js is required. The desktop app is available for macOS (Apple silicon) and Windows (64-bit).

## Add OpenRouter as a provider

The simplest route is the web UI: **Settings → Models → Add custom provider**. Use these values:

| Field | Value |
|---|---|
| Base URL | `https://openrouter.ai/api/v1` |
| API protocol | OpenAI-compatible chat completions (`openai-completions`) |
| API key | Your OpenRouter key. Do not commit it or share it |
| Models | The DeepSeek V4 Pro and DeepSeek V4 Flash IDs from the [OpenRouter model list](https://openrouter.ai/models) |

**TODO(verify):** the exact OpenRouter model IDs for DeepSeek V4 Pro and V4 Flash, and the reasoning-effort setting for each.

If you prefer a config file, the provider lives under the `llm-pi-ai` plugin. Sources disagree on the file name (`~/.dsh/settings.yaml` in one, a `cordis.patch.yml` patch file in another), so confirm with `dsh headless --dump-config`.

```yaml
llm-pi-ai:
  providers:
    openrouter:
      displayName: OpenRouter
      apiKeyEnv: OPENROUTER_API_KEY
      api: openai-completions
      baseURL: https://openrouter.ai/api/v1
      models:
        - id: <deepseek-v4-pro model id>
        - id: <deepseek-v4-flash model id>
```

Set the key in your shell instead of the file:

```bash
export OPENROUTER_API_KEY=your-key-here        # macOS / Linux
$env:OPENROUTER_API_KEY = "your-key-here"      # Windows PowerShell
```

## Add a local model server

A local server is added the same way as OpenRouter: a custom provider that points at an OpenAI-compatible endpoint whose URL ends in `/v1`.

| Runtime | Default base URL |
|---|---|
| LM Studio | `http://127.0.0.1:1234/v1` |
| Ollama | `http://127.0.0.1:11434/v1` |
| llama.cpp (`llama-server`) | `http://127.0.0.1:8080/v1` |

Things that bite:

- **Tool calling.** The harness sends tool schemas on every request, so the model and server must support tool calls. llama.cpp needs `--jinja`.
- **Context window.** Declare a `contextWindow` for each local model, and make the server's context length match it. Otherwise the harness assumes a large default and the server may silently truncate the prompt.
- **API key variable.** Even a local route needs a placeholder value in the variable named by `apiKeyEnv`.
- **Speed.** CPU-only inference is very slow for agent work. Use a GPU or Apple silicon.

**TODO(verify):** pick the local runtime. LM Studio is not covered in the sources found; Ollama, llama.cpp and vLLM are.

## Subagents with their own model

The harness has a subagent delegation tool. Two documented ways to control which model a subagent uses:

1. **Let the main agent choose.** In the web UI open **Plugins → Subagent**. The **Model selection** section sets which model routes agents may pick for their subagents. Enable it and select the routes you want, for example the DeepSeek V4 Flash route and the local route.
2. **Fix the model per tool.** The delegation tool takes `agentOptions` with a child `model`. The child policy is fixed per tool instance, so a different model means a second, distinctly named tool instance.

The same page sets the maximum delegation depth and how many subagents may run in parallel.

**TODO(verify):** which of the two works better for the workshop, and the exact wording that makes the main agent pick the right model.

## Project instructions

The harness repository tells agents to follow `AGENTS.md`. **TODO(verify):** whether `dsh` reads an `AGENTS.md` in your project the way Claude Code reads `CLAUDE.md`.

## Sources

- [DeepSeek Harness](https://www.deepseek.com/en/harness/)
- [deepseek-ai/deepseek-harness](https://github.com/deepseek-ai/deepseek-harness)
- npm packages `@deepseek-ai/dsh-tool-subagent` and `@deepseek-ai/dsh-client-ui-settings-subagent`
- [Custom provider setup (APIYI docs)](https://docs.apiyi.com/en/scenarios/agent/deepseek-harness.md)
- [DeepSeek Harness with a local model](https://computingforgeeks.com/deepseek-harness-local-model/)
