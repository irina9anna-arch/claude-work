# Phase 0: Repository verification

Checked 2026-10-31. Each repo was cloned shallow (`git clone --depth 1`), and its README, LICENSE file and last commit date were read from the clone.
Stars, issues and commits come from the GitHub web page, read through WebFetch, because this session's GitHub API is limited to the user's own repos. Treat those numbers as **approximate**.
"OmniRoute-compatible" means the component either makes no LLM calls of its own, or its model endpoint can be changed through config or an env var. It does **not** mean tested; nothing has been run against OmniRoute yet.

## Resolution

All 20 resolved. Two of the names in the brief were wrong or cut off:
- `reason-machine/…` → **reason-machines/reason-network** (the org name is plural)
- `kanaworksai-…` → **kanaworksai-hue/audio-studio-mcp**

## Table

| # | Repo | Real purpose | Interface | Lang | License | Last commit | Stars / commits / open issues | OmniRoute-compatible? | Overlaps |
|---|---|---|---|---|---|---|---|---|---|
| 1 | stablyai/orca | Desktop "orchestrator": runs Claude Code / Codex / OpenCode / Pi side by side, each in its own git worktree, with a mobile companion app | Electron desktop app (Linux AppImage/AUR) | TS | MIT | 2026-10-31 | 112k / 18.3k / 4.2k | Indirect: it launches agent CLIs, so each CLI's own base URL applies (repo references `ANTHROPIC_BASE_URL`, `OPENCODE_BASE_URL`) | Ruflo |
| 2 | ruvnet/ruflo | "Agent meta-harness" for Claude Code/Codex: plugins, MCP server, console, routing/policy mods, federation | Claude Code plugin + MCP (`npx ruflo mcp start`) + CLI | TS (+Rust) | MIT | 2026-10-31 | 80.4k / 10.0k / 668 | Yes: `OPENAI_BASE_URL`, `OPENROUTER_BASE_URL`, `OLLAMA_BASE_URL` in code | Orca, Reason Network |
| 3 | reason-machines/reason-network | P2P end-to-end-encrypted messaging between coding-agent sessions on teammates' machines | Daemon + MCP server | Rust? (not checked) | MIT OR Apache-2.0 | 2026-10-30 | 1.4k / 284 / 8 | N/A, makes no LLM calls. **Requires sign-in to a Reason account** (free); relay can be self-hosted | Ruflo federation |
| 4 | vercel-labs/durable-harness | Runs agent harnesses as durable Workflow-SDK runs; resumes after a crash from the event log | Library (npm), Next.js-oriented | TS | Apache-2.0 | 2026-10-29 | 274 / 26 / 5 | Partial: examples use Vercel AI Gateway; `ANTHROPIC_BASE_URL` exists. **0.0.x, experimental** | fx0 (same libfx/World stack) |
| 5 | feder-cr/git-ai ("gitai") | Open-source version of Anthropic's described "Scion" server-side VCS for many parallel agents (CAS landing, changeset log) | Go server + client/SDK | Go | MIT (GitLab-derived; `ee/` carve-out, but no `ee/` dir present) | 2026-10-23 | 1.0k / 396 / 1 | N/A for core (server never calls an LLM); optional conflict resolver takes "any provider" | Orca worktrees |
| 6 | Graphify-Labs/graphify | Turns repo+docs into a queryable knowledge graph (tree-sitter, no embeddings) | CLI (`graphifyy` via uv) + agent skill | Python | Apache-2.0 | 2026-10-30 | 139.8k / 2.7k / 728 | Yes: `OPENAI_BASE_URL`, `OLLAMA_BASE_URL`. Code parsing needs no LLM | Graphicle. README upsells paid Graphify Cloud |
| 7 | akitaonrails/ai-memory | Cross-agent long-term memory + typed handoffs (Claude Code → Codex), self-hosted server | Rust server + CLI + hooks (20+ harnesses) | Rust | MIT | 2026-10-30 | 14.7k / 5.2k / 26 | Yes: `AI_MEMORY_LLM_BASE_URL`; default path makes zero LLM calls | Hy-Memory, Graphicle memory |
| 8 | Graphicle-AI/graphicle | Code graph + project memory in one local Rust binary, serves cited excerpts | CLI + MCP | Rust | Apache-2.0 | 2026-10-30 | 483 / 48 / 3 | Yes: "any OpenAI-compatible server", `OPENAI_BASE_URL` | Graphify **and** ai-memory |
| 9 | Tencent-Hunyuan/Hy-Memory | Memory engine SDK (`hy-memory`) + MemHub desktop/CLI unifying agent traces; own 30B model optional | Python SDK + CLI/desktop | Python/TS | Apache-2.0 (Tencent preamble, no extra limits) | 2026-10-28 | 1.9k / 219 / 53 | Yes: `MEMORY_LLM_BASE_URL`, `MEMORY_EMBEDDER_BASE_URL` | ai-memory |
| 10 | microsoft/TokenEconomizer | Research plugin that cuts agent cost by learning repo "micro-skills", pruning tools, breaking failure loops | Plugin for Claude Code / Codex / Copilot CLI / Pi | Python | Apache-2.0 | 2026-10-30 | 2.7k / 60 / 3 | Probably: it makes its own model calls; `OPENAI_BASE_URL`/`ANTHROPIC_BASE_URL` found in code | ai-memory (learned facts) |
| 11 | Leonxlnx/earshot | Local CPU "ears" for agents: describe and score audio, synthesize SFX in code | CLI + Python API + MCP | Python | MIT | 2026-10-22 | 3.4k / 40 / 4 | N/A, no LLM | audio-studio-mcp (SFX) |
| 12 | ctoldigital/axuse | Browser/computer use through the accessibility tree and CDP (no screenshot loop); Linux via X11 | MCP server (Node ≥22) | JS | ⚠️ **PolyForm Noncommercial 1.0.0** | 2026-10-27 | 700 / 22 / ? | N/A, no LLM | — |
| 13 | MaryCache/imozuru | Claude Code skill: research that traces claims back to primary sources; saves sources and cites file:line | Claude Code skill/plugin | Python (helpers) | MIT | 2026-10-28 | 1.4k / 81 / ? | Runs in the host agent (no own endpoint). README is Japanese-first (English README exists) | — |
| 14 | onehumanbeing/esp32-iphone-agent | AI drives a real iPhone through an ESP32-S3 (AirPlay in, USB HID out) | Firmware + iOS app | Swift/C | MIT | 2026-10-28 | 4.1k / 57 / 13 | Yes (`LLM_BASE_URL`), but runs **on the phone** | — **hardware** |
| 15 | Entelligence-AI/agentplay | Local replay viewer for agent session transcripts (map + timeline), with redaction | CLI (`uvx agentplay`) + local web viewer | Python/TS | Apache-2.0 | 2026-10-27 | 340 / 110 / 14 | N/A, no LLM | — |
| 16 | heygen-com/hyperframes | HTML/CSS/GSAP → deterministic MP4 renderer + agent skills | CLI (npm) + skills + desktop app | TS | Apache-2.0 | 2026-10-31 | 76.4k / 6.5k / 71 | N/A, rendering makes no LLM calls (host agent writes the HTML). Needs Node 22 + FFmpeg | rossark-motion |
| 17 | vercel-labs/fx0 | Hosted web app: describe a page and an agent writes one HTML file (built on libfx) | Next.js web app | TS | Apache-2.0 | 2026-10-30 | 418 / 108 / 3 | ⚠️ **No.** Hard-wired to Vercel AI Gateway (`ai-gateway.vercel.sh`), Vercel sign-in, Redis + Neon | Claude itself can write the HTML |
| 18 | EL4CTEO/blender-skills | 28 Blender workflow skills for coding agents | Agent skills (+ optional Blender MCP) | Python | MIT | 2026-10-30 | 615 / 8 / ? | Runs in the host agent | — **heavy app** (Blender 5.2+) |
| 19 | kanaworksai-hue/audio-studio-mcp | Local music/SFX generation with Stable Audio 3 | MCP server | Python | MIT (code). Model weights: separate Stability license, **unconfirmed** | 2026-10-24 | 548 / 48 / ? | N/A, no LLM. Local model | earshot |
| 20 | rossark0/rossark-motion-skill | Skill for "alive" character/illustration animation | Agent skill + templates | Swift / TS (React Native) | MIT | 2026-10-22 | 581 / 11 / ? | Runs in the host agent | hyperframes |

`?` = the page showed no count.

## Flags

**Non-commercial license**
- **axuse**: PolyForm Noncommercial. If the end-to-end task is for a real Cyprus business, this is a blocker. Phase 1 needs an alternative, such as Playwright MCP (not verified here).

**Doesn't match the brief, or can't use OmniRoute as is**
- **fx0** is a hosted Vercel app, not a local tool. Its model calls go to Vercel AI Gateway (paid, billed to your Vercel team), so it breaks the "free providers via OmniRoute" rule unless the code is changed.
- **rossark-motion-skill** targets **SwiftUI / React Native**, not animated SVG as the brief says.
- **durable-harness** is 0.0.x, has 26 commits, and is built around Vercel Workflow World + AI Gateway. Using it locally on Linux with OmniRoute is unproven.

**Hardware or heavy dependencies**
- **esp32-iphone-agent** needs an ESP32-S3 board + iPhone (iOS 18+).
- **blender-skills** needs Blender 5.2+.
- **audio-studio-mcp**: the README names only **Apple Silicon (MLX)** and **Windows + NVIDIA (PyTorch)**. **Linux is not listed.** It also downloads a ~5.9 GB model, and the gated Hugging Face weights need an account.

**Needs an account or external service**
- **reason-network** requires a Reason account sign-in (free).
- **graphify** advertises a paid Cloud product; the OSS CLI is local.

**Maturity**
- Low maturity (<100 commits): durable-harness (26), blender-skills (8), rossark (11), axuse (22), earshot (40), graphicle (48), audio-studio (48), esp32 (57), TokenEconomizer (60).
- None are archived. All had commits between 22 and 31 Oct 2026.

## Unverified assumptions (TODO)

- [ ] Stars/issues/commits come from WebFetch page summaries and have not been cross-checked against the API.
- [ ] The base-URL support listed above comes from grepping the code; nothing was tested against OmniRoute.
- [ ] The Stable Audio 3 weights license (commercial-use terms) has not been read from the source.
- [ ] Whether audio-studio-mcp can run on Linux + NVIDIA (the code may support it even though the README doesn't).
- [ ] Whether ruflo's mods or console send telemetry.
- [ ] `~/omniroute-free-providers.csv` and OmniRoute (`localhost:20128`) exist on **your PC**. This cloud session can't see them.
