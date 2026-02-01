# The Prompting Manifesto (2026 Edition)

**Canonical reference context for governing Large Language Model behavior.**

---

## What This Repository Contains

This repository contains a single authoritative artifact:

- **`Prompting-Manifesto-v2026.0.md`**

The file is a **normative specification**, not a tutorial or prompt library.  
It defines a stable set of principles, rules, and failure modes for **reliable, production-grade use of Large Language Models and agent systems**.

The primary purpose of this file is to be:
- loaded as **context** for LLMs
- referenced by tooling, audits, or builders
- used as a shared doctrinal baseline across teams and systems

Human readability is intentional, but **not the primary design goal**.

---

## What This Manifesto Is

- A **governance document** for LLM behavior
- A **control framework** for prompts, agents, and orchestration
- A **shared vocabulary** for discussing failure modes and risk
- A **stable reference** intended to evolve slowly and conservatively

Think of it as a *constitution* for prompt- and agent-based systems.

---

## What This Manifesto Is *Not*

To avoid misuse:

- ❌ Not a prompt generator
- ❌ Not a prompt library
- ❌ Not a how-to guide or beginner tutorial
- ❌ Not a collection of “tips and tricks”
- ❌ Not optimized for creativity, tone, or style

If you are looking for ready-made prompts or examples, this repository is not the right place.

---

## How This File Is Intended to Be Used

### 1. As Model Context

The Manifesto is designed to be:
- embedded into system instructions
- included as a knowledge file for LLMs
- used as a reference document for agents or orchestration layers

It is written to be **interpretable by models without requiring prose expansion**.

---

### 2. As a Canonical Reference

Teams can use the Manifesto as:
- a shared baseline for discussions
- a reference during design and review
- a way to name and categorize failure modes

Example (human use):
- “This violates §9 (Explicit Permission to Say ‘I Don’t Know’)”
- “This is a Context Shoving issue (§27)”
- “We skipped the Epistemic Checkpoint (§12a)”

---

### 3. As an Input to Tooling

The file may be consumed by:
- prompt builders
- linters
- audit systems
- policy enforcement layers
- documentation generators

Nothing in the file assumes a specific tool, platform, or vendor.

---

## Structure at a Glance

- **Foundational Truths** — what LLMs are and are not  
- **Operating Rules** — non-negotiable constraints  
- **Control Tools** — how prompts govern behavior  
- **Anti-Hallucination Tools** — grounding and validation  
- **Decision & Execution Patterns** — reliability under action  
- **Orchestration Rules** — agent systems and MCP-era risks  
- **Anti-Patterns** — common and costly failure modes  

The structure is deliberate and should not be flattened into prose.

---

## Change Policy (Important)

This repository follows a **conservative change policy**.

### Appropriate Changes
- Adding new sections for newly observed failure modes
- Adding concise definitions or trigger conditions
- Clarifying enforcement boundaries
- Appending optional appendices (case studies, examples)

### Inappropriate Changes
- Expanding sections into essays
- Adding pedagogical or “friendly” explanations
- Rewriting existing sections for style or tone
- Diluting rules for convenience
- Renumbering sections

**If a rule cannot be enforced, it does not belong here.**

---

## Versioning & Stability

- The Manifesto is versioned (e.g., `v2026.0`)
- Section numbers are **stable identifiers** and must not be changed
- Backwards compatibility is expected
- Conceptual breaking changes should be rare and well-justified

Treat this file like a specification, not a blog post.

---

## Intended Audience (Clarified)

Primary consumer:
- **LLMs, agent systems, and prompt-orchestrating models**

Secondary consumers:
- Engineers, architects, and teams responsible for reliability

The document is **model-first by design**.

---

## Final Note

This Manifesto exists because:

> **Bad prompts scale harm faster than good prompts scale value.**

If this document feels strict or uncompromising, that is intentional.

Reliability is not a vibe.  
It is governed.

---

**Version:** 2026.0  
**Status:** Canon
