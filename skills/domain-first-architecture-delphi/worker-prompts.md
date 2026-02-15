# Worker Prompts — Domain-First Architecture Delphi

Use these prompts verbatim. Fill placeholders in `{braces}`.

---

## Worker 1 — Domain Ownership Mismatch

You are analyzing architectural ownership mismatch.

Scope: `{scope_paths}`
Primary domains: `{domains}`

Task:
1. For each domain, identify owner files for:
   - schema/table definitions
   - initialization/seed logic
   - runtime logic
   - debug/reducer logic
2. Flag domains where ownership is split across technical buckets with no deliberate boundary.
3. Produce **Architectural Mismatch Matrix** rows with evidence.

Output format:
- One row per domain:
  `Domain | Schema Owner | Init Owner | Runtime Owner | Debug Owner | Files touched for one change | Mismatch | Severity | Evidence`
- Include exact file + symbol + line ranges.

Reject vague findings.

---

## Worker 2 — Change-Coupling / Friction

You are analyzing change-coupling.

Scope: `{scope_paths}`
Scenarios:
- `{scenario_1}`
- `{scenario_2}`
- `{scenario_3}`

Task:
1. For each scenario, simulate implementation and list touched files + dirs.
2. Count cross-layer hops (schema ↔ init ↔ runtime ↔ debug).
3. Compute friction score:
   `files_touched + (dirs_touched * 2) + (cross_layer_hops * 3)`
4. Identify recurring bundles that always move together.

Output format:
- Scenario table with file list, dir list, hops, friction score
- Recurring coupling groups with evidence
- Severity suggestion per group

---

## Worker 3 — Init Dependency DAG (Hidden Flow)

You are extracting initialization dependencies.

Scope: `{init_files}`

Task:
1. Build explicit DAG for init/seed flow.
2. For every edge, include payload (what data is passed).
3. Mark hidden dependencies currently encoded implicitly in long functions.
4. Label hidden edges as `HIDDEN_DAG`.

Output format:
- DAG edges in text form:
  `producer_fn -> payload -> consumer_fn`
- Hidden dependency list with file/symbol/line evidence
- Suggested function boundaries to expose edges clearly

---

## Worker 4 — False Constraint Audit

You are auditing architectural assumptions.

Scope: `{scope_paths}`
Framework constraints: `{framework_constraints}`

Task:
1. List assumptions currently driving layout decisions.
2. Validate each assumption with concrete evidence from framework behavior/docs/code.
3. Split into:
   - Real constraints
   - False constraints (myths)
4. For each false constraint, propose safer structure unlocked by disproving it.

Output format:
- Assumption table:
  `Assumption | Real/False | Evidence | Refactor unlocked`
- Confidence per assumption

---

## Worker 5 — Safe Seam Extraction Plan

You are generating execution seams.

Scope: `{scope_paths}`
Input findings: Concept inventory + mismatch matrix + DAG

Task:
1. Identify move-only seams (no behavior change).
2. For each seam produce Replace→Wire→Delete map:
   - Replace: what old symbols/functions are superseded
   - Wire: exactly which callers switch first
   - Delete: what is removed only after verification
3. Order seams from lowest blast radius to highest.

Output format:
- Ordered seam plan (S1, S2, S3...)
- For each seam:
  - Goal
  - Files
  - Replace→Wire→Delete
  - Verification commands
  - Risk

No monolithic rewrites.

---

## Synthesis Prompt

You are the synthesis worker for domain-first architecture delphi.

Input: outputs from workers 1–5.

Produce:
1. Concept Inventory (canonical vs alternates)
2. Architectural Mismatch Matrix
3. Init Dependency DAG (payload edges)
4. Prioritized P0/P1 findings with scores
5. Replace→Wire→Delete plan per finding
6. Ticket-ready decomposition (epic + child tasks)

Rules:
- Drop findings without evidence.
- Drop findings without clear seam.
- Prefer smallest safe move first.
- Call out unresolved decisions explicitly.
