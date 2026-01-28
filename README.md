# Agent Judgment Layer

**This project defines a non-bypassable judgment step before autonomous agent actions.**

---

## Why This Matters

Recent incidents (Clawdbot, autonomous coding agents) exposed a critical gap:

> **AI agents act before humans can judge.**

Current solutions focus on:
- Model safety (too early)
- Logging/monitoring (too late)

**This layer operates at execution time:**

```
[Agent proposes action] → [Judgment layer] → [Human approval if needed] → [Execution]
                              ↑
                         Decision point
```

---

## What's Included (Open Source)

This repository provides **the judgment skeleton**:

1. **[ACTION_TAXONOMY.md](ACTION_TAXONOMY.md)**
   - Universal classification: READ / WRITE / EXEC
   - Risk escalation rules
   - Special cases (CI/CD, package managers, IaC)

2. **[JUDGMENT_FORMAT.yaml](JUDGMENT_FORMAT.yaml)**
   - Standard trace format for judgment records
   - Decision states: `allow`, `block`, `escalate`
   - Approval chain structure
   - Audit metadata

**Use cases:**
- GitHub bot authors: Add judgment hooks before PR merges
- Agent framework builders: Integrate pre-execution checks
- Security teams: Define organization-specific escalation rules

---

## What's NOT Included (Commercial)

This repository **does not** include:
- Enforcement runtime (cannot be bypassed)
- Human-in-the-loop binding
- Responsibility sealing (tamper-proof approval records)
- Organization policy engines
- Compliance audit trails

**Why separate?**
- Open standards enable adoption
- Enforcement requires liability and support

If you need production-grade enforcement, contact [your-email].

---

## Quick Start

### For Agent Framework Authors

1. **Classify your agent's actions** using `ACTION_TAXONOMY.md`
2. **Emit judgment records** in `JUDGMENT_FORMAT.yaml` format
3. **Block EXEC actions** until approval is granted

Example pseudocode:

```python
def execute_agent_action(action):
    judgment = classify_action(action)  # Returns READ/WRITE/EXEC

    if judgment.type == "EXEC":
        approval = request_human_approval(action)
        if not approval.granted:
            raise ExecutionBlocked("Human approval required")

    record_judgment(judgment)  # Log in JUDGMENT_FORMAT
    return perform_action(action)
```

### For GitHub Bot Authors

Add a pre-merge hook:

```yaml
# .github/workflows/agent-judgment.yml
on: pull_request
jobs:
  check_agent_actions:
    runs-on: ubuntu-latest
    steps:
      - name: Validate judgment records
        run: |
          # Check if PR includes judgment traces
          # Block merge if high-risk actions lack approval
```

---

## Design Philosophy

**Three principles:**

1. **Judgment happens before execution, not after**
2. **Risk classification is universal (READ/WRITE/EXEC)**
3. **Standards are open, enforcement is not**

---

## Roadmap

- [x] Initial taxonomy and format (v1.0)
- [ ] Reference implementation (Python)
- [ ] GitHub Action for PR validation
- [ ] Integration examples for major agent frameworks
- [ ] Community feedback period (Q1 2026)

---

## Contributing

This is a **draft for community feedback**.

We welcome:
- Taxonomy refinements
- Real-world edge cases
- Format improvements
- Integration patterns

**Not accepting:**
- Enforcement implementations (see commercial note above)
- Framework-specific features (keep it universal)

Open an issue or PR to contribute.

---

## License

Apache 2.0 - See [LICENSE](LICENSE)

**Rationale:** Permissive license enables broad adoption in both open and commercial projects.

---

## Context

This project emerged from the January 2026 debate around autonomous agents (Clawdbot incident, execution safety discussions).

**Core insight:**
> "The problem isn't that AI is too powerful.
> The problem is that judgment happens after execution."

We're fixing the order.

---

## Contact

- **GitHub Issues:** For technical questions
- **Email:** [your-email] for commercial/partnership inquiries

---

**Status:** Draft (v1.0) | Seeking community feedback
