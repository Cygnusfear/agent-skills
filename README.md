# agent-skills

Reusable skills for [pi](https://github.com/cygnusfear/pi-stuff) and other coding agents.

## Usage

Symlink or clone into your agent's skills directory:

```bash
git clone https://github.com/cygnusfear/agent-skills.git skills
```

Or in your pi `package.json`:

```json
{
  "pi": {
    "skills": ["./skills"]
  }
}
```

## Skills

| Skill | Description |
|-------|-------------|
| `architectural-analysis` | Deep architectural audit — dead code, duplication, anti-patterns |
| `blitz` | Parallel multi-issue sprints with git worktrees |
| `brainstorming` | Explore intent and design before implementation |
| `code-review` | Ultra-critical 6-pass PR review |
| `create-plan` | Comprehensive implementation plans |
| `ctx` | Build deep project understanding |
| `delphi` | Parallel oracle investigations for divergent insights |
| `design-spec-extraction` | Extract JSON design specs from visual sources |
| `systematic-debugging` | Structured root-cause analysis |
| `teams-driven-development` | Execute plans with parallel workers |
| `test-driven-development` | TDD workflow |
| `the-oracle` | Deep multi-source research |
| `video-explorer` | Analyze video via frame extraction |
| ... and 26 more |

## License

MIT
