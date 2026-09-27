# devDesign - CLAUDE.md

Nino's standing design and engineering standards. Every other `~/dev*` project follows these documents. There is no code here.

## The documents

- `DESIGN_PRINCIPLES.md` - project classes (Lab, Personal, Shared), the non-negotiables N1 to N7, and principles 1 to 10. The rules other projects quote by number.
- `REFERENCE_ARCHITECTURE.md` - the current stack and setup: default stack, the Mac and the NAS, Server Manager contract, remote access, hard rules H1 to H3, backup register. Goes stale when hardware or tools change.
- `PROJECT_TEMPLATE.md` - Part 1 one-page spec (SPEC.md), Part 2 state document (ENGINEERING_STATE.md), Part 3 deviation record (lite and full).
- `REVIEW_PROCESS.md` - how to review a project against the principles and the reference architecture.

All four are version 3.0, dated 20 or 21 September 2026.

## When you are working on another project

Read DESIGN_PRINCIPLES.md and REFERENCE_ARCHITECTURE.md before designing anything. Decide the class by the questions in "Project classes", in order. Unsure means Shared. A departure from a principle needs a deviation record in that project's `docs/decisions/`. A departure from a non-negotiable or a hard rule is not allowed.

## When you are asked to change these documents

- Only Nino changes a non-negotiable (N1 to N7) or a hard rule (H1 to H3). You may challenge one: name it, say why it creates risk or waste, offer the simplest alternative, recommend follow, exception or revise (principle 10). Once per project.
- Bump the version and the "Updated" date at the top of every file you change.
- Update REFERENCE_ARCHITECTURE.md in the same change that alters the environment (a new port, a new service, a new backup).
- Mark anything described but not yet built as TARGET.
- Keep the style: short sentences, plain words, no jargon, hyphens not em dashes. Keep each document short.
- A change here can make other projects non-compliant. Say which projects are affected. `~/devUtils/devProjectStatus.html` lists them.

## Known gaps

- This folder is not in git. Changes have no history. Consider `git init` (see `~/devUtils/gitStatus.html`).

Audited on 26 September 2026.
