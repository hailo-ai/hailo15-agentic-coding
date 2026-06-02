# hailo15-agentic-coding

The Claude agentic layer for [`hailo-media-library`](https://github.com/hailo-ai/hailo-media-library).

This repo holds the **most up-to-date skills and agents** plus all other Claude-related configuration.
This is an AI-assisted layer that allows customers to explore, modify and build apps without requiring deep knowledge of the codebase:

- **Explore** Hailo's documentation at any stage
- **Build & modify** Media Library applications
- **Debug & evaluate** running deployments on the H15 board

Just describe what you want in plain English. Claude scans the skills on every
prompt and runs the matching one; you can also call a skill explicitly with
`/skill-name`.

```
> explain this pipeline, add a FHD (1920×1080@30FPS) stream, swap detection to
  YOLOv8n and deploy it to my connected h15l
  → /explain-pipeline → /add-stream → /get-model → /swap-model → /cross-compile → /deploy
```

## What's in here

| | |
|---|---|
| `skills/` | One-command tools that turn multi-page procedures into a single prompt - connect, explore, modify, build, deploy, debug. |
| `agents/` | Scoped experts (`doc-explorer`, `pipeline-expert`, `apps-expert`) that skills call to return summarized context to the main session. |

### Skills

| Phase | Skill | What it does |
|---|---|---|
| Connection | `/connect` | Configure IP + SSH keys and connect to the H15. |
| Explore | `/explain-pipeline` | Walk through what an app does - sources, AI stages, and the configs that feed it. |
| Modify | `/add-stream` | Add another output stream (resolution, encoder, port). |
| Modify | `/add-overlay` | Add, modify, or remove AI overlays (bboxes, masks, landmarks) - on-board burn-in or host-side. |
| Modify | `/swap-model` | Replace the AI model in an app with a different HEF. |
| Modify | `/edit-pipeline` | Modify the stage topology of an existing app. |
| Modify | `/get-model` | Propose and download a HEF from the public Hailo Model Zoo. |
| Build | `/cross-compile` | Cross-compile an app for the H15 using the Yocto SDK. |
| Deploy | `/deploy` | Push artifacts (binary, configs, HEFs) to the board and verify it runs. |
| Run | `/run-app` | Run an app on the H15 and display it |
| Debug | `/board-status` | Snapshot board health - temp, power, CPU/DRAM/NN-core/DSP utilization. Read-only. |
| Maintenance | `/update-claude-beta` | Pull the latest beta skills, agents, and config from the agentic-coding repo into your checkout. |

### Agents

| Agent | Role |
|---|---|
| `doc-explorer` | Reads the official Hailo PDF user guides and returns concise excerpts with page citations. |
| `pipeline-expert` | Owns the Media Library pipeline architecture. |
| `apps-expert` | Knows the reference apps and picks the closest one to copy/modify for a task. |

## Prerequisites

- [Claude Code](https://claude.com/claude-code) installed and authenticated.
- This repo's contents available under `.claude/` of your `hailo-media-library`
  checkout. Run `claude` from the repo root - skills and agents load automatically.
- An H15 SBC reachable over ethernet.
- The Yocto SDK on the host (only for `/cross-compile` and `/deploy`).

## ⚠️ Disclaimer

This tool is built on top of **Claude**, an external Large Language Model
provided by Anthropic. LLMs are **non-deterministic** - the same prompt can
produce different output, and that output may be incomplete, inaccurate, or wrong.

It is **not intended for production use**. Always review what it
proposes - generated code, configs, model choices, and commands run on your
board/host - before relying on it. Hailo provides this tooling **as-is, with no
warranty**, and takes **no responsibility** for any output, action, or outcome
resulting from its use.
