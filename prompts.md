# Inference at the Edge of Your IDE: prompts

Every prompt used in the workshop, with a note on when to run it. Keep this file open as you follow along.

We build the same app, Prompt Pantry, in four setups so you can compare what each one gives you and what it asks of you. Prompts marked **TODO(verify)** depend on DeepSeek harness behavior that has not been tested yet (see [harness/README.md](harness/README.md)).

> **Reset between runs.** After Stage 0, commit your project. Before each new run, discard the previous attempt with `git reset --hard && git clean -fd` so every setup starts from the same place. Keep your specs committed.

## Stage 0: project setup

Create a new folder for your project, copy `starter/specs/` into it as `specs/`, open it in your editor and start Claude Code in the terminal. Then run:

```
Read @specs/tech-stack.md. Scaffold the project in the current directory: a Vite React TypeScript app with React Router, Vitest, React Testing Library and jest-dom. Install the dependencies, remove the template demo content (keep index.html and the config files), and make sure `npm test` runs with no tests failing. Do not build any pages yet.
```

Commit the result.

## Stage 1: Claude Code, all cloud

### 1a. One agent, one task at a time

```
Look in @specs/mission.md and @specs/tech-stack.md for project details.
Look at the @specs/roadmap.md and:
- Implement each task one at a time from Phase 1, Phase 2 and Phase 3.

Do not fix anything after the subagent. Do a review and provide a list of critical mistakes.
```

### 1b. A fresh subagent per phase

```
Look in @specs/mission.md and @specs/tech-stack.md for project details.
Look at the @specs/roadmap.md and:
- Implement Phase 1 using a **new spawned** @general subagent.
- Implement Phase 2 using a **new spawned** @general subagent.
- Implement Phase 3 using a **new spawned** @general subagent.

Do not fix anything after the subagent. Do a review and provide a list of critical mistakes.
```

### 1c. A cheaper model for the subagents

```
Look in @specs/mission.md and @specs/tech-stack.md for project details.
Look at the @specs/roadmap.md and:
- Implement Phase 1 using a **new spawned** Haiku subagent.
- Implement Phase 2 using a **new spawned** Haiku subagent.
- Implement Phase 3 using a **new spawned** Haiku subagent.

Do not fix anything after the subagent. Do a review and provide a list of critical mistakes.
```

## Stage 2: DeepSeek harness and OpenRouter

### 2a. Connect OpenRouter

Follow [harness/README.md](harness/README.md) to add OpenRouter as a custom provider with only the two models used in the workshop: DeepSeek V4 Pro and DeepSeek V4 Flash. Create your OpenRouter API key first, and do not share it with anyone.

### 2b. One agent, one task at a time

Select DeepSeek V4 Pro for the main agent, then run the same prompt as 1a:

```
Look in @specs/mission.md and @specs/tech-stack.md for project details.
Look at the @specs/roadmap.md and:
- Implement each task one at a time from Phase 1, Phase 2 and Phase 3.

Do not fix anything after the subagent. Do a review and provide a list of critical mistakes.
```

### 2c. Subagents on a cheaper model

**TODO(verify):** how the harness picks the model for a subagent. The prompt below assumes model selection is enabled for the DeepSeek V4 Flash route.

```
Look in @specs/mission.md and @specs/tech-stack.md for project details.
Look at the @specs/roadmap.md and:
- Implement Phase 1 using a **new spawned** subagent running the DeepSeek V4 Flash model.
- Implement Phase 2 using a **new spawned** subagent running the DeepSeek V4 Flash model.
- Implement Phase 3 using a **new spawned** subagent running the DeepSeek V4 Flash model.

Do not fix anything after the subagent. Do a review and provide a list of critical mistakes.
```

## Stage 3: hybrid, a local subagent

### 3a. Connect a local model

Start your local runtime, load a small model that supports tool calls, and add it to the harness as a custom provider. See "Add a local model server" in [harness/README.md](harness/README.md). Check the base URL and model ID in your runtime and swap in yours.

### 3b. Cloud main agent, local implementer

Keep DeepSeek V4 Flash on OpenRouter as the main agent and make the local model the only route available to subagents. Local models do best with small, well-bounded jobs, so this prompt runs **one phase per run**. Send it three times: as written, then with "Run only Phase 1" changed to "Run only Phase 2", then to "Run only Phase 3".

**TODO(verify):** the exact tool names in the harness. The prompt asks for whole-file writes because small local models often fail at partial edits.

```
Read @specs/mission.md, @specs/tech-stack.md, and @specs/roadmap.md. Run only Phase 1.

Read the phase's target files, then delegate this phase exactly once to a fresh subagent running the local model, with a compact, self-contained packet: repo-relative writable paths, complete required final state, and exact behavior and tests to preserve. Do not refer the child to specification files or include file contents. Do not provide complete files, complete functions, code blocks, pseudocode, or line-by-line implementations. Include exact literals, imports, component props, route paths, test semantics, and preserved behavior when they are contract requirements.

Include this exact instruction: "For every writable file, write its complete final content; never make partial edits."

Tell the child this is its final tool call and it must stop whether it passes or fails: `npm test`.

After it returns, only read the reported changed files. Do not run shell commands or edit anything yourself. Report the exact validation result, files changed, and only critical contract violations: missing required strings, components, props, routes, imports, or unexpected files.
```

If the review reports issues, tell the main agent to send the fixes back to the subagent, for example: `Have the subagent fix <the reported issue>`.

## Stage 4: fully local

Add a larger local model (one that handles tool calls reliably) as a second local route, then make it the **main agent** as well. Reuse the Stage 3b prompt, phase by phase, exactly as before.

Expect some potholes. They are part of the comparison.
