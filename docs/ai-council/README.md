# AI Council Workspace

Multi-AI review workspace for AI tooling and policy documents.
Each AI contributes as a reviewer; one AI (currently Claude) drafts and
integrates master documents.

## Directory Layout

```
docs/ai-council/
├── AGENTS.md          # Working rules for AIs in this workspace
├── README.md          # This file
├── prompts/           # Instruction templates sent to reviewer AIs
│   └── README.md
├── responses/         # Verbatim per-AI responses (immutable)
│   └── README.md
├── reference/         # External materials and out-of-band commentary
│   └── ...
└── versions/          # Master document versions (created as needed)
    └── ...
```

## How to Use

### Adding a new reviewer AI

1. The AI reads [AGENTS.md](./AGENTS.md).
2. The user (operator) shares the latest prompt from [`prompts/`](./prompts/)
   together with any required reference materials.
3. The AI's verbatim response is saved by the operator under
   [`responses/`](./responses/) using the naming convention.
4. The lead drafting AI integrates the response into the next master
   document version under `versions/`.

### Issuing a new prompt

1. Operator (or lead AI) creates a new file in `prompts/` using the
   naming convention `YYYY-MM-DD_to-<audience>.md`.
2. Operator sends the prompt verbatim to each reviewer AI.
3. Each response is collected in `responses/`.

### Versioning master documents

- `versions/v1.md` is the first draft.
- Reviewer feedback is integrated into `versions/v2.md`, etc.
- Earlier versions are never edited.

## File Naming Convention

```
YYYY-MM-DD_ai-name_topic.md
```

- Date: the date of the response (or prompt issuance).
- AI name (lowercase): `claude`, `perplexity`, `gemini`, `gpt`, ...
- Topic: short, hyphen-separated.

See [AGENTS.md §3](./AGENTS.md) for details.

## Immutability

Files under `prompts/`, `responses/`, and `reference/` are immutable after
commit. Corrections are added as `_corrigendum` files.

See [AGENTS.md §4](./AGENTS.md) for details.

## Confidentiality Boundary

This workspace MUST NOT contain client identifiers, case numbers,
privileged content, personal data, or secrets. It documents AI
architecture decisions only.

See [AGENTS.md §7](./AGENTS.md) for details.

## Related Documents

- [Top-level README](../../README.md)
- [v1 consultation document](./versions/v1.md)
