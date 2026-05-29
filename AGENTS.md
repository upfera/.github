# AGENTS.md — Upfera AI Agent Instructions

This file defines how AI agents (e.g. Junie) must operate in this repository.

The **README.md is the single source of truth for architecture and system design.**

---

# 🧠 1. Primary Rule

> Always read and follow README.md before taking any action.

If anything is unclear:
→ treat README.md as authoritative
→ do NOT assume or invent architecture

---

# 📚 2. Knowledge Source Hierarchy

Agents must use sources in this order:

1. README.md (architecture + system model)
2. Repository code (implementation reality)
3. External context (ONLY if explicitly requested)

---

# ✏️ 3. Documentation Rule

If an agent performs a change that affects system behavior:

- MUST update README.md ONLY if repository behavior or system reality changes
- MUST keep README consistent with reality
- MUST NOT create separate competing documentation sources

---

# ⚙️ 4. Execution Rules

Agents must:

- follow existing repository boundaries
- avoid structural refactoring unless explicitly requested
- prefer minimal diffs
- maintain idempotent behavior where applicable
- respect realm-based architecture (do not flatten or simplify it)

---

# 🚫 5. Forbidden Actions

Agents must NOT:

- redefine architecture
- rename repositories or layers
- introduce new conceptual layers
- bypass realm definitions
- duplicate documentation that already exists in README

---

# 🧭 6. Decision Flow

When executing a task:

1. Read README.md
2. Identify impacted layer (platform / realm / runtime)
3. Apply minimal change
4. Update README.md if behavior changed
5. Stop

---

# 🧠 7. Mental Model

- README = truth
- Code = implementation
- Agent = executor, not designer

---

# 🔁 8. Key Principle

> If README and reality differ — update README, not the architecture.

---