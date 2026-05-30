# Design Principles & Engineering Philosophy

> This document explains the architectural thinking and operating principles behind AI Data Cleaner. It is the public-facing counterpart to the internal agent specification — focused on the *why* rather than the *how*.

---

## Product Goal

Build a data cleaning tool that is:
- **Safe** — the original data is never at risk
- **Transparent** — every action is explainable and documented
- **Deterministic** — cleaning outcomes are predictable and reproducible
- **Measurable** — quality can be quantified, not just observed

The target user is a junior-to-mid data professional who needs confidence that their dataset has been cleaned correctly, with an audit trail to support that claim.

---

## Core Architectural Decisions

### Decision 1: Immutable Original Dataset

The uploaded dataset is stored in a protected state immediately upon upload. All subsequent operations — analysis, cleaning, filtering — operate on a separate working copy.

**Why:** Data professionals need to compare before/after states, catch overcorrection, and have a rollback point. Making immutability a hard constraint (not a convention) removes an entire class of accidental data loss bugs.

### Decision 2: AI as Analyst, Not Executor

The language model (Azure OpenAI) plays a single role: analyzing the dataset and producing structured recommendations. It does not execute any transformations.

**Why:** LLMs introduce variability. For data cleaning, determinism is non-negotiable — the same dirty value must be cleaned the same way every time. This architecture gives you the intelligence of AI with the reliability of classical code.

### Decision 3: Structured Error Taxonomy

Rather than treating all data issues as a flat list, the system classifies errors into three tiers (Type 1, 2, 3) based on cleaning complexity and confidence requirements.

**Why:** A single strategy cannot safely handle both "trailing whitespace" and "partial duplicate with business-rule ambiguity." The taxonomy makes the cleaning strategy explicit and auditable, and it gives users a shared vocabulary for discussing data quality.

### Decision 4: Calibration as First-Class Citizen

The system supports loading a ground-truth specification against which detected and cleaned results are compared quantitatively.

**Why:** Without a measurement baseline, "the data is cleaner" is subjective. With calibration, you get detection coverage rates, over-detection warnings, and per-error-type cleaning efficiency — making quality a number, not an opinion.

### Decision 5: Default / Advanced Mode Split

The UI defaults to a simplified, opinionated mode with safe presets. Users who need granular control can switch to Advanced mode.

**Why:** Junior users need guardrails; power users need flexibility. Forcing a single mode serves neither. The split also ensures that the most common path (safe defaults) is the path of least resistance.

---

## Operating Priorities

When making product decisions, the following priority order applies (highest to lowest):

1. **Stability** — the application must be reliably usable
2. **Security** — no credentials leak, no unauthorized data access
3. **Dataset Safety** — the original data is never at risk
4. **Correctness** — cleaning results must be accurate
5. **User Experience** — the workflow must be clear and efficient
6. **Feature completeness** — new capabilities are added incrementally

This order means: a new feature that introduces instability is not shipped. A UX improvement that risks dataset safety is not implemented.

---

## Development Methodology

### Part-by-Part Progression

The project is built in discrete, explicitly scoped parts. Each part:
- Has a defined scope before implementation begins
- Is implemented and tested before moving to the next
- Is logged in the version history with files changed
- Has its bugs tracked in a dedicated bug log

This prevents scope creep and ensures a stable baseline between iterations.

### Approval Gates

No part is implemented without explicit approval of the scope. This applies even when working with AI-assisted development — the developer remains in control of what gets built and when.

### Testing Protocol

After each change: run the application, validate the golden path, check for regressions in previously working features. Type checking and linting verify syntax; only running the app verifies behavior.

---

## Security Posture

- No credentials in source code — all secrets in `.env`
- `.env` listed in `.gitignore` — never committed
- AI assistant operates with no write access to the dataset
- No server-side data persistence — sessions are local
- No external logging of user data

---

## What This Document Is Not

This document describes architectural philosophy and operating principles. It does not describe:
- The specific prompts or instructions used by the AI components
- The internal session state management
- The exact formulas used in calibration scoring
- The implementation details of individual cleaning methods

Those belong in the internal technical specification.
