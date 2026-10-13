# devDesign - CLAUDE.md

Nino's standing design and engineering standards. Every other `~/dev*` project follows these documents. There is no code here.

## The documents

- `DESIGN_PRINCIPLES.md` - project classes (Lab, Personal, Shared), the non-negotiables N1 to N7, and principles 1 to 10. The rules other projects quote by number.
- `REFERENCE_ARCHITECTURE.md` - the current stack and setup: default stack, the Mac and the NAS, Server Manager contract, remote access, hard rules H1 to H3, backup register. Goes stale when hardware or tools change.
- `PROJECT_TEMPLATE.md` - Part 1 one-page spec (SPEC.md), Part 2 state document (ENGINEERING_STATE.md), Part 3 deviation record (lite and full).
- `REVIEW_PROCESS.md` - how to review a project against the principles and the reference architecture.
- `UI_PRINCIPLES.md` - how apps and pages look and behave: native apps follow Apple's Human Interface Guidelines, web pages share one style, accessibility, and a UI review checklist.

The first four are version 3.0 or later, dated September 2026. UI_PRINCIPLES.md is version 1.0, dated 13 October 2026.

## When you are working on another project

Read DESIGN_PRINCIPLES.md and REFERENCE_ARCHITECTURE.md before designing anything. If the project has a user interface, read UI_PRINCIPLES.md too. Decide the class by the questions in "Project classes", in order. Unsure means Shared. A departure from a principle needs a deviation record in that project's `docs/decisions/`. A departure from a non-negotiable or a hard rule is not allowed.

## When you are asked to change these documents

- Only Nino changes a non-negotiable (N1 to N7) or a hard rule (H1 to H3). You may challenge one: name it, say why it creates risk or waste, offer the simplest alternative, recommend follow, exception or revise (principle 10). Once per project.
- Bump the version and the "Updated" date at the top of every file you change.
- Update REFERENCE_ARCHITECTURE.md in the same change that alters the environment (a new port, a new service, a new backup).
- Mark anything described but not yet built as TARGET.
- Keep the style: short sentences, plain words, no jargon, hyphens not em dashes. Keep each document short.
- A change here can make other projects non-compliant. Say which projects are affected. `~/devUtils/devProjectStatus.html` lists them.

## Version control

This folder is a git repository, with remote `origin` at github.com/nMoscardini/devDesign. Commit each change to these documents with a message that says what changed and which projects it affects. The repository should stay private: REFERENCE_ARCHITECTURE.md describes ports, the tunnel, and exposure.

Audited on 26 September 2026. Updated 13 October 2026.
