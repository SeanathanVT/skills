# skills

Portable agent skills, built from real working sessions and tested against a no-skill baseline. Install by asking your agent.

Each skill is a top-level folder following the [Agent Skills](https://agentskills.io/specification) format, with its own README covering usage and install.

| Skill | Use it when |
| --- | --- |
| [pre-pr-review](pre-pr-review/) | Work is done and you want a last-pass review, cleanup, and doc harmonization before opening a PR |

## Install

Paste into your agent session, replacing `<skill>`:

> Install the agent skill at `https://github.com/SeanathanVT/skills/tree/main/<skill>` by following the "Instructions for agents" section of its README.md.

Or clone this repo and symlink `<skill>/` into your harness's skills directory (paths per harness are in each skill's README).
