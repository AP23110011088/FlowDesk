# FlowDesk — AI-Assisted Settings Suite

![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)
![Node.js](https://img.shields.io/badge/Node.js-20_LTS-green.svg)
![Commits](https://img.shields.io/badge/Commits-Conventional-orange.svg)
![AI Tooling](https://img.shields.io/badge/AI-Claude_Code-purple.svg)

> A full-stack settings management suite built with AI-assisted development as part of the AI Developer Track capstone (FE-02 Prompting Drill).

## Why FlowDesk?

FlowDesk explores the difference between vague and precise AI prompting by building the same feature twice — a user settings form with validation — using two different prompting strategies. The result is a concrete, diff-backed demonstration of how prompt quality directly determines output quality.

## Project Structure

```
flowdesk/
├── client/
│   ├── settings-vague/     # Round 1: single vague prompt output
│   │   └── index.html
│   └── settings-precise/   # Round 2: precise prompt with constraints & verification
│       └── index.html
├── .gitignore
├── CLAUDE.md               # AI assistant conventions & learned rules
├── LICENSE
├── README.md
└── WORKFLOW.md             # 400-word comparison of both rounds
```

## The Two Branches

| Branch | Prompt Style | Lines | ARIA attrs | Edge cases |
|--------|-------------|-------|-----------|------------|
| `feat/settings-vague` | One sentence, no context | 67 | 0 | 1 |
| `feat/settings-precise` | Full spec + verification step | 654 | 12 | 5 |

See [WORKFLOW.md](./WORKFLOW.md) for the full comparison.

## Tech Stack

| Layer | Technology |
|-------|-----------|
| Frontend | Vanilla HTML + CSS + JavaScript |
| AI Tooling | Claude Code (Antigravity) |
| Version Control | Git + GitHub |

## Getting Started

No build step needed. Open either form directly in your browser:

```bash
# Round 1 — vague prompt output
open client/settings-vague/index.html

# Round 2 — precise prompt output
open client/settings-precise/index.html
```

## Conventions

- **Commits**: [Conventional Commits 1.0.0](https://www.conventionalcommits.org/)
- **AI Rules**: See [CLAUDE.md](./CLAUDE.md) for all prompting conventions

## License

MIT — see [LICENSE](./LICENSE) for details.
