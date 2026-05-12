# Cursor: Hypothesis prototype skill + rules

This repository packages the **hypothesis-prototype** Cursor Agent Skill and the **hypothesis-prototypes** project rule used for Hexa-themed, tracked HTML prototypes (BEES-style patterns).

## Contents

| Path | Purpose |
|------|---------|
| `.cursor/skills/hypothesis-prototype/SKILL.md` | Agent skill: scaffold flow, PRD/HLR/min requirements, Hexa + BEES UI patterns |
| `.cursor/rules/hypothesis-prototypes.mdc` | Rule scoped to `prototypes/**/*`: vanilla stack, tracking, registration |

## Install (coworkers)

### Option A — Copy into a project

1. Clone or download this repo.
2. Merge the `.cursor/` folder into your project root (next to your `prototypes/` folder if you use the same layout).

Your tree should include:

```
your-repo/
  .cursor/
    skills/hypothesis-prototype/SKILL.md
    rules/hypothesis-prototypes.mdc
  prototypes/
    _shared/
    ...
```

### Option B — User-level skill (all workspaces)

Copy `skills/hypothesis-prototype/` into your Cursor user skills directory (Windows example):

`%USERPROFILE%\.cursor\skills\hypothesis-prototype\`

Keep the rule in each repo that contains `prototypes/`, or copy the `.mdc` into `.cursor/rules/` per project.

### After install

- In Cursor, enable **Agent Skills** and ensure the workspace contains this skill path.
- The skill expects a **PRD**, **HLR**, or **minimum requirements** brief before scaffolding (see `SKILL.md`).

## Full prototype stack

This kit documents conventions only. The **Hexa port, tracker, gallery, and templates** live in a separate repository that already hosts prototypes, for example:

https://github.com/silarerenan-cmyk/bre-edi-central-tracking-live-prototype

Clone that repo (or copy `prototypes/_shared/` and `prototypes/_template/` from it) before building new prototypes.

## Publish updates from a maintainer machine

See [PUBLISH.md](PUBLISH.md).
