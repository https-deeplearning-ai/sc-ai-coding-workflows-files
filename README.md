# Inference at the Edge of Your IDE

Companion files for the workshop *Inference at the Edge of Your IDE: Agentic React Development from Cloud LLMs to On-Device Models*.

Across the workshop you build the same small React app, Prompt Pantry, in four different AI coding setups: Claude Code in the cloud, the DeepSeek harness with OpenRouter models, a hybrid with a local subagent, and fully local. Keeping the app the same lets you compare what each setup gives you and what it asks of you.

This repository is adapted from the files of the DeepLearning.AI course *AI Coding Workflows: From Cloud to Local* ([original repository](https://github.com/https-deeplearning-ai/sc-ai-coding-workflows-files)). The app, specs, prompts and tooling here are changed for this workshop.

## How to use this repo

You do not work inside this repo. Create a project folder of your own, copy `starter/specs/` into it as `specs/`, and follow [prompts.md](prompts.md) from Stage 0.

| Path | Purpose |
|---|---|
| `starter/specs/mission.md` | Describes the app, its purpose and the experience it should provide |
| `starter/specs/tech-stack.md` | Records the required technologies and constraints |
| `starter/specs/roadmap.md` | Breaks the app into three phases that coding agents can plan and implement |
| `prompts.md` | Every prompt used in the workshop, with notes on when to run each one |
| `harness/README.md` | Draft notes for connecting the DeepSeek harness to OpenRouter and local models, and for subagent models |
| `.gitignore` | Excludes `node_modules`, build output, local env files and editor files |

## Prerequisites

- [Git](https://git-scm.com/downloads/)
- [Node.js](https://nodejs.org/) (current LTS)
- An editor: [Orca](https://www.onorca.dev/) or [VS Code](https://code.visualstudio.com/download)
- [Claude Code](https://code.claude.com/docs/en/getting-started)
- The [DeepSeek harness](https://www.deepseek.com/en/harness/)
- An [OpenRouter](https://openrouter.ai/docs/quickstart) account and API key
- Later, for the local stages: a local model runtime (to be decided)
