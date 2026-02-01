# The Prompting Manifesto (2026 Edition)

**A canonical governance framework for reliable LLM behavior, agent systems, and prompt orchestration.**

---

## What This Repository Is

This repository contains **The Prompting Manifesto (v2026.0)** — a **normative, versioned canon** that defines how Large Language Models *should be constrained, guided, and governed* in professional, high-stakes, and production contexts.

This is **not** a prompt library.  
This is **not** a collection of tips or tricks.  
This is **not** a beginner tutorial.

It is a **behavioral constitution** designed primarily for **LLMs and agent systems**, with human readability as a deliberate byproduct.

If you are building:
- custom GPTs
- agentic workflows
- orchestration layers
- decision-support systems
- automation that can cause real harm when wrong  

…this document is intended to be **loaded as context**, referenced by tools, and enforced by systems.

---

## What This Repository Is *Not*

To avoid confusion:

- ❌ Not a “best prompts” list  
- ❌ Not a creative writing guide  
- ❌ Not roleplay or vibe optimization  
- ❌ Not a how-to book for casual users  
- ❌ Not a place for stylistic experimentation  

If you are looking for:
- clever phrasing
- one-shot prompts
- personality tuning
- demos that look good but break later  

This is not the right repository.

---

## How to Use the Manifesto

### 1. As LLM / Agent Context (Primary Use)

The Manifesto is designed to be:
- embedded into system instructions
- included as a knowledge file in Custom GPTs
- referenced by prompt builders, auditors, or linters
- used as a governing document for agent behavior

You do **not** need to expand it into prose for this to work.  
Its structure, labels, and hierarchy are intentional.

---

### 2. As a Review & Audit Framework

Use the Manifesto to:
- audit prompts before deployment
- diagnose hallucinations or failure modes
- identify missing constraints or unsafe incentives
- explain *why* a prompt or agent failed

This is especially effective when paired with:
- prompt review checklists
- audit packets
- postmortems
- governance tooling

---

### 3. As a Shared Language Across Teams

The Manifesto provides:
- stable section IDs
- named failure modes
- consistent terminology

This allows teams to say things like:
- “This violates §9 (Permission to Say ‘I Don’t Know’)”
- “This is a Context Shoving issue (§27)”
- “We skipped the Epistemic Checkpoint (§12a)”

Shared language reduces debate and increases correctness.

---

## How This Document Is Structured

- **Foundational Truths** → what LLMs are and are not  
- **Operating Rules** → non-negotiable constraints  
- **Control Tools** → how prompts govern behavior  
- **Anti-Hallucination Tools** → grounding and validation  
- **Decision & Execution Patterns** → reliability under action  
- **Orchestration Rules** → agent systems and MCP-era risks  
- **Anti-Patterns** → how prompts and systems quietly fail  

This is a **rule graph**, not a narrative.

---

## Change Policy (Important)

This repository follows a **conservative change policy**.

### Allowed Changes
- Adding **new sections** to address new failure modes
- Clarifying definitions with minimal, precise language
- Adding enforcement triggers or scope notes
- Appending appendices (case studies, examples, tooling)

### Discouraged / Rejected Changes
- Expanding sections into essays
- Adding “friendly” or pedagogical prose
- Introducing vibes, tone advice, or stylistic guidance
- Diluting rules for convenience
- Rewriting existing sections without strong justification

### Guiding Principle

> **If a rule cannot be enforced, it does not belong here.**

---

## Versioning

- The Manifesto is versioned (e.g., `v2026.0`)
- Section numbers are **stable** and must not be renumbered
- Backwards compatibility is expected
- Breaking conceptual changes require strong justification

Treat this like a specification, not a blog post.

---

## Who Should Contribute

Contributions are welcome **only if** you are addressing:
- observed failure modes in real systems
- cross-model behavior (GPT, Gemini, Claude, etc.)
- agent orchestration issues
- governance, safety, or reliability gaps

If your contribution starts with:
> “I think it would be clearer if…”

…it probably does not belong.

If it starts with:
> “This failed in production because…”

…it probably does.

---

## License / Usage

This repository is intended to be:
- copied into internal systems
- embedded into tooling
- referenced by governance processes

Attribution is appreciated but not required unless otherwise stated.

---

## Final Note

This Manifesto exists because:

> **Bad prompts scale harm faster than good prompts scale value.**

If this document feels strict, that’s intentional.  
If it feels unfriendly, that’s a feature.

Reliability is not a vibe.  
It is enforced.

---

**Version:** 2026.0  
**Status:** Canon  
