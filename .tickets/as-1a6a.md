---
id: as-1a6a
status: closed
deps: []
links: []
created: 2026-02-14T07:58:04Z
type: task
priority: 2
assignee: update-references
tags: [team]
---
# Update all references from 'handbook' to 'playbook' and the new SKILL.md directory format in the obsidian-plan-wiki skill.

Work in: /Users/alexander/.pi/agent/skills/obsidian-plan-wiki/

IMPORTANT: Wait for the convert-entries worker to finish before starting. But you can prepare by reading all the files first.

Files to update:
1. `SKILL.md` (main skill file) — update:
   - 'handbook' → 'playbook' everywhere
   - Lookup logic: `docs/handbook/**/20.01-*.md` → `docs/playbook/**/20.01-*/SKILL.md`
   - Skill fallback: `handbook/20-git/20.01-*.md` → `playbook/20-git/20.01-*/SKILL.md`
   - Directory structure diagram: `handbook/` → `playbook/`
   - Table paths: `handbook/10-docs/10.01-open-questions-system.md` → `playbook/10-docs/10.01-open-questions-system/SKILL.md`
   - Section title: 'Handbook' → 'Playbook'
   - The Johnny Lookup section needs to reflect new paths

2. `references/core-principles.md` — handbook → playbook, update path patterns
3. `references/templates.md` — handbook → playbook, update path patterns  
4. `references/claude-template.md` — handbook → playbook, update path patterns
5. `references/workflows.md` — handbook → playbook, update path patterns
6. `references/obsidian-open-questions-system.md` — handbook → playbook, update path patterns

Key changes:
- 'handbook' → 'playbook' as directory/concept name
- File paths change from `NN.NN-slug.md` to `NN.NN-slug/SKILL.md`
- Lookup pattern: `docs/playbook/**/NN.NN-*/SKILL.md`
- The concept description should mention that each entry is a SKILL.md (standard format understood by LLM agents)

Update all references from 'handbook' to 'playbook' and the new SKILL.md directory format in the obsidian-plan-wiki skill.

Work in: /Users/alexander/.pi/agent/skills/obsidian-plan-wiki/

IMPORTANT: Wait for the convert-entries worker to finish before starting. But you can prepare by reading all the files first.

Files to update:
1. `SKILL.md` (main skill file) — update:
   - 'handbook' → 'playbook' everywhere
   - Lookup logic: `docs/handbook/**/20.01-*.md` → `docs/playbook/**/20.01-*/SKILL.md`
   - Skill fallback: `handbook/20-git/20.01-*.md` → `playbook/20-git/20.01-*/SKILL.md`
   - Directory structure diagram: `handbook/` → `playbook/`
   - Table paths: `handbook/10-docs/10.01-open-questions-system.md` → `playbook/10-docs/10.01-open-questions-system/SKILL.md`
   - Section title: 'Handbook' → 'Playbook'
   - The Johnny Lookup section needs to reflect new paths

2. `references/core-principles.md` — handbook → playbook, update path patterns
3. `references/templates.md` — handbook → playbook, update path patterns  
4. `references/claude-template.md` — handbook → playbook, update path patterns
5. `references/workflows.md` — handbook → playbook, update path patterns
6. `references/obsidian-open-questions-system.md` — handbook → playbook, update path patterns

Key changes:
- 'handbook' → 'playbook' as directory/concept name
- File paths change from `NN.NN-slug.md` to `NN.NN-slug/SKILL.md`
- Lookup pattern: `docs/playbook/**/NN.NN-*/SKILL.md`
- The concept description should mention that each entry is a SKILL.md (standard format understood by LLM agents)


## Notes

**2026-02-14T07:58:17Z**

Starting work. Reading all files first to prepare.

**2026-02-14T07:58:39Z**

All 6 files read and changes prepared. Waiting for convert-entries (as-2034) to finish before applying reference updates.

**2026-02-14T08:01:41Z**

convert-entries worker finished. Applying reference updates now.

**2026-02-14T08:05:38Z**

DONE: Updated all 'handbook' → 'playbook' references and path patterns across 9 files:nn1. SKILL.md — Johnny Lookup section, Quickstart, directory structure diagram, Playbook table (15 entries), Rebase Strategyn2. references/core-principles.md — Johnny Decimal structure, lookup flow, example naming, audit commandsn3. references/templates.md — AGENTS.md template, wiki links, directory referencesn4. references/claude-template.md — Johnny Lookup, progressive disclosuren5. references/workflows.md — wiki creation, documentation tablen6. references/obsidian-open-questions-system.md — archive path, Dataview queryn7. playbook/10-docs/10.03-obsidian-flow/SKILL.md — 5 handbook→playbook referencesn8. playbook/10-docs/10.01-open-questions-system/SKILL.md — archive wiki linkn9. assets/starter-template/ — renamed handbook/ dir to playbook/, updated AGENTS.md (3 refs + directory AGENTS.md)nnAll file paths updated from NN.NN-slug.md → NN.NN-slug/SKILL.md format. Zero 'handbook' references remain.
