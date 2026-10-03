# Candice

Candice is a personal AI built for a family. She lives in iMessage, remembers what matters and checks in before you even have to ask.

This repo documents how she is built: the message pipeline, memory system, proactive engine, tech stack and infrastructure. All in one place, with diagrams you can actually follow.

Visit the site: https://candiceai.vhenjoseph.com

## Scope — what this is and is not

### What this is

- **A single-household appliance.** One Mac, one family, one instance. Configuration is generated per install and is not meant to be shared between households.
- **An orchestrator, not a model.** Candice is glue: message plumbing, memory, scheduling, guardrails, and prompt assembly. Reasoning happens in the Claude API; embeddings, vision, and transcription happen in local Apple Foundation Model.
- **Self-hosted and self-maintaining.** It updates itself nightly behind a test gate, rolls back on red and audits its own conversations to learn behavioural rules.
- **An agent with real hands.** It has shell, file write, and web access on the machine it runs on, and it sends messages on the family's behalf.

### What this is not

- **Not a product, a service, or a hosted app.** There is no signup, no server we run, no support commitment. If it breaks at 3am, you are the on-call.
- **Not multi-tenant and not access-controlled between family members.** Each member gets a separate memory namespace and persona, but the process holds every member's data at once and there is no per-member authorization boundary inside it. Anyone who can text the butler's handle is trusted at that member's level.
- **Not a sandbox.** Permission prompts are disabled by design (see the warning below). Guardrail hooks are the entire safety net, and they are pattern-based.
- **Not a security product, and not audited.** Security file (private) is a code-reading exercise, not a validated adversarial test.
- **Not an Anthropic product**, and it ships no API key or subscription access.
- **Not financial, medical, or legal advice.** It will happily summarize your budget or a lab result. Verify anything consequential yourself.
- **Not a general-purpose framework.** It is opinionated around iMessage, macOS, Apple Silicon, and one vault layout. Portability is not a goal.

---

## Architecture

```
family member (iMessage)
      │
      ▼
chat db  ── read-only poll, ~3s
      │
      ▼
Router ── the inbound pipeline
      ├─ media enrichment   image/video → Ollama llava · audio → faster-whisper
      ├─ link/thread context
      ├─ content gate    screens UNTRUSTED text (email, web, X, RSS) for injection
      ├─ personas        per-member system prompt
      ├─ memory          mem0 + Chroma + nomic-embed-text  (local)
      ├─ context assembly 3-layer vault retrieval (vector + co-occurrence + FTS)
      │
      ▼
Local LLM ── the single cloud chokepoint
      ├─ PII masking        mask on the way out, unmask on the way back (fail-closed)
      ├─ Authority       ACT / RECOMMEND / DRAFT / BLOCK tiering
      └─ Guardrail hook  PreToolUse deny (protected paths, destructive shell)
      │
      ▼
  claude -p  ────────────────────────────────────► Claude API   ← the only egress
      │
      ▼
iMessage reply ── osascript reply (delivery verified against chat.db)


                         ── the autonomous half ──

Proactive (asyncio, 60s tick)
      └─ triggers/  morning_brief · email_watch · ai_digest · health_nudge
                    nightly_ingest · daily_self_audit · reminder_queue
                    model_watch · pattern_watch · weekly_diagnostic
                    todo_runner · mission_runner · event_reminder · …
                    (29 registered, some conditional on config; each is
                     time-windowed, cooldown-gated, and mutable)
      │
      ├─ agent runner    agentic text → unattended `claude -p` subprocess
      │                     (bounded run, git-based revert of protected paths,
      │                      restart-recovery: re-adopt or queue, never auto-rerun)
      └─ missions/goals        week-scale work in bounded chunks, survives restarts

```

**Processes.** Eight launchd agents: the FastAPI server (single uvicorn worker because SQLite is single-writer, wrapped in `caffeinate`), the dashboard, mDNS, a watchdog, two digest jobs, a nightly mem0 audit, and the nightly self-update.

**Trust boundary.** Important! Everything is local except one edge: message text, masked, goes to the Claude API. Embeddings, vectors, media understanding, and all family data at rest never leave the Mac. The honest caveat is that a tool-using agent's *mid-run* reads (file contents, command output, fetched pages) enter the model context unmasked — described in a private SECURITY file.

---

Built by Vhen. 2026.
