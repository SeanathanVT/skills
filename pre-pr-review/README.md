# pre-pr-review

An agent skill for the last pass before opening a PR. It reviews the branch's changes, reports verified findings, waits for approval, then fixes, re-reviews until clean, and stages the result with a drafted commit message. It never commits.

## Checks

- Correctness regressions introduced by the change
- Over-engineering: needless abstraction, reinvented standard-library or platform features
- Readability, and cyclomatic complexity above 10
- Current industry best practice, verified against official docs rather than recalled
- Project conventions: style, layout, voice, CONTRIBUTING rules
- Docs match the current code and each other, with no stale references (historical docs exempt)
- Comments: brief, only the non-obvious why, no iteration narrative
- Hygiene: no local paths, debug leftovers, or references to uncommitted notes
- CHANGELOG: released sections untouched, `[Unreleased]` holds only the net delta
- Prose: no em-dashes or AI tells
- Lint and format via the project's own tooling, else per-language defaults

Full criteria: [checklist.md](checklist.md).

## Usage

Ask the agent for a final review, or invoke it directly (`/pre-pr-review` in Claude Code). Optional scope argument:

| Argument | Scope |
| --- | --- |
| (none) | Everything since the merge-base with the default branch, plus uncommitted work |
| `uncommitted` | Staged, unstaged, and untracked changes only |
| `last-commit` | `HEAD` only |
| path(s) | Only those files or directories |
| extra repo path(s) | Also check cross-repo doc references |

Works without plugins. If present, it can delegate the code half to review skills (for example `ponytail-review`, `code-review`, `security-review`) and fan out large scopes to subagents.

## Install

A skill is this folder, placed in a directory the agent harness scans. Symlink it to keep this repo the source of truth, or copy it.

| Harness | Personal (all projects) | Project (commit with the repo) |
| --- | --- | --- |
| [Claude Code](https://code.claude.com/docs/en/skills) | `~/.claude/skills/` | `.claude/skills/` |
| [OpenCode](https://opencode.ai/docs/skills/) | `~/.config/opencode/skills/`, or `~/.claude/skills/` / `~/.agents/skills/` | `.opencode/skills/`, `.claude/skills/`, or `.agents/skills/` |
| [Codex CLI](https://developers.openai.com/codex/skills) | `~/.agents/skills/` | `.agents/skills/` |
| [Gemini CLI](https://github.com/google-gemini/gemini-cli/blob/main/docs/cli/skills.md) | `~/.gemini/skills/` or `~/.agents/skills/` | `.gemini/skills/` or `.agents/skills/` |
| [Copilot CLI](https://docs.github.com/en/copilot/how-tos/copilot-cli/customize-copilot/add-skills) | `~/.copilot/skills/` or `~/.agents/skills/` | `.github/skills/`, `.agents/skills/`, or `.claude/skills/` |

`~/.agents/skills/` is shared by Codex, Gemini, Copilot, and OpenCode; `~/.claude/skills/` by Claude Code and OpenCode. Use a shared directory if you run more than one harness, otherwise the harness-specific one. From a clone of this repo, at its root:

```bash
ln -s "$PWD/pre-pr-review" ~/.claude/skills/pre-pr-review   # Claude Code, OpenCode
ln -s "$PWD/pre-pr-review" ~/.agents/skills/pre-pr-review   # Codex, Gemini, Copilot, OpenCode
```

Use a copy instead of a symlink if the harness runs on a different machine or container than the one where the source path exists. Restart the session (or reload skills) afterward.

### Agent-assisted install

Paste this into any agent session:

> Install the agent skill at `https://github.com/SeanathanVT/skills/tree/main/pre-pr-review` by following the "Instructions for agents" section of its README.md.

### Instructions for agents

1. Identify the harness you are running in from your own identity or system prompt. If unknown, ask the user.
2. Ask the user for scope (personal or project) unless they already said. Pick the directory from the table above; where a row lists several, follow the rule under the table.
3. Check `<skills-dir>/pre-pr-review` with `ls -la` (catches dangling symlinks). A symlink already pointing to this source: nothing to do, skip to step 5. Anything else there: show the user and ask before replacing it.
4. Install. Local source that resolves where the harness runs: symlink with an absolute path. Otherwise copy the whole folder (for a URL, clone or download the full directory, not individual files). Create `<skills-dir>` if missing.
5. Verify `<skills-dir>/pre-pr-review/SKILL.md` resolves and its frontmatter has `name: pre-pr-review` and a `description`. If the harness has a listing command (Claude Code `/skills`, Gemini `gemini skills list`, Copilot `copilot skill list`), tell the user to run it after reload.
6. Report the installed path, whether it is a symlink or a copy, and that a session restart or skill reload may be needed.
