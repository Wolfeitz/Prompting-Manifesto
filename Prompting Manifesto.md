# The Prompting Manifesto (2026 Edition)

**Version:** 2026.0 (clarified)
**Status:** Canon
**Change Policy:** Conservative (interpretation > expansion)
**Audience:** Teams and organizations shipping LLM-powered systems (human-readable; machine-consumable by design)

*A Discipline for Reliable AI-Powered Systems*

---

## PREFACE (Commentary — not normative)

> The following is context for human readers.
> It is not consumed as instruction by LLM systems.

### Why This Manifesto Exists

This manifesto exists because "prompting tips" are not a discipline.

Lists of clever phrases, magic words, and stylistic hacks do not scale, do not survive production, and do not protect reputations. They fail precisely when stakes are highest: automation, policy, decision-making, and long-running agentic systems.

This document treats prompting as what it actually is:

> **The engineering of incentive structures for probabilistic systems operating under ambiguity.**

### Who This Is For

This manifesto is for:

* Teams shipping LLM-powered features to production
* Platform and infrastructure teams building AI tooling
* Prompt engineers, agent builders, and system architects
* Anyone responsible for the reliability, safety, or correctness of AI-generated outputs

It is also consumable as a reference by LLMs, agent systems, and prompt-orchestrating models — but the primary audience is the humans who design, review, and maintain these systems.

This manifesto is *not* for:

* Prompt hobbyists
* Vibe optimization
* Creative roleplay
* One-off demos

This document is designed for contexts where correctness, reliability, and reputation matter.

---

## HOW TO USE THIS MANIFESTO

### Read Paths

* **Executive / Risk & Governance**
  Focus on Parts I, III, VIII, IX, and XI

* **Practitioner / Builder**
  Focus on Parts II through VII

* **Agent & Platform Architect**
  Focus on Parts III, V, VII, and VIII

### Interpretation Rule

This manifesto is **normative**, not descriptive.

* When a rule conflicts with convenience, correctness takes priority.
* When sections appear to overlap, the higher-numbered **Part** governs scope, not precedence.
* Stopping without producing an answer is a valid and sometimes correct outcome.

### How to Apply This Incrementally

You are not expected to apply everything at once.

Apply rules proportionally to:

* blast radius
* irreversibility
* cost of being wrong

### Model Capability Assumptions

This manifesto assumes a **capability floor** roughly equivalent to current frontier models (e.g., Claude Opus/Sonnet, GPT-4-class, Gemini Pro). Not all rules apply equally to all model sizes.

| Capability Tier | Examples | Applicability |
|---|---|---|
| Frontier (>100B, instruction-tuned, extended thinking) | Claude Opus/Sonnet 4.x, GPT-4o, Gemini 1.5 Pro | Full manifesto applies |
| Mid-range (7B–70B, instruction-tuned) | Qwen 72B, Llama 3 70B, Mistral Large | Parts I–VII apply. Parts VIII (agentic) and advanced techniques (CCP, epistemic checkpoints) require validation per model. |
| Small (≤7B) | Qwen 3B, Phi-3, Llama 3 8B | Parts I–V apply (structural rules, constraints, grounding). Metacognitive techniques (Sections 12, 12a, 40) may exceed model capability and produce unreliable results. Agentic use (Part VIII) is not recommended without extensive guardrails. |

**The structural principles are universal.** Incentive design, ambiguity reduction, output constraints, and anti-patterns apply to any autoregressive model. What changes with scale is the model's ability to follow *multi-step metacognitive instructions* — self-assessment, confidence calibration, epistemic checkpoints, and sustained autonomous reasoning. When in doubt, test the specific technique on the specific model before relying on it in production.

### Autonomy-Based Applicability (Clarification)

This manifesto distinguishes requirements by **degree of autonomy**, not by UI surface (e.g., “chat” vs “system”). Interface labels change; autonomy does not.

| Prompt / System Type                                                        | Required Parts           |
| --------------------------------------------------------------------------- | ------------------------ |
| Human-in-the-loop chat                                                      | Parts I–V                |
| Decision-support chat                                                       | Parts I–VII              |
| Agentic / autonomous systems (self-directed continuation, tools, or memory) | Parts I–VIII (mandatory) |

When a system can continue without human correction, **Part VIII applies in full**.

**Cross-cutting concerns:** Parts IX (Anti-Patterns) and X (Practical Application) apply at all autonomy levels. Part IX provides failure-mode awareness regardless of system type. Part X provides deployment checklists and tool selection guidance that scale with stakes, not autonomy.

---

# PART I — THE REALITY OF LLMs (FOUNDATIONAL TRUTHS)

> *If you disagree with this section, stop reading.*

## 1. What LLMs Actually Are (and Are Not)

* Statistical optimization engines, not knowledge bases
* Reasoning capabilities are real but bounded — models can chain logic, decompose problems, and self-correct, but these abilities degrade under ambiguity and are not guaranteed to generalize. Extended thinking (chain-of-thought, scratchpads) significantly improves reliability but does not eliminate failure modes
* Compression of patterns, not retrieval of facts — but modern models increasingly combine pattern recognition with learned reasoning heuristics that produce genuine inference
* “Intelligence” is emergent and task-dependent, not a stable internal property — treat capability as something to verify per-task, not assume globally

## 2. Autoregression, Momentum, and Plausibility

* Accuracy loses to momentum unless constrained — though extended thinking and self-verification reduce this effect, they do not eliminate it
* Models continue because continuation is cheaper than stopping
* Plausible output is often lower cost than uncertainty
* **Current-generation caveat:** Models with extended thinking (e.g., chain-of-thought, reasoning traces) can catch and correct momentum errors mid-generation. This makes the problem less visible, not absent — which arguably makes explicit constraints *more* important, since silent self-correction is not auditable

## 3. Helpfulness as a Failure Mode

* Models are optimized to be agreeable — recent training (RLHF, constitutional AI) has reduced raw sycophancy, but the pressure to produce *something useful* remains the dominant optimization target
* Silence is interpreted as permission
* Guessing is rational behavior under ambiguity — newer models are better at expressing uncertainty unprompted, but will still default to a best-guess answer when the prompt does not explicitly authorize refusal

## 4. Confidence Is Stylistic, Not Epistemic

* Tone does not reflect certainty — though current-generation models are increasingly trained to calibrate hedging language with actual uncertainty, the correlation is loose and should not be relied upon
* Polished answers are not more reliable
* Overconfidence is often a surface artifact — models trained with extended thinking may *internally* represent uncertainty in their reasoning traces while still producing confident-sounding final outputs

## 5. Reputation Is Downstream of the Worst Answer

* One wrong answer outweighs many correct ones
* Demos lie; production remembers
* Risk is asymmetric and cumulative

---

# PART II — PROMPTING AS AN ENGINEERING DISCIPLINE

## 6. Prompts as Incentive Structures

* Every prompt defines an optimization target
* Unspecified objectives are filled implicitly
* Remove incentives to guess or over-extend

## 7. Ambiguity Is the Root of Hallucination

* Hallucinations are rational under uncertainty
* Missing inputs differ from vague goals
* Refusal is sometimes the correct behavior

## 8. Control Layers vs Wordsmithing

* Structure beats phrasing
* Constraints beat cleverness
* Prompting is governance, not persuasion

---

# PART III — OPERATING RULES FOR RELIABLE PROMPTING

> *These apply to every serious use case.*

## 9. Explicit Permission to Say “I Don’t Know”

**Applies when:** facts, estimates, high-stakes decisions, or missing information are involved.

* Refusal must be explicitly allowed
* Guessing must never be implied as required
* Stopping without an answer is valid

## 10. Truth Anchoring & Fact-Check First

**Applies when:** queries are verifiable (prices, schedules, standings, winners).

* Separate verifiable facts from inference
* Browse immediately when verification exists
* Stop if confirmation is unavailable

## 11. Citations, Sources, and Temporal Awareness

**Applies when:** recency, currency, or “latest” claims are involved.

* Always distinguish event date vs publish date
* “Recent” requires an explicit time anchor
* Today is a variable, not an assumption

## 12. Confidence Calibration & Accuracy Mode

**Applies when:** recommendations, judgments, or conclusions are provided.

* Label confidence (High / Medium / Low)
* Optionally provide numeric confidence (0.0–1.0)
* State what would raise confidence

## 12a. Epistemic Checkpoint (Pre-Commit Confidence Gate)

**Applies when:**

* factual correctness matters
* blast radius is medium or higher
* the model is about to produce a confident or fluent answer

**Rule:**
Before generating the final answer, the model must briefly assess:

* whether the question is answerable with available information
* its confidence level (qualitative or numeric)
* what specific uncertainty remains, if any

This assessment must **not** be presented as full chain-of-thought.
It may be a single labeled line, a confidence score, or a short uncertainty statement.

**Purpose:**
To surface epistemic uncertainty *before* linguistic momentum commits the model to a confident narrative.

## 13. Answerability Checks

**Applies when:** questions are underspecified or absolute.

* Identify unanswerable questions
* List required missing inputs
* Reframe instead of fabricating

## 14. Accountability & Correction

**Applies when:** guidance caused waste or error.

* Acknowledge plainly
* Correct without defensiveness
* Pivot with minimal delay

---

# PART IV — PROMPT CONSTRUCTION TOOLS (CONTROL LAYER)

## 15. Output Specification

* Deliverable, audience, format, length, tone
* Output medium: screen, markdown, PDF, DOCX, canvas
* Must-include vs must-avoid

## 16. Mission Locking & Scope Bounding

* Define the primary objective
* Specify pivot conditions
* Enforce scope ceilings

## 17. Role Framing (Expert Lens)

* Expertise, not roleplay
* Decision perspective matters
* Roles must change judgment, not tone

## 18. Response Mode Declaration

* Teach vs decide vs explore
* Suppress recommendations when required
* Suppress explanations when required

## 19. Interaction Pattern Control

* Question → plan → execute
* Hold-for-confirmation patterns
* Sequencing as safety

## 20. Constraints, Invariants, and Degrees of Freedom

* Hard vs soft constraints
* What must not change
* What may change

## 21. Negative Space Prompting

* Explicitly forbid behaviors
* Avoid buzzwords, frameworks, fluff
* Silence is not neutrality

---

# PART V — ANTI-HALLUCINATION & GROUNDING TOOLS

## 22. Intake Gates

* Ask only what changes the answer
* Limit clarification count
* Proceed with labeled assumptions

## 23. Assumption Ledgers

* Make uncertainty explicit
* Enable cheap correction
* Provide validation hooks

## 24. Ground Truth Declaration

* Override model priors
* Establish authority hierarchy
* Separate internal from public truth

## 25. Show Me the Inputs

* Minimum artifact principle
* Logs, configs, samples, screenshots
* Grounding beats verbosity

## 26. Knowledge Cutoff Awareness

* Flag potential staleness
* Reason from first principles
* Avoid fake recency

## 27. Context Ranking (Anti–Context Shoving)

* Attention is finite
* Large context ≠ correct prioritization
* Rank snippets by relevance

---

# PART VI — DECISION & STRATEGY PROMPTING

## 28. Options Menus

* Multiple viable paths
* Explicit tradeoffs
* Avoid false certainty

## 29. Perspective Framing

* Pragmatic / Visionary / Challenger
* Do not force all perspectives
* Surface alternative solution spaces

## 30. Decision Drivers

* Explicit optimization criteria
* One-line choice rules
* Align to stakeholder values

## 31. Failure-First Framing

* Start from breakdowns
* Pre-mortems over optimism
* Risk discovery first

## 32. Temporal Framing

* Now vs 6 months vs 2 years
* What changes and why
* Time-dependent correctness

---

# PART VII — EXECUTION, VERIFICATION, AND LOOP CONTROL

## 33. Verification Loops

* Do → See → If Not
* Single fallback discipline
* Debugging as first-class behavior

## 34. Sanity Check Passes

* Common-sense validation
* Constraint re-checks
* Contradiction detection

## 35. Output Audit Questions

* What is most likely wrong?
* Identify weakest links
* Enforce self-skepticism

## 36. Direction Reset Logic

* Track failed branches
* Pause after second failure
* Propose alternatives

## 37. Stop Conditions

* Prevent infinite loops
* Detect diminishing returns
* Escalate or abandon explicitly

---

# PART VIII — AGENTS, TOOLS, AND ORCHESTRATION (2026)

## 38. Context Decay & Entropy Management

* Context accumulates error
* Chatter reinforces mistakes
* Reset and summarize deliberately
* **Decay signals:** Contradictions with earlier statements, loss of constraint adherence, increasing verbosity, and hallucinated references to prior turns
* **Reset strategy:** Summarize current state → discard conversation history → re-inject summary + original constraints as a fresh prompt
* **When to force a reset:** After observing any decay signal, or proactively at fixed turn intervals in long-running agents

## 39. Orchestrator-Worker Topologies

* Flat agent collaboration fails
* Central synthesis required
* Rewrite state explicitly
* **Why flat fails:** Without a single point of synthesis, contradictory worker outputs merge silently into downstream state, producing incoherent results
* **When to rewrite state vs pass through:** Pass through when worker output is self-contained and verified; rewrite when outputs must be reconciled, ranked, or when downstream consumers need a unified view
* **Minimum viable orchestrator:** Accepts worker outputs, validates against task constraints, resolves conflicts, and produces a single canonical state before dispatching the next step

## 40. Epistemic vs Linguistic Steering

* Tone instructions mislead
* Calibrated Confidence Prompting (CCP) — a technique defined in this manifesto for separating epistemic assessment from answer generation
* Access belief state before language
* **How tone masks uncertainty:** Instructions like "be confident" or "be authoritative" suppress hedging language without changing underlying model certainty — the output *sounds* sure while the model is not
* **What CCP is:** A two-step prompting pattern that separates the confidence assessment step from the answer generation step, preventing linguistic momentum from inflating stated confidence. This is not a widely established term — it is a named practice introduced here to make the pattern referenceable.
* **Practical implementation:** First prompt the model to assess what it knows and doesn't know (epistemic step); then generate the answer constrained by that assessment (linguistic step). Never combine both in a single pass.
* **Current-model note:** Models with extended thinking (chain-of-thought) may perform internal confidence calibration during their reasoning trace. CCP remains valuable because it makes calibration *explicit and auditable* rather than relying on opaque internal processes.

## 41. Tool Use Transparency (MCP / RAG Era)

* No implicit tool reliance
* Log expectations and observations
* Discard output if raw results are missing
* **Minimum logging requirements:** Every tool invocation must record: tool name, input parameters, raw output, and whether the output was incorporated into the final response
* **Discard in practice:** If a tool returns empty, malformed, or unverifiable results, the model must not silently substitute generated content — it must either retry, report the failure, or refuse the claim
* **RAG-specific concern:** Retrieved chunks must be cited with source and recency metadata; answers that cannot be traced to a specific retrieved passage should be flagged as unsupported

## 42. Memory Use & Boundaries

* Store only durable preferences
* Prefer ephemeral context
* Avoid contamination and privacy risk
* **What qualifies as durable:** User-confirmed preferences, verified constraints, and explicitly saved decisions. Inferred preferences and single-use context do not qualify.
* **Contamination risk:** Prior conversation state can bias future outputs. When memory persists across sessions, stale assumptions, outdated facts, and resolved constraints can silently re-enter the optimization target.
* **Privacy boundary:** Never persist PII, credentials, or sensitive business data in memory unless the system is explicitly designed and authorized for it. Default to ephemeral.

---

# PART IX — ANTI-PATTERNS (HOW PROMPTS DIE)

## 43. Magic Spell Prompting

* **Definition:** Treating specific phrases ("take a deep breath," "think step by step") as incantations that reliably improve output regardless of context.
* **Trigger:** Prompt author copies phrasing from tip lists without understanding the underlying mechanism.
* **Why it fails:** Phrases work when they restructure the optimization target. Copied without that understanding, they're noise that consumes context and teaches false confidence in the prompt.

## 44. Roleplay Masquerading as Expertise

* **Definition:** Assigning a persona ("You are a world-class lawyer") and treating the output as if the model now possesses domain expertise.
* **Trigger:** Role framing used as a substitute for domain constraints, ground truth, or verification.
* **Why it fails:** Roles shift tone and vocabulary distribution, not knowledge. The model doesn't gain competence — it gains the *appearance* of competence, which is more dangerous than no role at all.

## 45. Silent Tool Use

* **Definition:** Allowing or encouraging the model to use tools (search, code execution, retrieval) without logging what was called, what was returned, or whether the result was actually used.
* **Trigger:** Tool-augmented prompts that don't require the model to cite or surface tool outputs.
* **Why it fails:** Unlogged tool use is unauditable. If the tool returns garbage or nothing, the model will paper over the gap with generated text, and no one will know.

## 46. Context Shoving

* **Definition:** Dumping large volumes of reference material into context under the assumption that more context = better answers.
* **Trigger:** RAG pipelines that retrieve aggressively, or users who paste entire documents "just in case."
* **Why it fails:** Attention is finite and non-uniform. Large contexts dilute relevance ranking and increase the probability that the model latches onto irrelevant fragments. Context is not comprehension.

## 47. Chain-of-Thought Fetishism

* **Definition:** Requiring explicit chain-of-thought reasoning for every query regardless of whether the task benefits from it.
* **Trigger:** Blanket "always show your reasoning" instructions applied uniformly.
* **Why it fails:** CoT is useful for multi-step reasoning but counterproductive for recall, classification, and simple generation. Forced reasoning creates plausible-sounding justifications that may not reflect actual model computation, and consumes token budget.

## 48. Infinite Iteration Without Reset

* **Definition:** Continuing to refine the same output through repeated revision passes without resetting context or re-establishing the objective.
* **Trigger:** "Try again" / "Make it better" loops beyond 2–3 iterations.
* **Why it fails:** Each iteration compounds context decay. The model increasingly optimizes for the delta from the last version rather than the original objective. Error accumulates, and the output drifts from intent while appearing to improve.

## 49. Treating Outputs as Answers Instead of Hypotheses

* **Definition:** Accepting model output as settled fact rather than a hypothesis requiring validation.
* **Trigger:** Any workflow where model output is consumed downstream without human review or verification.
* **Why it fails:** Outputs are the model's best statistical completion, not verified truth. Treating them as answers skips the verification step that separates useful AI from dangerous AI.

---

# PART X — PRACTICAL APPLICATION

## 50. Prompt Review Checklist

Before deploying a prompt to production, validate against these criteria:

**Structure & Constraints**
* Does the prompt specify output format, length, and audience?
* Are hard constraints and invariants explicit (Section 20)?
* Is scope bounded with clear pivot conditions (Section 16)?

**Failure & Uncertainty Handling**
* Is the model explicitly permitted to say "I don't know" (Section 9)?
* Are refusal behaviors defined for edge cases?
* Is confidence calibration required for the output type (Section 12)?
* Does the prompt include an epistemic checkpoint for high-stakes outputs (Section 12a)?

**Grounding & Verification**
* Are sources of ground truth declared (Section 24)?
* Is there a verification loop or sanity check pass (Sections 33–34)?
* For tool-augmented prompts: are tool calls logged and outputs cited (Section 41)?

**Red Flags — Reject If Present**
* Model is incentivized to guess rather than refuse
* No stop condition or fallback for failure
* Role framing substitutes for domain constraints (Anti-pattern 44)
* Tool outputs are consumed without logging (Anti-pattern 45)
* Prompt contains copied "magic" phrases without clear structural purpose (Anti-pattern 43)

## 51. Choosing the Right Tools for the Job

* Correctness-critical
* Decision-critical
* Exploration-only
* Low-stakes vs high-stakes

| Use Case             | Applicable Parts | Key Sections         |
|----------------------|------------------|----------------------|
| Correctness-critical | III, V, VII      | 9, 10, 12a, 33, 34  |
| Decision-critical    | III, VI          | 12, 28, 29, 30, 31  |
| Exploration-only     | II, IV           | 15, 17, 18, 21      |
| Low-stakes           | II, IV           | 15, 16 (light)      |
| High-stakes          | III, V, VI, VII  | All of III + 33–37   |

## 52. Maturity Model for Prompting Teams

**Level 1 — Ad-hoc**
Prompts are written per-task with no reuse, review, or standards.
No shared patterns. Success depends entirely on individual skill.
→ Advance when: team agrees on a shared prompt structure or review step.

**Level 2 — Structured**
Prompts follow a consistent format. Templates or scaffolds exist.
Some reuse across projects. No formal review or failure tracking.
→ Advance when: prompts are reviewed before deployment and failures are logged.

**Level 3 — Governed**
Prompt review is mandatory for production use. Failure modes are tracked.
Confidence calibration and refusal behaviors are enforced.
Anti-hallucination controls (Parts V, VII) are standard practice.
→ Advance when: prompts are version-controlled, tested, and monitored in production.

**Level 4 — Orchestrated**
Prompts are components in larger systems (agents, pipelines, workflows).
Context management, tool transparency, and reset logic are enforced.
Prompts are tested adversarially before deployment.
Failure postmortems feed back into prompt design.

---

# PART XI — CLOSING

## 53. The Manifesto (Condensed)

* Correctness over convenience
* Explicit uncertainty over confident guessing
* Structure over phrasing
* Enforcement over advice

## 54. Final Warning

* Bad prompts scale harm faster than value
* Reliability is earned, not implied

---

# APPENDICES

## Companion Tools

This manifesto is designed to be referenced by prompt-building tools
and agent systems. Implementations should declare which Parts they enforce
and at what autonomy level. See the Next Gen GPT Builder (v2026.6+)
for one such implementation.

## Contribution Guidelines

* Prefer adding new sections over expanding existing ones
* Add definitions, triggers, or failure modes before prose
* If a rule cannot be enforced, it should not be added
* Preserve backwards compatibility

## Appendix A — Glossary

| Term | Definition | First Appears |
|---|---|---|
| **Autoregression** | The mechanism by which LLMs generate output one token at a time, each conditioned on all prior tokens. Creates momentum effects where early output biases later output. | Section 2 |
| **Blast radius** | The scope of damage if a model output is wrong. Determines how strictly the manifesto should be applied. | How to Apply |
| **CCP (Calibrated Confidence Prompting)** | A two-step technique (defined in this manifesto) that separates epistemic assessment from answer generation to prevent linguistic momentum from inflating stated confidence. | Section 40 |
| **Chain-of-thought (CoT)** | A prompting technique that asks the model to show intermediate reasoning steps. Useful for multi-step problems; counterproductive when applied indiscriminately (see Anti-pattern 47). | Section 12a, 47 |
| **Context decay** | The gradual degradation of output quality as conversation length increases. Caused by attention dilution, error accumulation, and constraint drift. | Section 38 |
| **Epistemic checkpoint** | A required self-assessment step before the model commits to a confident answer. Surfaces uncertainty before linguistic momentum takes over. | Section 12a |
| **Extended thinking** | Model capabilities (reasoning traces, scratchpads, chain-of-thought) that allow internal deliberation before final output generation. Reduces but does not eliminate failure modes. | Section 1 |
| **Ground truth** | An authoritative fact or constraint that overrides model priors. Must be explicitly declared in the prompt. | Section 24 |
| **Hallucination** | Model output that is fluent and plausible but factually incorrect. A rational outcome of optimization under ambiguity, not a "bug." | Section 7 |
| **Incentive structure** | The implicit optimization target created by a prompt's structure, constraints, and framing. The core abstraction of this manifesto. | Section 6 |
| **Intake gate** | A controlled clarification step at the start of a task. Ask only what changes the answer; proceed with labeled assumptions otherwise. | Section 22 |
| **Invariant** | A hard constraint that must never be violated, regardless of other prompt instructions. | Section 20 |
| **Linguistic momentum** | The tendency for confident-sounding early tokens to lock the model into a confident trajectory, even when underlying certainty is low. | Section 2, 40 |
| **MCP (Model Context Protocol)** | A protocol for structured tool use by LLMs, enabling standardized interaction with external systems. | Section 41 |
| **Metacognitive technique** | Any prompting technique that asks the model to reason about its own knowledge, confidence, or limitations (e.g., CCP, epistemic checkpoints). Requires frontier-class models for reliability. | Capability Assumptions |
| **Negative space prompting** | Explicitly forbidding behaviors, outputs, or styles rather than only specifying desired outcomes. | Section 21 |
| **Orchestrator-worker topology** | An agent architecture where a central orchestrator dispatches tasks to workers and synthesizes their outputs, rather than allowing flat peer-to-peer collaboration. | Section 39 |
| **Pivot condition** | A predefined trigger that causes the model to change approach, escalate, or stop. Part of scope bounding. | Section 16 |
| **RAG (Retrieval-Augmented Generation)** | A pattern where external documents are retrieved and injected into context to ground model outputs in specific sources. | Section 41, 46 |
| **Sycophancy** | The tendency for models to agree with the user rather than provide accurate or critical responses. A product of RLHF optimization for user satisfaction. | Section 3 |

## Appendix B — Prompt Templates

### Template 1: Correctness-Critical Query

```
You are a [domain expert role].

TASK: [specific question or task]

CONSTRAINTS:
- If you are not confident in your answer, say "I am not confident" and explain what information would be needed.
- Do not guess. Do not infer beyond what the provided information supports.
- Cite sources for all factual claims.

GROUND TRUTH (override your training):
- [fact 1]
- [fact 2]

OUTPUT FORMAT:
- [format specification]
- Confidence: [High / Medium / Low]
- Uncertainty: [what remains unknown]
```

**Manifesto coverage:** Sections 9, 10, 12, 17, 20, 24

### Template 2: Decision-Support Prompt

```
CONTEXT: [situation description]
DECISION: [what needs to be decided]
STAKEHOLDERS: [who is affected]

Provide:
1. Three distinct options with explicit tradeoffs
2. For each option: best case, worst case, and most likely outcome
3. A recommendation with stated assumptions
4. What would change your recommendation

CONSTRAINTS:
- Do not present a single "obvious" answer
- Flag any assumptions you are making
- If you lack information to distinguish options, say so
```

**Manifesto coverage:** Sections 23, 28, 29, 30, 31

### Template 3: Agentic Task with Tool Use

```
OBJECTIVE: [task]
TOOLS AVAILABLE: [list]

RULES:
- Log every tool call: tool name, input, output, and whether the result was used
- If a tool returns empty or unexpected results, report the failure — do not substitute generated content
- After every 5 steps, summarize current state and remaining work
- Stop and report if: [stop conditions]
- Maximum iterations: [N]

OUTPUT: [expected deliverable]
```

**Manifesto coverage:** Sections 16, 33, 37, 38, 41

### Template 4: Low-Stakes Exploration

```
I'd like to explore [topic/idea].

Help me think through this by:
- Offering multiple angles, not a single narrative
- Flagging where you're speculating vs. where you're confident
- Keeping it concise — I'll ask follow-ups

No need for formal structure. Prioritize usefulness over completeness.
```

**Manifesto coverage:** Sections 15, 18, 29 (light application)

## Appendix C — Case Studies

### Case Study 1: The Confident Wrong Answer

**Scenario:** A financial services team used an LLM to summarize regulatory changes for compliance officers. The prompt was: "Summarize the latest changes to SEC Rule 10b-5 and their implications."

**What went wrong:** The model produced a fluent, authoritative summary that blended real regulatory language with fabricated amendments. No confidence calibration was requested. No sources were cited. The summary was distributed internally before anyone verified it.

**Root cause:** No permission to refuse (Section 9). No truth anchoring (Section 10). No citation requirement (Section 11). The prompt incentivized completeness over accuracy.

**Fix:** Added ground truth declaration with the actual regulatory text, required source citations, added an explicit "if you are unsure about any provision, flag it rather than summarizing it" instruction, and added confidence labels per claim.

### Case Study 2: The Infinite Refinement Loop

**Scenario:** A marketing team used an LLM to draft campaign copy. After the first draft, they entered a cycle of "make it punchier," "now make it more professional," "actually go back to the first version but keep the new ending." This continued for 12 iterations.

**What went wrong:** By iteration 8, the model was optimizing for the delta from the previous version, not the original brief. The final output satisfied none of the original requirements. Context decay (Section 38) had eroded the original constraints.

**Root cause:** No scope bounding (Section 16). No stop condition (Section 37). No reset logic (Section 38). Anti-pattern 48 (Infinite Iteration Without Reset).

**Fix:** Established a 3-iteration limit with mandatory reset. After iteration 3, the conversation resets: original brief is re-injected, and the best version so far is provided as a starting point — not the conversation history.

### Case Study 3: Silent Tool Failure in RAG

**Scenario:** A customer support agent used RAG to answer product questions. The retrieval pipeline occasionally returned no relevant documents, but the agent was not instructed to handle this case.

**What went wrong:** When retrieval failed, the model seamlessly generated plausible-sounding answers from its training data. These answers were often outdated or wrong for the specific product version. No one knew retrieval had failed because tool calls were not logged.

**Root cause:** Silent tool use (Anti-pattern 45). No discard rule (Section 41). No logging (Section 41).

**Fix:** Added mandatory tool logging. Added rule: "If no documents are retrieved or retrieved documents do not address the question, respond with 'I don't have current information on this — please contact support' rather than answering from general knowledge."

## Appendix D — Failure Postmortems

### Postmortem Template

Use this structure to analyze prompt failures:

```
INCIDENT: [one-line description]
DATE: [when discovered]
SEVERITY: [Low / Medium / High / Critical]
BLAST RADIUS: [who/what was affected]

WHAT HAPPENED:
[Factual description of the failure]

WHAT THE PROMPT SAID:
[Relevant prompt excerpt]

WHAT THE MODEL DID:
[Actual output behavior]

ROOT CAUSE:
[Which manifesto section was violated or missing]

CONTRIBUTING FACTORS:
[Other conditions that enabled the failure]

FIX APPLIED:
[What changed in the prompt or system]

VERIFICATION:
[How the fix was validated]

LESSONS:
[What generalizes beyond this specific case]
```

### Common Failure Patterns

| Failure | Typical Root Cause | Manifesto Section |
|---|---|---|
| Hallucinated facts in confident tone | No permission to refuse; no confidence calibration | 9, 12 |
| Outdated information presented as current | No temporal awareness; no knowledge cutoff flag | 11, 26 |
| Model agrees with user's incorrect premise | Sycophancy not constrained; no ground truth | 3, 24 |
| Tool-augmented answer contradicts retrieved data | Tool output not cited; linguistic generation overrode retrieval | 41, 45 |
| Output drifts from original objective after revisions | No reset logic; context decay | 37, 38, 48 |
| Model generates PII or sensitive data from training | No negative space constraints; no privacy invariant | 20, 21 |
| Agent loops indefinitely on failing subtask | No stop condition; no escalation rule | 37 |
| Confident answer to unanswerable question | No answerability check; no epistemic checkpoint | 12a, 13 |

## Appendix E — Reference Architectures

### Architecture 1: Single-Prompt with Guardrails

```
┌─────────────────────────────┐
│         User Input          │
└──────────────┬──────────────┘
               │
      ┌────────▼────────┐
      │   Input Filter   │  ← Reject malformed / adversarial input
      └────────┬────────┘
               │
      ┌────────▼────────┐
      │   System Prompt  │  ← Constraints, role, format, ground truth
      │   + User Query   │     (Sections 9, 15–21, 24)
      └────────┬────────┘
               │
      ┌────────▼────────┐
      │    LLM Call      │
      └────────┬────────┘
               │
      ┌────────▼────────┐
      │  Output Filter   │  ← Confidence check, format validation,
      └────────┬────────┘     PII scan (Sections 12, 20, 34)
               │
      ┌────────▼────────┐
      │   User Output    │
      └─────────────────┘
```

**When to use:** Low-to-medium stakes, human-in-the-loop, single-turn or short conversations.
**Manifesto parts:** I–V mandatory, VI–VII recommended.

### Architecture 2: RAG Pipeline with Verification

```
┌──────────┐    ┌──────────────┐    ┌──────────────┐
│  Query   │───▶│  Retriever   │───▶│  Retrieved   │
└──────────┘    │  (search,    │    │  Chunks      │
                │   vector DB) │    │  + metadata  │
                └──────────────┘    └──────┬───────┘
                                          │
                              ┌───────────▼───────────┐
                              │  Relevance Filter      │  ← Rank, prune
                              │  (Section 27)          │     irrelevant chunks
                              └───────────┬───────────┘
                                          │
                              ┌───────────▼───────────┐
                              │  LLM Generation        │  ← System prompt +
                              │  + Citation Requirement │     retrieved context
                              └───────────┬───────────┘    (Sections 24, 41)
                                          │
                              ┌───────────▼───────────┐
                              │  Citation Verification │  ← Every claim maps
                              │  (Section 41)          │     to a source chunk?
                              └───────────┬───────────┘
                                          │
                              ┌───────────▼───────────┐
                              │  Output + Sources      │
                              └───────────────────────┘
```

**When to use:** Correctness-critical, fact-dependent queries, knowledge-intensive domains.
**Manifesto parts:** I–V mandatory, VII mandatory.
**Key rule:** If no relevant chunks are retrieved, the system must refuse rather than fall back to parametric knowledge (Section 41).

### Architecture 3: Agentic Orchestrator-Worker

```
┌─────────────────────────────────────────────────┐
│                 ORCHESTRATOR                     │
│  ┌───────────────────────────────────────────┐   │
│  │  Task Plan + Constraint Registry          │   │
│  │  (Sections 16, 20, 37)                    │   │
│  └───────────────────┬───────────────────────┘   │
│                      │                           │
│    ┌─────────────────┼─────────────────┐         │
│    │                 │                 │         │
│    ▼                 ▼                 ▼         │
│ ┌──────┐       ┌──────────┐      ┌────────┐     │
│ │Worker│       │  Worker  │      │ Worker │     │
│ │  A   │       │    B     │      │   C    │     │
│ │(tool)│       │(research)│      │(write) │     │
│ └──┬───┘       └────┬─────┘      └───┬────┘     │
│    │                │                │          │
│    └────────────────┼────────────────┘          │
│                     ▼                           │
│  ┌───────────────────────────────────────────┐   │
│  │  State Synthesis + Conflict Resolution    │   │
│  │  (Section 39)                             │   │
│  └───────────────────┬───────────────────────┘   │
│                      │                           │
│  ┌───────────────────▼───────────────────────┐   │
│  │  Context Reset Check (Section 38)         │   │
│  │  Continue / Reset / Escalate / Stop       │   │
│  └───────────────────────────────────────────┘   │
└─────────────────────────────────────────────────┘
```

**When to use:** Multi-step tasks, tool-heavy workflows, autonomous agents.
**Manifesto parts:** All parts mandatory (I–VIII).
**Key rules:** Central synthesis required (Section 39). Tool calls logged (Section 41). Stop conditions enforced (Section 37). Context reset at decay signals (Section 38).
