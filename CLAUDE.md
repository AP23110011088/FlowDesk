# CLAUDE.md — FlowDesk AI Assistant Conventions

This file tells Claude Code (and other AI assistants) about FlowDesk's stack,
conventions, and rules. It is the single source of truth for AI-assisted development.

---

## Project Overview

**Name**: FlowDesk — AI-Assisted Settings Suite
**Track**: AI Developer Track — FE-02 Prompting Drill
**Purpose**: Demonstrate the impact of prompt quality by building the same feature twice

---

## Tech Stack

| Layer | Technology |
|-------|-----------|
| Frontend | Vanilla HTML5 + CSS3 + ES2022 JS |
| No framework | Plain files, no bundler, no npm |
| AI Tooling | Claude Code (Antigravity) |

---

## Directory Layout

```
flowdesk/
├── client/
│   ├── settings-vague/     ← Round 1 output (vague prompt)
│   └── settings-precise/   ← Round 2 output (precise prompt)
├── CLAUDE.md
├── WORKFLOW.md
├── README.md
└── LICENSE
```

---

## Coding Conventions

- Use semantic HTML5 elements (`<main>`, `<fieldset>`, `<legend>`, etc.)
- CSS custom properties (variables) for all design tokens — no hardcoded hex values
- ES modules or plain `<script>` — no bundler required
- One HTML file per feature — keep it self-contained

---

## Commit Convention

Follows **[Conventional Commits 1.0.0](https://www.conventionalcommits.org/)**:

```
feat(scope): short description
fix(scope): short description
docs: update WORKFLOW.md
chore: update .gitignore
```

---

## AI Assistant Rules

### Rule 1 — Every form input MUST have ARIA attributes
All `<input>` elements must include:
- `aria-required="true"` if the field is required
- `aria-invalid` toggled to `"true"`/`"false"` on validation
- `aria-describedby` pointing to both a hint element and an error element

A form without these attributes **fails accessibility review** regardless of appearance.

> **Fails if**: any `<input>` is missing `aria-invalid` or has no linked error element.

### Rule 2 — Validation must fire on blur AND on submit; never on submit alone
Inline field validation must trigger `onblur` so users receive feedback before they reach
the submit button. Submit-only validation forces users to scroll back — especially painful
on mobile.

> **Fails if**: error messages only appear after the submit button is clicked.

### Rule 3 — Every AI-generated form must include a self-verification checklist
Prompts for forms must end with:
> *"After writing the form, write a checklist of every validation rule and verify each one against the code."*

This catches logic bugs before review — e.g. the email regex that accepted `user@@example.com`
was caught this way. Skipping this step means the AI's first draft ships as truth.

> **Fails if**: no verification checklist exists in code comments or the PR description.

---

## Branch Strategy

```
main                    ← stable, contains docs and shared files
feat/settings-vague     ← Round 1: vague prompt output
feat/settings-precise   ← Round 2: precise prompt output
```
