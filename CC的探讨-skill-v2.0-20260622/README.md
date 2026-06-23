<p align="center">
  <img src="https://img.shields.io/badge/Claude%20Code-Skill-ff6b35?logo=anthropic&logoColor=white" alt="Claude Code Skill">
  <img src="https://img.shields.io/badge/version-2.0-blue" alt="version">
  <img src="https://img.shields.io/badge/license-MIT-green" alt="license">
</p>

# CC Explore — Claude Code Skill: Cross-Session Knowledge Management

> A **Claude Code Skill** that lets AI remember the turning points of every conversation. Not file management — **attention flow management**.

---

## 🤔 The Problem

Every Claude Code user has been here:

- New session → spend 10 minutes explaining context → AI barely catches up
- Great framework discussed last time → new session, AI knows nothing about it
- Context window gets bloated → AI gets lost in history → response quality nosedives

**This Skill solves it with two commands: `/cc-save` to archive, `/cc-load` to restore.**

---

## 🚀 Install (3 steps, < 30s)

```bash
# 1. Clone
git clone https://github.com/yyyxxx-art/cc-explore.git
cd cc-explore

# 2. Install as Claude Code Skills
mkdir -p ~/.claude/skills
cp skills/cc-save.md ~/.claude/skills/
cp skills/cc-load.md ~/.claude/skills/

# 3. Open Claude Code and type:
/cc-load
```

> 💡 **What is a Claude Code Skill?** Skills are custom slash-commands (`/skill-name`) that extend Claude Code with specialized workflows. Drop a `.md` file into `~/.claude/skills/` and Claude Code loads it automatically. [Learn more](https://docs.claude.codes).

---

## 📖 Usage

| Command | What it does |
|---------|-------------|
| `/cc-save` or say "**archive**" | Smart-match to existing workflow → dual write (memory system + desktop archive) → auto integrity check |
| `/cc-load` or "**continue**" | List all active workflows |
| `/cc-load {keyword}` | Restore a workflow, prioritizing condensed context (≤500 chars) |
| `/cc-load auto {id}` | Set workflow to auto-load on every session start |

**AI also archives proactively** — detects framework births, major decisions, and milestones → proposes archive → you approve.

---

## 🧠 Design Principles

| Principle | Origin | Meaning |
|-----------|--------|---------|
| **First Principles** | Aristotle → Musk | Manage attention, not files — "where was I?" |
| **MECE** | McKinsey | One folder = one workflow thread, strictly exclusive |
| **OODA Loop** | John Boyd | Observe→Orient→Decide→Act, human at the Feedback node |

---

## 📁 Archive Structure

```
Desktop/CC的探讨/
├── .index.json          ← Workflow index
├── .auto-load.json      ← Auto-load config
├── {workflow-A}/
│   ├── summary-{date}.md
│   ├── context-condensed.md   ← Auto-generated after ≥5 saves (≤500 words)
│   └── ...
└── {workflow-B}/
```

---

## 🔬 10 Long-Term Risks — Addressed

Simulated across 6 months / 50 workflows / 200 saves. All mitigated in-design:

| # | Risk | Fix |
|---|------|-----|
| ① | **Index Drift** — index out of sync with files | Post-save integrity check with auto-repair |
| ② | **Context Bloat** — accumulated history drowns AI | Condensed context (≤500 words) after 5 saves; loaded first |
| ③ | **Ghost Context** — irrelevant context pollutes new sessions | Prompt-before-load with skip button |
| ④ | **Matching Degradation** — archives go to wrong folder | Semantic match comparison with % scores, user picks |
| ⑤ | **Workflow Proliferation** — one-off chats pile up | active→dormant→archived lifecycle |
| ⑥ | **No Garbage Collection** — append-only entropy | >180-day zombie alert, max 1 reminder/session |
| ⑦ | **Self-Reference Rot** — memory file diverges from skill logic | Self-check rule: skill files are the source of truth |
| ⑧ | **Version Divergence** — skill and memory drift apart | Clear boundary: skill = execution, memory = reminder |
| ⑨ | **Permission Decay** — cross-session auth expires | Permanent grant declaration embedded; citable on prompt |
| ⑩ | **Session Forking** — multi-window archive conflict | lastUpdated detection + warning |

---

## 🎯 Who Is This For

- Developers in deep, long-term collaboration with Claude Code (L3-L5)
- Solo devs juggling multiple parallel workflows
- Anyone tired of re-explaining context every session

---

## 📄 License

MIT © 2026 yyyxxx-art

---

> *"The number of people truly embedding AI deeply into their dev workflow is still small. First movers have an information arbitrage advantage."*
> — From this Skill's very first archive record
