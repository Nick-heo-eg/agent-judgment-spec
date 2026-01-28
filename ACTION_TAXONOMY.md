# Action Taxonomy

**Purpose:** Define a universal classification system for autonomous agent actions.

Every action an AI agent takes falls into one of three categories, regardless of the underlying tool or API:

---

## Classification

### READ
**Definition:** Actions that observe state without modification.

**Characteristics:**
- State observation only
- No resource allocation
- No side effects
- Idempotent operations

**Examples:**
- File system reads
- Query operations
- Status checks
- Information retrieval

**Risk Level:** Low
**Judgment Required:** Minimal (privacy/access control only)

---

### WRITE
**Definition:** Actions that modify state but do not trigger execution.

**Characteristics:**
- State mutation
- Local scope only
- No immediate propagation
- Reversible through version control

**Examples:**
- File modifications
- Local data updates
- Configuration changes
- Staged commits (not published)

**Risk Level:** Medium
**Judgment Required:** High (irreversibility, scope of change)

---

### EXEC
**Definition:** Actions that trigger computation, side effects, or propagation.

**Characteristics:**
- State propagation to external systems
- Resource allocation and execution
- Side effects beyond local scope
- Irreversible without manual intervention

**Examples:**
- Publishing changes to shared systems
- Triggering automated pipelines
- Deploying to production
- Modifying execution definitions
- External communications
- Package installations with side effects

**Risk Level:** High
**Judgment Required:** Requires explicit judgment record

---

## Special Cases

### Compound Actions
Some operations cross classification boundaries:

| Pattern | Classification | Reason |
|---------|---------------|---------|
| Stage + Publish | EXEC | Publishing component triggers propagation |
| Package installation | EXEC | May execute arbitrary code during setup |
| Build artifact | WRITE | Creates output, doesn't execute |
| Run artifact | EXEC | Initiates execution context |

### Escalation Rules

1. **Modifying execution definitions → EXEC**
   - Pipeline configurations
   - Workflow specifications
   - Automation rules
   - Rationale: Defines future execution behavior

2. **Dependency manifest changes → EXEC**
   - Package declarations
   - Dependency lists
   - Library specifications
   - Rationale: Supply chain impact

3. **Infrastructure declarations → EXEC**
   - Resource provisioning
   - Service deployment
   - System configuration
   - Rationale: Real-world resource changes

---

## Design Principle

> **"If propagation occurs beyond the local context without explicit intervention, classify as EXEC."**

This taxonomy is intentionally simple to allow broad adoption across agent frameworks.

**Key test:** Can this action affect external parties or systems? If yes, it requires a judgment record with responsibility attribution.

---

## Versioning

- **Version:** 1.0
- **Status:** Draft for community feedback
- **License:** Apache 2.0
