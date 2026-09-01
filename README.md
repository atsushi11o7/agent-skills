# Agent Skills

A personal collection of [Agent Skills](https://agentskills.io/specification), built for use with [Claude Code](https://claude.com/claude-code) (and any other agent that supports the same convention).

## Skills

| Skill | What it does |
|---|---|
| [`git-commit`](skills/git-commit) | Writes a Chris Beams–style commit message from the actual diff, confirms it with you, then commits. Never pushes. |
| [`gh-pr-create`](skills/gh-pr-create) | Reviews the branch's diff and commit history against its base, drafts a PR title and body, confirms with you, then runs `gh pr create`. |
| [`gh-issue-create`](skills/gh-issue-create) | Drafts a bug report, feature request, or task issue, confirms with you, then runs `gh issue create`. |
| [`kaggle-competitions-submit`](skills/kaggle-competitions-submit) | Checks the submission file against the competition's requirements and today's submission count, drafts a submission message, confirms with you, then runs `kaggle competitions submit`. |

Where relevant, a skill checks the project's `AGENTS.md` (or `CLAUDE.md`) for project-specific conventions before falling back to its own defaults. None of them take an action that's hard to undo — pushing, posting, submitting — without showing you the result and confirming first.

## Templates

- [`templates/AGENTS.md`](templates/AGENTS.md) — a general-purpose starting point for a project's `AGENTS.md`. Trim it down to whatever sections your project actually needs; it's meant to be edited, not used as-is.

## Usage

These skills follow the `skills/*/SKILL.md` layout from the Agent Skills specification, so they can be installed into any other project with the [GitHub CLI](https://cli.github.com/)'s `gh skill` extension:

```bash
# Install one skill for Claude Code, in the current project
gh skill install atsushi11o7/agent-skills git-commit --agent claude-code

# Install all skills at once
gh skill install atsushi11o7/agent-skills --all --agent claude-code

# Install for every project on this machine instead of just this one
gh skill install atsushi11o7/agent-skills git-commit --agent claude-code --scope user
```

Update installed skills later with:

```bash
gh skill update --all
```
