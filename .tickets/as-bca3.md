---
id: as-bca3
status: closed
deps: []
links: []
created: 2026-02-14T11:51:12Z
type: task
priority: 2
assignee: fix-output-dirs
tags: [team]
---
# Work in /Users/alexander/Projects/agent-skills

Fix 4 skills that write output to bespoke directories (.reviews/, .audits/) instead of using tk tickets.

For each skill, replace ALL references to custom output directories with tk ticket creation. The output (review report, audit report) should be written as the body of a tk ticket, not a file in a custom directory.

Skills to fix:

1. `skills/code-review/SKILL.md` — uses `.reviews/{timestamp}/` directories. Replace with: create tk ticket tagged `review` with the review content. For parallel mode, each reviewer writes a tk ticket, then the synthesis goes in a parent ticket.

2. `skills/review-changes/SKILL.md` — uses `.reviews/code-review-{timestamp}.md`. Replace with: create tk ticket tagged `review`.

3. `skills/architectural-analysis/SKILL.md` — uses `.audits/architectural-analysis-{timestamp}.md`. Replace with: create tk ticket tagged `audit`.

4. `skills/file-name-wizard/SKILL.md` — uses `.audits/naming-audit-{timestamp}.md`. Replace with: create tk ticket tagged `audit`.

Also fix `skills/update-docs/SKILL.md` description — remove reference to `.plans` and `.tasks`, just say 'docs and AGENTS.md files'.

The principle: ALL agent output goes into tk tickets. No custom directories. The `skills/ticket/SKILL.md` skill already documents this rule.

Do NOT change any other behavior in these skills — only the output destination.

After fixing, run: rg -l '\.(audits|reviews|postmortems|reports|findings|oracle|plans)/' --glob '*.md' skills/ | grep -v ticket/
This should return nothing (or only legitimate references like GitHub PR reviews in blitz).

Work in /Users/alexander/Projects/agent-skills

Fix 4 skills that write output to bespoke directories (.reviews/, .audits/) instead of using tk tickets.

For each skill, replace ALL references to custom output directories with tk ticket creation. The output (review report, audit report) should be written as the body of a tk ticket, not a file in a custom directory.

Skills to fix:

1. `skills/code-review/SKILL.md` — uses `.reviews/{timestamp}/` directories. Replace with: create tk ticket tagged `review` with the review content. For parallel mode, each reviewer writes a tk ticket, then the synthesis goes in a parent ticket.

2. `skills/review-changes/SKILL.md` — uses `.reviews/code-review-{timestamp}.md`. Replace with: create tk ticket tagged `review`.

3. `skills/architectural-analysis/SKILL.md` — uses `.audits/architectural-analysis-{timestamp}.md`. Replace with: create tk ticket tagged `audit`.

4. `skills/file-name-wizard/SKILL.md` — uses `.audits/naming-audit-{timestamp}.md`. Replace with: create tk ticket tagged `audit`.

Also fix `skills/update-docs/SKILL.md` description — remove reference to `.plans` and `.tasks`, just say 'docs and AGENTS.md files'.

The principle: ALL agent output goes into tk tickets. No custom directories. The `skills/ticket/SKILL.md` skill already documents this rule.

Do NOT change any other behavior in these skills — only the output destination.

After fixing, run: rg -l '\.(audits|reviews|postmortems|reports|findings|oracle|plans)/' --glob '*.md' skills/ | grep -v ticket/
This should return nothing (or only legitimate references like GitHub PR reviews in blitz).


## Notes

**2026-02-14T11:51:40Z**

Starting work. Read all 5 skill files. Now applying changes to replace bespoke output directories with tk ticket creation.

**2026-02-14T11:53:17Z**

DONE: Fixed all 5 skill files. Replaced .reviews/ dirs in code-review and review-changes with tk tickets tagged 'review'. Replaced .audits/ dirs in architectural-analysis and file-name-wizard with tk tickets tagged 'audit'. Updated update-docs description and removed .tasks reference, replaced .docs-updates output with tk ticket tagged 'docs'. Verification: rg for bespoke dirs returns no matches (only legitimate GitHub PR review refs in blitz).

**2026-02-14T11:53:23Z**

DONE: Task completed.
