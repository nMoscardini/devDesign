# Project Template

Version 3.0. Updated 20 September 2026.

Use this to start a project or record a deviation. Keep it short. Write "not applicable" rather than deleting a heading.

## Part 1: One-page spec (SPEC.md)

- **Class.** Lab, Personal, or Shared, and why.
- **Problem.** Who has it, and what happens today.
- **Success.** How you will know, in terms someone can observe.
- **Riskiest assumption.** What, if wrong, makes the project pointless. Version 1 tests it.
- **Version 1 scope.** What is in. What is deliberately out.
- **People and data.** Whose data, and for each kind: purpose, lawful basis, who is responsible, who can see it, how long it is kept, how it is deleted (N6). Confirm nobody is ranked (N7). Write "no data about other people" if true.
- **Architecture in brief.** Components, where each runs, what is reachable from outside, which defaults are not used.
- **Reuse check.** What you looked at first.
- **Failure and recovery.** What can break, and the backup and restore plan for data you cannot recreate.
- **AI use.** Where AI is used, and what validates its output. "None" is valid.
- **Verification.** The smoke test, and the real or realistic data it runs on.
- **Deviations.** Links to any deviation records.

## Part 2: State document

Keep it at the project root as ENGINEERING_STATE.md, or the name the project already uses. Nino supplies it at the start of AI sessions. It is authoritative. Update it at the end of every session.

- **Status.** Class, version, one sentence on where it stands.
- **What it is.** Two sentences.
- **Current architecture.** Components, where they run, ports, what is exposed.
- **Decisions.** Date, decision, reason, link to any deviation record.
- **Open items.** What is unresolved, and what each waits on.
- **Known problems.** What is fragile, and how to tell.
- **Run, stop, recover, back up.** The commands, or where they are.
- **Last verified.** Date, what was checked, what was not.
- **Session log.** Newest first: date, what changed, what was decided.

## Part 3: Deviation record

Record a departure from a principle or default when it changes architecture, security, or data handling, or would surprise a future reader. A departure from a non-negotiable or hard rule is not allowed. If Nino wants one, he changes the document.

One file per decision in `docs/decisions/`, numbered, with date and status (proposed, accepted, superseded).

**Lite record (Personal).** Lab needs none.

- **Decision.** One sentence.
- **Why.** The need behind it.
- **Principle affected.** For example 2 or H1.
- **Reversal point.** What would make you revisit it.

**Full record (Shared).** The lite fields, plus:

- **Options considered.** Credible alternatives, including the default.
- **Consequences.** Effect on complexity, cost, security, maintenance, and operations.
