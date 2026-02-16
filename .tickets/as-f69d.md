---
id: as-f69d
status: in_progress
deps: []
links: []
created: 2026-02-16T10:31:50Z
type: task
priority: 1
assignee: Alexander Mangel
tags: [research, oracle, maintenance]
---
# Repo Maintenance Audit: totalrecall-rs — Full Findings & Recommendations

Comprehensive deep-dive audit of totalrecall-rs repo clutter, stale artifacts, and disk recovery opportunities. Every suspect file, directory, worktree, and branch investigated with evidence.



## Goal
Provide actionable cleanup plan for totalrecall-rs with evidence-backed classifications for every suspect item.

## Acceptance Criteria
- [ ] Every suspect item classified with evidence
- [ ] Disk recovery estimates provided
- [ ] Safe deletes vs human-decision items clearly separated

## Verification
- [ ] All items from the survey ticket investigated
- [ ] Git history and references checked for each item

## Worktree
- /Users/alexander/Projects/totalrecall-rs


---

# AUDIT FINDINGS

*Audit date: 2026-02-16 | Auditor: oracle-repo-audit*
*Target repo: /Users/alexander/Projects/totalrecall-rs*

## 1. IMMEDIATE SAFE DELETES

### 1.1 eval_results*.json (9 files, 4.1 MB total) — SAFE_DELETE
- **What**: Evaluation benchmark result JSON files from Dec 2025
- **Gitignored**: Yes (`eval_results*.json` in .gitignore)
- **Referenced by code**: No
- **Git tracked**: No
- **Evidence**: All from Dec 18-20 2025. One-off eval run outputs with iterative naming (baseline, fixed, hybrid, phase1, with_router). No code loads these.
- **Recovery**: 4.1 MB

### 1.2 docker-compose.yml.backup — SAFE_DELETE
- **What**: Backup of docker-compose.yml before some edit
- **Gitignored**: Yes (`*.backup` pattern)
- **Content**: Identical structure to current docker-compose.yml
- **Evidence**: Current docker-compose.yml is in git — any version recoverable from history.
- **Recovery**: 1.8 KB

### 1.3 test_keyring.rs — SAFE_DELETE
- **What**: One-off debug script testing macOS keyring access for Claude Code OAuth credentials
- **Gitignored**: Yes (`test_*.rs` pattern)
- **Referenced**: No
- **Evidence**: Self-contained main() function, not part of the build.
- **Recovery**: 1.4 KB

### 1.4 examples/ (empty directory) — SAFE_DELETE
- **What**: Empty directory, no files, no git history
- **Evidence**: Created Dec 17, never populated. Scaffolded and forgotten.
- **Recovery**: 0 bytes (reduces noise)

### 1.5 .worktrees/viewer-rewrite/ — SAFE_DELETE (3.5 GB)
- **What**: Git worktree for viewer-rewrite branch
- **Branch merged**: YES — confirmed via `git branch --merged main`
- **Uncommitted changes**: 12 files (608 ins / 2098 del) — viewer UI experiments never committed
- **Evidence**: Branch fully merged. Uncommitted changes are abandoned experiments.
- **WARNING**: Save patch first if desired: `cd .worktrees/viewer-rewrite && git diff > ~/viewer-rewrite-uncommitted.patch`
- **Recovery**: 3.5 GB (3.4 GB is target/)

### 1.6 /Users/alexander/Projects/totalrecall-locomo-eval/ — SAFE_DELETE (2.3 GB)
- **What**: External worktree on detached HEAD at ancient commit (ec1b7ba, ~Dec 16)
- **Status**: Detached HEAD, 10 modified files, 2+ months stale
- **Evidence**: The eval code has been heavily refactored since. Uncommitted changes would conflict massively with current main.
- **Recovery**: 2.3 GB (2.2 GB is target/)

### 1.7 Merged local branches (4) — SAFE_DELETE
- `viewer-rewrite` (4 weeks), `fix/skip-keyring-in-tests` (9 weeks), `feat/llm-judge-metrics` (9 weeks), `feature/llm-provider-abstraction` (9 weeks)
- **Evidence**: All in `git branch --merged main`. No open PRs.
- **Command**: `git branch -d viewer-rewrite fix/skip-keyring-in-tests feat/llm-judge-metrics feature/llm-provider-abstraction`

### 1.8 Merged remote branches (6) — SAFE_DELETE
- forgejo/main (mirror), forgejo/feat/worker-service-daemon, forgejo/fix/skip-keyring-in-tests, forgejo/feat/llm-judge-metrics, origin/feat/llm-judge-metrics, forgejo/feature/llm-provider-abstraction
- **Evidence**: All in `git branch -r --merged main`

### 1.9 Duplicate backup branch — SAFE_DELETE
- `backup/main_contaminated_20251217_092035` is BYTE-IDENTICAL to `backup/main_contam` (both at commit 8bcbdae)
- **Command**: `git branch -D backup/main_contaminated_20251217_092035`

---

## 2. ARCHIVE THEN DELETE

### 2.1 backups/ (6.2 GB) — ARCHIVE_THEN_DELETE
- **What**: PostgreSQL SQL dumps and one SQLite file
- **Contents**: Two 3.0 GB dumps (Jan 2026), five smaller (7-46 MB, Dec 2025), one SQLite (17 MB)
- **Git tracked**: No (gitignored). .gitkeep also NOT tracked.
- **Referenced**: No code references, BUT `justfile` backup target creates files here
- **gitignore comment**: "NEVER DELETE" — human deliberately marked this
- **Recommendation**: Compress the two 3 GB dumps (`gzip` → likely ~600 MB each). Move old Dec dumps to external storage. Keep latest only.
- **Recovery**: Up to 6.2 GB (but respect "NEVER DELETE" — get human confirmation)

### 2.2 docker/ directory (12 KB) — ARCHIVE_THEN_DELETE
- **What**: Early docker-compose setup from Phase 1 (Dec 15-17)
- **Contents**: docker-compose.yml, docker-compose.dev.yml, init.sql
- **Superseded by**: Root docker-compose.yml, docker-compose.full.yml, root Dockerfile (all newer)
- **Referenced**: No code or docs reference this directory
- **Evidence**: Historical scaffolding from initial development phase
- **Recovery**: 12 KB (trivial, but reduces confusion)

### 2.3 data/locomo10.json (2.7 MB) — ARCHIVE_THEN_DELETE
- **What**: LoCoMo evaluation dataset (10 conversations)
- **Gitignored**: Yes (`data/` pattern)
- **Referenced**: Zero references in codebase
- **Evidence**: Used during Dec 2025 eval development, now unused
- **Recovery**: 2.7 MB

---

## 3. NEEDS HUMAN DECISION

### 3.1 .worktrees/refactor-split-modules/ (5.8 GB) — ACTIVE WORK
- **Branch**: `refactor/split-modules` — NOT merged
- **5 unique commits**: Splits db/postgres.rs (3381→15 modules), mcp/tools.rs, synthesis/dreaming.rs
- **Uncommitted changes**: 68 files, 646 insertions, 997 deletions
- **Last activity**: TODAY (11 minutes before audit)
- **Options**: (A) Complete & merge, (B) Abandon, (C) Keep but clean target/ to save 5.8 GB

### 3.2 IMPLEMENTATION_PLAN.md — HISTORICAL
- **What**: Original Rust rewrite plan from day 1 (Dec 15)
- **Tracked**: Yes. Not referenced by any code or docs.
- **Options**: Move to docs/archive/ or delete (in git history forever)

### 3.3 config.toml vs config.template.toml — CONVENTION FIX NEEDED
- **Issue**: Both tracked, BYTE-FOR-BYTE IDENTICAL
- **Convention**: config.toml should be gitignored (local), config.template.toml tracked (template)
- **Options**: (A) Gitignore config.toml, keep template, (B) Remove template since identical

### 3.4 justfile vs Makefile — PICK ONE
- **Issue**: Two build systems with IDENTICAL targets (12 targets each, same names)
- **Both tracked**, neither referenced in docs
- **Options**: Keep just (modern) or make (universal). Delete the other.

### 3.5 Unmerged feature branches (per branch decision)

| Branch | Commits | Age | Summary | Likely Action |
|--------|---------|-----|---------|---------------|
| `feat/dream-rebuild-relationships` | 2 | 8w | Adds rebuild-relationships command | Rebase or abandon |
| `perf/parallelize-eval-reranking` | 3 | 8w | Parallelizes reranking API calls | Rebase or abandon |
| `feat/oauth-credential-provider` | 3 | 9w | OAuth credential provider | Rebase or abandon |
| `feature/sqlite-migration` | 18 | 9w | Massive SQLite migration | Likely OBSOLETE (repo is PG-only) |
| `backup/pr-116-original-20251218-042720` | 1 | 9w | PR #116 backup | Delete if PR merged |
| `backup/main_contam` | 1 | 9w | Contaminated main backup | Historical, likely safe to delete |

### 3.6 ~42 unmerged remote branches — BULK PRUNE
- All 8-9 weeks old, no open PRs
- Mix of experimental features and fixes from Dec 2025 burst
- **Recommendation**: Review list, delete dead-end experiments, keep valuable unmerged work

### 3.7 flake.nix — KEEP or DELETE
- **What**: Nix flake for dev environment (from initial commit)
- **Note**: NO flake.lock exists — never actually used (`nix develop` generates one)
- **Decision**: Keep if Nix users exist. Delete if nobody uses Nix.

### 3.8 Untracked/modified tickets (10 files)
- 7 untracked, 3 modified in .tickets/
- **Decision**: Commit or clean up

---

## 4. KEEP (with justification)

| Item | Why Keep |
|------|----------|
| **MIGRATION.md** | Referenced by README.md (lines 193, 321). Active user documentation. |
| **docker-compose.yml** | Primary dev workflow (PostgreSQL + eval profiles) |
| **docker-compose.full.yml** | Production-like deployment (includes app service) |
| **viewer/** | Active React/Vite web viewer, git-tracked, recently merged rewrite |
| **hooks/** | Claude Code hooks config (hooks.json) — active integration |
| **benches/** | Criterion benchmarks — referenced by Cargo.toml `[[bench]]` entries |
| **scripts/** | 10 eval/benchmark/test scripts — scan-magic-values.py updated Jan 22 |
| **Dockerfile** | Required for containerized deployment |

---

## 5. DISK RECOVERY ESTIMATE

| Item | Size | Classification |
|------|------|---------------|
| .worktrees/viewer-rewrite/ | 3.5 GB | SAFE_DELETE |
| /Projects/totalrecall-locomo-eval/ | 2.3 GB | SAFE_DELETE |
| eval_results*.json | 4.1 MB | SAFE_DELETE |
| docker-compose.yml.backup | 1.8 KB | SAFE_DELETE |
| test_keyring.rs | 1.4 KB | SAFE_DELETE |
| backups/ | 6.2 GB | HUMAN DECISION |
| .worktrees/refactor-split-modules/target/ | 5.8 GB | HUMAN DECISION |
| data/locomo10.json | 2.7 MB | ARCHIVE_THEN_DELETE |
| viewer/node_modules/ | 123 MB | Rebuildable (`bun install`) |

| Category | Space |
|----------|-------|
| **Immediate safe deletes** | **5.8 GB** |
| **With human approval (backups)** | **+6.2 GB** |
| **With human approval (refactor target/)** | **+5.8 GB** |
| **TOTAL POTENTIAL** | **~17.8 GB** |

---

## RECOMMENDED CLEANUP COMMANDS

```bash
# === SAFE DELETES ===
cd /Users/alexander/Projects/totalrecall-rs

# Files
rm eval_results*.json docker-compose.yml.backup test_keyring.rs
rmdir examples/

# Worktrees (save patches first if desired)
git worktree remove .worktrees/viewer-rewrite --force
git worktree remove /Users/alexander/Projects/totalrecall-locomo-eval --force

# Merged branches
git branch -d viewer-rewrite fix/skip-keyring-in-tests feat/llm-judge-metrics feature/llm-provider-abstraction
git branch -D backup/main_contaminated_20251217_092035

# === HUMAN CONFIRMATION NEEDED ===
# rm -rf .worktrees/refactor-split-modules/target/
# gzip backups/backup_20260121_*.sql
# rm -rf data/
# rm -rf docker/
# git branch -D feature/sqlite-migration backup/main_contam backup/pr-116-original-20251218-042720
```
