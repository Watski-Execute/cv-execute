# CV Execute

**An evidence-first, Markdown-native CV system for working with your existing AI.**

No dedicated app. No account system. No database. No proprietary model.

CV Execute helps an LLM turn a person's real experience into a role-specific CV without inventing credentials.

## Core idea

```text
messy career history
        ↓
verified evidence bank
        ↓
opportunity / vacancy analysis
        ↓
criterion → evidence mapping
        ↓
role-specific CV
        ↓
claim + readability quality gate
```

The AI is the interface.

Markdown is the portable context.

Git can be the version/history layer.

## Why this exists

Most CV tools begin with formatting.

CV Execute begins with evidence.

The system tries to answer:

1. What is actually true about the candidate?
2. What work is the candidate willing / able to consider?
3. What does this specific employer appear to need?
4. Which evidence best supports those needs?
5. How should that evidence be presented clearly without overstating it?

## Philosophy

> **Evidence first. Positioning second. Design third.**

> **Broad search. Narrow shot. Deep evidence behind it.**

A candidate can explore several plausible role families without stuffing all of those identities into one CV.

The evidence base may be broad.

Each application should remain narrow.

## Quick start

1. Copy the files in `templates/`.
2. Fill in `master-evidence-bank.md`.
3. Optionally fill in `candidate-constraints.md`.
4. Paste a vacancy into `vacancy.md`.
5. Give `SYSTEM.md`, the relevant candidate files and the vacancy to your preferred LLM.
6. Ask it to build the evidence matrix and draft a targeted CV.
7. Run the quality gate before submission.

Suggested instruction:

> Read SYSTEM.md first. Treat the evidence bank as the factual source of truth. Respect candidate constraints. Analyze vacancy.md, build the evidence matrix, identify unsupported requirements, then draft the strongest truthful CV for the role.

## Repository structure

```text
cv-execute/
├── README.md
├── SYSTEM.md
├── CHANGELOG.md
├── LICENSE
├── docs/
│   ├── evidence-model.md
│   ├── cv-architecture.md
│   ├── opportunity-targeting.md
│   ├── privacy-and-public-repos.md
│   ├── tailoring-workflow.md
│   └── quality-gate.md
├── templates/
│   ├── master-evidence-bank.md
│   ├── candidate-constraints.md
│   ├── vacancy.md
│   └── evidence-matrix.md
└── example/
    └── fictional-candidate.md
```

## Evidence states

CV Execute distinguishes:

- **VERIFIED**
- **SUPPORTED**
- **ADJACENT**
- **UNVERIFIED**
- **GAP**
- **DO NOT CLAIM**

This helps prevent repeated AI rewriting from gradually turning adjacent experience into fake tenure.

## What this is not

CV Execute is not:
- a résumé-design application
- an ATS-score generator
- a guarantee of employment
- a license for an AI to invent experience
- tied to ChatGPT, Claude, Gemini, or another model

## Privacy

The public framework and a user's real evidence should normally be separate.

A public repo can contain the generic protocol/templates/examples.

A real evidence bank may belong in local storage or a private repository.

Never place secrets or credentials in an AI-readable CV repository.

See `docs/privacy-and-public-repos.md`.

## Status

**v0.2 — early public prototype**

The project is deliberately simple.

Build software only when real usage reveals a problem that Markdown + an existing LLM cannot solve cleanly.
