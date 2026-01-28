# Action Taxonomy

**Purpose:** Define a universal classification system for autonomous agent actions.

Every action an AI agent takes falls into one of three categories, regardless of the underlying tool or API:

---

## Classification

### READ
**Definition:** Actions that observe state without modification.

**Examples:**
- `git status`
- `cat file.txt`
- API GET requests
- Database SELECT queries
- File system reads

**Risk Level:** Low
**Judgment Required:** Minimal (privacy/access control only)

---

### WRITE
**Definition:** Actions that modify state but do not trigger execution.

**Examples:**
- Edit source code
- Modify configuration files
- Create new files
- Database INSERT/UPDATE
- `git add`, `git commit`

**Risk Level:** Medium
**Judgment Required:** High (irreversibility, scope of change)

---

### EXEC
**Definition:** Actions that trigger computation, side effects, or propagation.

**Examples:**
- `git push`
- `npm install` (runs scripts)
- CI/CD pipeline triggers
- Production deployments
- `docker run`
- Sending emails/notifications
- Modifying `.github/workflows/`

**Risk Level:** High
**Judgment Required:** Mandatory (non-bypassable approval)

---

## Special Cases

### Compound Actions
Some operations cross boundaries:

| Action | Classification | Reason |
|--------|---------------|---------|
| `git commit -m "..." && git push` | EXEC | Push component triggers propagation |
| `npm install` | EXEC | Can run arbitrary scripts via lifecycle hooks |
| `docker build` | WRITE | Only creates image, doesn't run |
| `docker run` | EXEC | Executes container |

### Escalation Rules

1. **Any action touching CI/CD definitions → EXEC**
   - `.github/workflows/`, `.gitlab-ci.yml`, `Jenkinsfile`
   - Rationale: Defines future execution behavior

2. **Package manager operations → EXEC**
   - `package.json`, `Gemfile`, `requirements.txt` changes
   - Rationale: Supply chain risk

3. **Infrastructure as Code → EXEC**
   - `terraform apply`, `kubectl apply`
   - Rationale: Real-world resource changes

---

## Design Principle

> **"If it can reach production or external systems without another human step, it's EXEC."**

This taxonomy is intentionally simple to allow broad adoption across agent frameworks.

---

## Versioning

- **Version:** 1.0
- **Status:** Draft for community feedback
- **License:** Apache 2.0
