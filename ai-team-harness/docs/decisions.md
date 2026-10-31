# Decisions

## Constraints (user, 2026-10-31)
- Commercial use, so non-commercial licenses are excluded.
- Drop **axuse** (PolyForm Noncommercial) and **fx0** (hard-wired to the paid Vercel AI Gateway).
- Orchestrator = Claude Opus 5.5. Sub-agent and bulk LLM calls go through OmniRoute (`http://localhost:20128/v1`) using free providers only.
- Simplest harness that works: ≤ 8 core components for v1.

## Phase 1: one winner per layer

OmniRoute is a prerequisite, not one of the 8 components.

| Layer | Winner | Why | Cut |
|---|---|---|---|
| Fleet manager + git isolation | **stablyai/orca** (MIT) | Runs any CLI agent, each in its own git worktree. Has a scriptable CLI, Linux builds, the most maturity (112k★, 18k commits). Sub-agents run as OpenCode/Pi pointed at OmniRoute. | **ruflo**: overlaps; huge surface (8k+ files, 100+ agents); its mods run with account permissions. **git-ai**: a server-side VCS built for hundreds of agents is overkill for ~6 on one PC; git worktrees are enough. **reason-network**: links agents across teammates' machines; one PC doesn't need it, and it needs an account. |
| Shared memory | **akitaonrails/ai-memory** (MIT) | Cross-agent memory + typed handoffs; makes zero LLM calls by default; self-hosted; 14.7k★, 5.2k commits. | **Hy-Memory**: heavier SDK with several stores, aimed at chat memory. **Graphicle**: memory half duplicates ai-memory. **TokenEconomizer**: a cost optimizer for paid agents; little value on free providers in v1. |
| Code graph | **Graphify-Labs/graphify** (Apache-2.0) | Code is parsed with tree-sitter, deterministic and local, no LLM; 139.8k★. | **Graphicle**: young (48 commits); overlaps both graph and memory. |
| Durable resume | **None in v1.** Resume uses ai-memory handoffs + committed worktree state. | Simplest option that meets the need. | **durable-harness**: 0.0.x, 26 commits, built on Vercel Workflow World + AI Gateway; unproven locally. Revisit if crash-mid-turn becomes a real problem. **Needs your OK; see open questions.** |
| Browser | **microsoft/playwright-mcp** (Apache-2.0; replaces axuse) | Accessibility-tree browser MCP; headless on Linux; last commit 2026-10-30. | **axuse** (non-commercial license). |
| Research | **MaryCache/imozuru** (MIT) | Traces claims to primary sources and cites file:line, which fits "cited sources". | — |
| Video | **heygen-com/hyperframes** (Apache-2.0) | Turns HTML+GSAP into deterministic MP4; Node 22 + FFmpeg; Linux .deb. It also covers the prototype step: the orchestrator writes the HTML itself. | **fx0** (user decision). **rossark-motion-skill**: targets SwiftUI/React Native, not SVG; GSAP in HyperFrames covers motion. |
| Audio | **Leonxlnx/earshot** (MIT) | Runs on CPU locally; synthesizes SFX in code; can also check audio sync. | **audio-studio-mcp** moves to optional: its README names no Linux support and it needs a GPU + a 5.9 GB model. |
| Session replay | **Entelligence-AI/agentplay** (Apache-2.0) | Local, no LLM; reads existing transcripts. | — |

**Core count: 8** (orca, ai-memory, graphify, playwright-mcp, imozuru, hyperframes, earshot, agentplay).

**Optional / hardware:** esp32-iphone-agent (ESP32-S3 + iPhone), blender-skills (Blender 5.2+), audio-studio-mcp (GPU; Linux untested).

## Known gap
- **Music bed for the promo video.** earshot makes SFX, not music. Options: (a) test audio-studio-mcp on your Linux GPU, (b) a royalty-free track you supply, (c) earshot-synthesized pads. Decide before Slice 6.

## Open questions
1. Is it OK to skip durable-harness in v1 (Slice 4 becomes "resume via ai-memory handoff")?
2. Does your PC have an NVIDIA GPU (for the audio-studio-mcp music option)?
3. Phase 3 must run on your PC: connect it to this session through Remote Control, or run the steps yourself?
