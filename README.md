# agent-skills

39 reusable skills for [pi](https://github.com/badlogic/pi-mono) and other coding agents. Compatible with `npx skills add`.

## Install

```bash
npx skills add cygnusfear/agent-skills
```

Or clone and point your agent at the `skills/` directory:

```bash
git clone https://github.com/cygnusfear/agent-skills.git
```

For pi, add to `~/.pi/agent/settings.json`:

```json
{
  "skills": ["/path/to/agent-skills/skills"]
}
```

## Skills

### Analysis & review

| Skill | Description |
|-------|-------------|
| `architectural-analysis` | Find dead code, duplication, anti-patterns, and type issues across a codebase |
| `code-review` | Six-pass PR review covering runtime failures, consistency, architecture, and environment risks |
| `review-changes` | Review current git diff against related plans; flag bad or over-engineering |
| `receiving-code-review` | Process review feedback with technical rigor instead of blind agreement |
| `requesting-code-review` | Prepare and request review after completing features |
| `file-name-wizard` | Audit filename conventions against AGENTS.md standards |

### Planning & execution

| Skill | Description |
|-------|-------------|
| `brainstorming` | Explore intent, requirements, and design before writing code |
| `create-plan` | Build a structured implementation plan as a ticket |
| `writing-plans` | Turn specs or requirements into multi-step plans |
| `check-plan` | Audit progress against a plan; verify completed work and remaining tasks |
| `executing-plans` | Execute a written plan in a separate session with review checkpoints |
| `test-driven-development` | Write the test first, watch it fail, write minimal code, refactor |

### Parallel work

| Skill | Description |
|-------|-------------|
| `blitz` | Parallel multi-issue sprints using git worktrees and concurrent agents |
| `dispatching-parallel-agents` | Run 2+ independent tasks without shared state |
| `teams-driven-development` | Execute plan tasks with parallel workers in one session |
| `delphi` | Run multiple oracle investigations in parallel; synthesize divergent findings |

### Research & debugging

| Skill | Description |
|-------|-------------|
| `the-oracle` | Deep multi-source research combining codebase exploration and web search |
| `systematic-debugging` | Structured root-cause analysis for bugs and test failures |
| `ctx` | Build comprehensive project understanding at session start |
| `chrome-devtools` | Automate Chrome via DevTools — screenshots, JS evaluation, network inspection |
| `video-explorer` | Analyze video by extracting frames hierarchically |

### Design & documentation

| Skill | Description |
|-------|-------------|
| `design-spec-extraction` | Extract W3C DTCG-compliant JSON design specs from mockups or screenshots |
| `obsidian-plan-wiki` | Create and manage behavior specification wikis in Obsidian format |
| `obsidian-upgrade` | Migrate Obsidian wikis to the latest structure and comment format |
| `the-archivist` | Document engineering decisions as Architecture Decision Records |
| `update-docs` | Sync documentation with current codebase state |
| `writing-clearly-and-concisely` | Apply Strunk's rules to any prose humans read |

### Git & workflow

| Skill | Description |
|-------|-------------|
| `create-pr` | Create a PR with linked issues and closing keywords |
| `finishing-a-development-branch` | Choose how to integrate completed work — merge, PR, or cleanup |
| `using-git-worktrees` | Create isolated worktrees for feature branches |
| `verification-before-completion` | Run verification commands before claiming work is done |

### Skill & agent authoring

| Skill | Description |
|-------|-------------|
| `create-skill` | Create or update a skill with proper structure and frontmatter |
| `create-mcp-skill` | Create a skill that wraps an MCP server |
| `writing-skills` | Edit and verify skills before deployment |
| `create-worker` | Configure specialized worker agents for delegation |

### Internal

| Skill | Description |
|-------|-------------|
| `4-step-program` | Fix-review-iterate-present loop for delegated tasks |
| `ticket` | Persist skill output as tk tickets |
| `superpower-zustand` | Enforce StoreBuilder pattern for Zustand stores |
| `using-superpowers` | Guidelines for loading skills effectively |

## License

MIT
