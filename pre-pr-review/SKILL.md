---
name: pre-pr-review
description: Use when work is finished and the user is about to open a PR, merge, release, or end a session, or asks for a final pass, last pass, cleanup, sanity check, doc harmonization, or a review of everything since branching.
---

# Pre-PR Review

Last-pass review before a PR: report verified findings, pause, fix and re-review until clean. Never commit.

## 1. Scope

Run `git status` and `git log` first; never assume state. Default scope: `git merge-base HEAD <default-branch>` through the working tree (committed, staged, unstaged, untracked). Arguments override: `uncommitted`, `last-commit`, paths, extra repos for cross-repo docs.

## 2. Inventory

List changed files. Add in-scope docs: README*, CHANGELOG, ROADMAP, CONTRIBUTING, AGENTS.md, CLAUDE.md, docs/, help text, example configs, and any file that mentions a changed identifier, flag, env var, or path (grep them). Read the project's conventions: CONTRIBUTING, linter configs, CI workflows, sibling files.

## 3. Tools

Run the project's own lint, format, and test commands first (CI config, Makefile, Taskfile, package scripts, pre-commit). Then fill gaps per [checklist.md](checklist.md#tools), always including a cyclomatic complexity tool. Missing tool: install it in a container; on a host, or if unsure, ask first.

## 4. Review

Apply every section of [checklist.md](checklist.md) to the scope. For large scopes, subagents (one per repo or per doc) and installed review skills (ponytail-review, code-review, security-review) may help; none are required. Verify each finding against the code, docs, or official upstream sources before reporting. Drop anything speculative.

## 5. Report, then stop

Make no edits before the user approves. This includes one-character fixes and reverts that look obviously correct.

The report is exactly:

1. One line: scope reviewed (base commit, file count).
2. Findings table, highest severity first:
   `| # | Severity | File:line | Finding | Evidence | Proposed fix |`
   Severity is `bug`, `cleanup`, or `nit`. Evidence is a command output, a quoted line, or a source URL.
3. **Decisions needed**: findings with more than one valid fix, each with options.
4. **Pre-existing / out of scope**: issues not introduced by this change.
5. **Tools**: what ran, and what could not run and why.
6. "Which items should I apply?"

## 6. Fix loop

Apply only approved items, in the project's style. Re-run steps 3 and 4 on everything the fixes touched. Repeat until there are no findings. Then `git add` the changes and print a commit message in the project's convention (Conventional Commits if none), with any attribution trailer the user or harness requires. Never commit or push.

## Red flags

| Thought | Reality |
| --- | --- |
| "This fix is trivial, I'll just apply it" | Report it. The user approves every edit. |
| "Code reads fine, skip the complexity tool" | Run it. CC > 10 is a reported finding. |
| "That's best practice" (from memory) | Cite a current official source or drop it. |
| "Docs probably still match" | Grep every changed identifier across all docs. |
| "Lots of nits make a thorough review" | Report nits only if few; signal over volume. |
