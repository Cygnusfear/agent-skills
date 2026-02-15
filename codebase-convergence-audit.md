# Codebase Convergence Audit

## The Problem

LLM-generated codebases accumulate **divergent implementations** of the same feature. Each session an agent works in, it solves a problem locally without checking whether that problem was already solved elsewhere. Over time you get 3 functions that build the same Discord message, 4 error handlers that format errors slightly differently, 2 config parsers that read the same env vars.

This is not normal code duplication (copy-paste of utility functions). This is **semantic duplication** — the same *intent* implemented through different *code paths* that drift apart over time. A bug fix in one path doesn't reach the others. Formatting changes in one path leave the others stale. Eventually you can't tell which path is canonical.

### Why LLMs Create This

1. **No project memory** — each session starts fresh. The agent doesn't know a relogin message builder already exists in `worker-common`.
2. **Local problem solving** — agents solve the problem in front of them. If RAMRAM needs to post a relogin message, it builds one inline. It doesn't search for an existing shared function first.
3. **Different authors** — different sessions write different code for the same thing. One uses markdown hyperlinks, another dumps raw URLs. Both "work."
4. **No architectural enforcement** — nothing stops an agent from creating a fourth implementation. AGENTS.md says "don't duplicate" but doesn't say *what already exists*.

### What It Costs

- **Silent regression** — you fix the message format in path A, paths B and C still show the old format.
- **Behavior inconsistency** — auto-triggered relogin shows a hyperlink, manual `/relogin` shows a raw URL. Same feature, different UX.
- **Maintenance multiplication** — every change requires finding and updating N paths instead of 1.
- **Debugging confusion** — "which code path generated this message?" becomes a real question.

---

## The Audit Process

### What to Look For

The audit hunts for **convergence failures** — places where multiple code paths implement the same semantic intent.

| Signal | What It Means |
|--------|---------------|
| Same emoji/keyword in multiple files | Different paths generating the same user-facing message |
| Same API endpoint called from multiple services | Multiple implementations of the same integration |
| Same struct/type defined in multiple crates | Shared concept without shared definition |
| Same error message formatted in multiple places | Divergent error surfacing |
| Same config env var read in multiple places | Config parsing duplication |
| `fn` names that are synonyms across files | `post_message` / `send_message` / `deliver_message` doing the same thing |
| Raw Discord/Telegram/HTTP calls in business logic | Integration code that should be in a shared layer |

### Audit Categories

**1. User-Facing Messages** (HIGHEST PRIORITY)
Messages the human sees — Discord posts, error messages, status updates. These MUST be single-source because:
- The human notices inconsistency immediately
- Formatting/wording changes need to hit all paths
- Buttons/components must use matching custom_ids

**Search method:** Grep for distinctive markers — emoji, bold markdown, specific phrases.
```bash
rg '🔑|🚦|⚠️|✅.*auth|❌.*failed' services/ --glob '*.rs'
rg 'RAMRAM.*paused|auto-paused|tick failed' services/ --glob '*.rs'
```

**2. API Integration Points**
Calls to external services (Discord REST, Anthropic, pi-host, gateway). Each integration point should have ONE wrapper.

**Search method:** Grep for raw HTTP calls to known services.
```bash
rg 'discord.com/api|channels/.*/messages' services/ --glob '*.rs'
rg '/v1/pi/auth|/v1/ramram|/v1/sessions' services/ --glob '*.rs'
```

**3. Error Handling Patterns**
How errors get surfaced, formatted, deduplicated, and delivered. The error pipeline should be uniform.

**Search method:** Grep for error formatting patterns.
```bash
rg 'channel_error|surface_error|post.*error.*discord' services/ --glob '*.rs'
rg 'format!.*error|format!.*failed|format!.*⚠' services/ --glob '*.rs'
```

**4. Config/Environment Reading**
Same env var read and parsed in multiple places, potentially with different defaults.

**Search method:** Grep for env var reads.
```bash
rg 'std::env::var\(' services/ --glob '*.rs' | sort | uniq -c | sort -rn
rg 'DISCORD_BOT_TOKEN|PROVIDER_BASE_URL|PI_HOST' services/ --glob '*.rs'
```

**5. Shared Data Types**
Same struct/enum defined in multiple crates when it should live in `common/`.

**Search method:** Grep for struct definitions with similar names.
```bash
rg 'pub struct|pub enum' services/ --glob '*.rs' | awk -F: '{print $NF}' | sort | uniq -c | sort -rn
```

---

## How to Address Agents in a Delphi Audit

### The Prompt Structure

The Delphi oracles need to be pointed at **signals, not conclusions**. You're asking them to find convergence failures they can prove with evidence.

**Key principles for the oracle prompt:**

1. **Define convergence failure precisely** — "multiple code paths that implement the same user-visible behavior or system integration" — not "find duplication" (which sounds like copy-paste detection).

2. **Give them concrete search techniques** — the grep patterns above. Oracles with specific search strategies find more than oracles told to "look around."

3. **Require evidence format** — each finding must include: the files, the line numbers, what the code does, and WHY it's a convergence failure (not just "these look similar").

4. **Separate the categories** — don't ask one oracle to find everything. Give each oracle the same full prompt but expect them to naturally diverge into different categories based on what they find first.

5. **Demand the canonical answer** — for each finding, the oracle must propose WHERE the single implementation should live and what the shared interface looks like.

### What NOT to Do

- **Don't say "find duplication"** — agents will grep for identical lines, which misses semantic duplication entirely.
- **Don't list specific files to check** — let oracles discover the scope. Pointing them at `relogin.rs` means they won't look at `trigger_relogin_flow`.
- **Don't describe the relogin problem** — that's the example that PROMPTED this audit. If you prime with it, oracles will find relogin-like patterns and miss everything else.
- **Don't ask for "code smells"** — too vague. "Convergence failure" is specific and actionable.

### Oracle Output Format

Each oracle should produce a structured list:

```
## Convergence Failure: [Name]

**Intent:** What the code is trying to do (e.g., "post an auth re-login prompt to Discord")

**Paths found:**
1. `file:line` — [what this path does, how it differs]
2. `file:line` — [what this path does, how it differs]
3. `file:line` — [what this path does, how it differs]

**Behavioral differences:** [Where the paths produce different results]

**Canonical location:** [Where the single implementation should live]

**Shared interface:**
```rust
// What the unified function signature should look like
pub fn do_the_thing(param: Type) -> Result
```

**Migration effort:** [trivial / moderate / significant]
```

---

## After the Audit: Prevention

Finding existing convergence failures is half the work. Preventing new ones requires two mechanisms:

### 1. Canonical Path Registry

A file (`docs/reference/canonical-paths.md` or equivalent) that lists every shared capability and where it lives. Agents read this before writing new code.

```markdown
# Canonical Paths

| Capability | Module | Function | Notes |
|-----------|--------|----------|-------|
| Relogin messages | `worker-common::relogin` | `build_relogin_message()` | ALL relogin messages |
| Discord error surfacing | `worker-common::discord_errors` | `DiscordErrorSurface` | ALL error→Discord |
| RAMRAM session cache | `worker-common` | `RamramSessionCache` | Session ID resolution |
| Discord channel posting | ??? | ??? | Currently divergent |
```

The "???" entries are findings from the audit — things that SHOULD be canonical but aren't yet.

### 2. `@canonical` Code Markers

In the shared function itself, a doc comment that agents will see when reading the file:

```rust
/// @canonical: relogin-message
/// ALL relogin/re-auth messages in the system MUST use this function.
/// Do NOT build relogin messages elsewhere. If you need a new variant,
/// add a parameter here.
pub fn build_relogin_message(...) -> ReloginMessage {
```

The `@canonical` tag is grep-able:
```bash
rg '@canonical' services/
```

### 3. AGENTS.md Integration

Each service's `AGENTS.md` should list what shared capabilities it uses and must NOT reimplement:

```markdown
## Shared Capabilities (DO NOT REIMPLEMENT)
- Relogin messages: `bhaktiram_worker_common::relogin`
- Error surfacing: `bhaktiram_worker_common::DiscordErrorSurface`
- Session cache: `bhaktiram_worker_common::RamramSessionCache`
```

---

## Audit Cadence

This isn't a one-time thing. Every few weeks of active LLM development, run a convergence audit. The codebase drifts faster than you think.

Quick smoke test (5 minutes):
```bash
# User-facing messages — look for emoji/markdown that appears in multiple services
rg '🔑|🚦|⚠️|✅|❌' services/ --glob '*.rs' -l | sort | uniq -c | sort -rn

# Raw Discord API calls outside of shared modules
rg 'discord.com/api' services/ --glob '*.rs' -l | grep -v 'common/'

# Same env var read in multiple crates
rg 'std::env::var\("' services/ --glob '*.rs' | sed 's/.*var("//' | sed 's/".*//' | sort | uniq -c | sort -rn | head -20
```

If the smoke test shows new divergence, run the full Delphi audit.
