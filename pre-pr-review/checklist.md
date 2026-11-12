# Pre-PR Review Checklist

Flag an item only when it applies to the in-scope change. Each finding needs evidence.

## Tools

Prefer the project's configured tooling. Otherwise, by file type:

| Files | Lint / format | Complexity |
| --- | --- | --- |
| Shell | `shellcheck`, `shfmt -d` | `lizard -C 10` |
| Python | `ruff check`, `ruff format --check` | `lizard -C 10` or `ruff check --select C901` |
| Markdown | `markdownlint-cli2` | |
| YAML / Ansible | `yamllint`, `ansible-lint` | |
| Terraform | `terraform fmt -check`, `terraform validate`, `tflint` | |
| Rust | `cargo fmt --check`, `cargo clippy` | `lizard -C 10` |
| JS / TS | project `eslint` / `prettier --check` | `lizard -C 10` |
| Go, Java, Kotlin, C/C++, others | the language's standard linter | `lizard -C 10` |

`lizard` (`pip install lizard`) covers most languages. Use the project's lint rules over these defaults where they conflict.

## Correctness

- Behavior removed or changed without a matching doc update or breaking-change marker.
- Flags, arguments, or inputs not validated at trust boundaries.
- Fixes in this change that introduce new regressions (re-check callers).

## Over-engineering

- Code that reimplements a standard-library, shell, or platform feature.
- Abstractions, options, parameters, or branches with one caller or no caller (dead branches, unused functions, unreachable cases).
- Duplicate output, duplicate logic, or config nobody asked for.
- A simpler native pattern exists for the same result.

## Readability and complexity

- Any function with cyclomatic complexity above 10 (report the measured value).
- Deep nesting, long functions, or names that need a comment to explain.
- Clever one-liners a reader must decode.

## Best practice and conventions

- Deviation from current industry practice for the language or tool. Cite an official, current source (docs, style guide, standard); never rely on memory alone.
- Deviation from the project's own conventions: formatting, file layout, naming, voice, CONTRIBUTING rules. If the project convention conflicts with best practice, report it as a decision, not a fix.
- Commit messages and versions: Conventional Commits and SemVer, if the project uses them; version numbers consistent across files.

## Documentation harmonization

- A doc describes behavior, flags, env vars, paths, defaults, or examples that the code no longer has, or omits ones it now has.
- Two docs contradict each other, or repeat the same fact (keep one source, link to it).
- Stale references: commit hashes, file names, section numbers, versions, URLs, or cross-repo links that no longer resolve.
- Exempt: docs that are explicitly historical (investigation logs, ADRs, released CHANGELOG sections).
- README: written for end users, concise, Quick Start complete and runnable from a clean machine.

## Comments

- Comments that restate what the code does, or explain basics to a beginner.
- Iteration narrative: "previously", "now", "v2", "iter3", "turned out", "fixed the earlier bug". Git history holds this.
- Comments longer than the non-obvious reason they convey.

## Hygiene

- Local or personal paths (`/work`, `/Users/...`, `tmp/`), hostnames, or dev-environment details.
- References to uncommitted notes, briefs, scratch files, or debug logs.
- Debug leftovers: flags enabled, print/echo tracing, test-only env vars, commented-out code.
- Named test devices, people, or customers in code or user-facing docs.
- Secrets or tokens.

## CHANGELOG

- Any edit to a released section (anything other than `[Unreleased]`).
- `[Unreleased]` entries for work that was added and removed, or broken and fixed, within this same unreleased change. List only the net delta vs. the last release.
- Entries written for developers rather than users, or not following the project's existing format (Keep a Changelog if none).

## Prose

- Em-dashes in any added text.
- AI tells: filler openers, "robust", "seamless", "leverage", "delve", "it's worth noting", inflated summaries, needless bold.
