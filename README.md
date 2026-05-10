# SGA2025 .github

Default community health files for all repositories under the
[SGA2025](https://github.com/SGA2025) account.

## What's here

| File | Purpose |
|------|---------|
| `SECURITY.md` | Security policy + vulnerability reporting |
| `CONTRIBUTING.md` | Multi-party protocol + workflow |
| `PULL_REQUEST_TEMPLATE.md` | PR template with risk classification + security self-check |

## How this works

GitHub automatically applies these files as defaults to all
repositories under the SGA2025 account, unless a specific
repository overrides them with its own version.

Learn more: [Creating a default community health file](https://docs.github.com/en/communities/setting-up-your-project-for-healthy-contributions/creating-a-default-community-health-file)

## Primary reference

See `ifpa-website/SECURITY-INFRASTRUCTURE-PLAN-2026-04-26.md`
for the architectural context that produced these files.

## AI Council Workspace

`docs/ai-council/` hosts a multi-AI review workspace for AI tooling and
policy documents. It contains:

- [`docs/ai-council/AGENTS.md`](docs/ai-council/AGENTS.md) — working
  rules for AIs operating in this workspace
- [`docs/ai-council/README.md`](docs/ai-council/README.md) — workspace
  purpose and directory layout
- `docs/ai-council/prompts/` — instruction templates sent to reviewer AIs
- `docs/ai-council/responses/` — verbatim per-AI responses (immutable)
- `docs/ai-council/reference/` — external materials and commentary

New reviewer AIs should read `docs/ai-council/AGENTS.md` first.
Operators placing new content should follow the
`YYYY-MM-DD_ai-name_topic.md` naming convention defined there.
