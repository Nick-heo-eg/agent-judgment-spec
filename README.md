# Agent Judgment Layer

**This project defines how judgment must be recorded before autonomous agent actions occur.**

**This specification is intentionally non-executable.**

---

## What This Is

**This specification ensures that when autonomous agents act, they are never the sole decision-maker.**

Autonomous agents require a structural answer to a simple question:

> **When must judgment authority shift from agent to human?**

This project does not enforce decisions. It defines:
- How actions are classified by external impact
- How judgment authority is transferred
- Who bears responsibility

**The structural difference:**

```
WITHOUT this layer:
[Agent] → [Action] → [Execution] → [Log]
                                     ↑
                          Responsibility inferred after

WITH this layer:
[Agent] → [Action] → [Judgment Record] → [Authority Transfer] → [Decision]
                            ↑
                  Responsibility assigned before
                  Judgment authority can shift to human/org
```

**This is not about stopping actions. It's about switching decision authority.**

When an action requires human judgment:
- The agent doesn't hit a wall
- Judgment authority transfers to the appropriate party
- Context is preserved for the next decision-maker
- Choice becomes possible

This is not a security tool. It is a **judgment authority transfer protocol**.

---

## What's Included (Open Source)

This repository provides **the judgment skeleton**:

1. **[ACTION_TAXONOMY.md](ACTION_TAXONOMY.md)**
   - Universal classification: READ / WRITE / EXEC
   - Risk escalation rules
   - Special cases (CI/CD, package managers, IaC)

2. **[JUDGMENT_FORMAT.yaml](JUDGMENT_FORMAT.yaml)**
   - Standard trace format for judgment records
   - Authority transfer states: `allow`, `escalate`, `block`
   - Responsibility attribution structure
   - Context preservation for next decision-maker

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

If you need production implementation, contact egoholdings.response@gmail.com.

---

## Quick Start

### For Agent Framework Authors

1. **Classify your agent's actions** using `ACTION_TAXONOMY.md`
2. **Create judgment records** in `JUDGMENT_FORMAT.yaml` format
3. **Transfer authority** when escalation is triggered

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

    # If escalation is triggered, judgment authority transfers
    if record.escalation.triggered:
        # Framework transfers decision to target_authority
        # Context is preserved in the judgment record
        return transfer_authority(record)

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

## Why Logs Are Not Enough

Logs record what happened.
**Judgment records explain why it was allowed to happen.**

### The Problem with Post-Execution Logs

Without a judgment record:
- **Responsibility is inferred** (reconstructed from logs after the fact)
- **Authority is ambiguous** (who had the right to approve this?)
- **Context is lost** (why did this seem reasonable at the time?)
- **Choice is eliminated** (action already executed)

### What Judgment Records Provide

With a judgment record created before execution:
- **Responsibility is assigned** (explicit accountability)
- **Authority is transferred** (clear decision-maker for each action type)
- **Context is preserved** (rationale, policy basis, risk assessment)
- **Choice is enabled** (human can decide whether to proceed)

**The difference:**
Logs are for audit.
Judgment records are for governance.

This specification defines the structure that makes governance possible.

---

## Design Philosophy

**Core principle:**
> When autonomous agents propose high-impact actions, judgment authority must be transferable.

**Three mechanisms:**

1. **Classification by external impact** (READ/WRITE/EXEC)
   - Not by agent capability, but by action consequences

2. **Judgment authority transfer**
   - When EXEC actions are proposed, authority can shift to human/org
   - The record preserves context for the next decision-maker

3. **Responsibility before execution**
   - Accountability is assigned before action, not reconstructed after
   - Standards are open, implementation is not

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
- **Email:** egoholdings.response@gmail.com for commercial/partnership inquiries

---

**Status:** Draft (v1.0) | Seeking community feedback
