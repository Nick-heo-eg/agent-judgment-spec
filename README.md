# Agent Judgment Layer

**This project defines how judgment must be recorded before autonomous agent actions occur.**

---

## What This Is

Autonomous agents require a structural answer to a simple question:

> **When must an action wait for human judgment?**

This project does not enforce decisions. It defines:
- How actions are classified
- How decisions are recorded
- Who bears responsibility

**The judgment interface:**

```
[Agent proposes action] → [Judgment record created] → [Decision logged] → [Action proceeds or waits]
                              ↑
                      Definition boundary
```

This is not a security tool. It is a **responsibility interface**.

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
- Runtime implementation
- Responsibility binding mechanisms
- Tamper-proof sealing
- Organization policy engines
- Compliance audit systems

**Why separate?**
- Open standards enable adoption
- Implementation requires liability and support

If you need production implementation, contact [your-email].

---

## Quick Start

### For Agent Framework Authors

1. **Classify your agent's actions** using `ACTION_TAXONOMY.md`
2. **Emit judgment records** in `JUDGMENT_FORMAT.yaml` format
3. **Record responsibility** before action execution

Example pseudocode:

```python
def execute_agent_action(action):
    judgment = classify_action(action)  # Returns READ/WRITE/EXEC

    # Create judgment record
    record = create_judgment_record(
        action=action,
        classification=judgment,
        responsibility=determine_accountability(action)
    )

    # Your framework decides what to do with this record
    # (enforcement is framework-specific, not defined here)
    return record
```

### For Integration Authors

Validate judgment records in your workflow:

```yaml
# Example: CI validation
on: pull_request
jobs:
  validate_judgment:
    runs-on: ubuntu-latest
    steps:
      - name: Check judgment records exist
        run: |
          # Verify PR includes judgment traces
          # Validate responsibility attribution
          # How you use this is framework-specific
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

This project emerged from the January 2026 discussion on autonomous agent execution patterns.

**Core principle:**
> "Accountability requires a record of judgment, not just a log of actions."

This defines the record format.

---

## Contact

- **GitHub Issues:** For technical questions
- **Email:** [your-email] for commercial/partnership inquiries

---

**Status:** Draft (v1.0) | Seeking community feedback
