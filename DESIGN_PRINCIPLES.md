# Design Principles

Version 3.0. Updated 20 September 2026.

## Purpose

Guiding principles for Nino's software. They keep projects simple, safe, and maintainable by one person, and stop the overall intent getting lost between sessions.

Most of this is defaults, not laws. If a default gets in the way, say so and record why you are departing from it. The non-negotiables (N1 to N7) and the hard rules in REFERENCE_ARCHITECTURE.md (H1 to H3) are different. They are gates. They come first.

Four files:

- DESIGN_PRINCIPLES.md (this file). Durable rules. Always read it.
- REFERENCE_ARCHITECTURE.md. The current stack and setup. It changes when hardware or tools change.
- PROJECT_TEMPLATE.md. For starting a project: a one-page spec, a state document, deviation records.
- REVIEW_PROCESS.md. How to review a project.

If they disagree, the gates win, then this file, then the others.

## Project classes

How much rigour a project needs depends on who is hurt if it fails. Write the class in the README or spec. For Lab, a comment at the top of the main file is enough. Ask in order and stop at the first yes.

1. Does anyone other than Nino use it or depend on it? Shared.
2. Does it hold, process, or share data about other people? Shared.
3. Can anything about it be reached from the internet? Shared.
4. Does Nino rely on it regularly? Personal.
5. Otherwise it is an experiment on the Mac. Lab.

Names that turn up incidentally in Nino's own sermons or notes do not trigger question 2. They do if the project shares that content, or extracts, tags, counts, or reports on the named people.

- **Lab.** Gates only. Say what question you are exploring and when to stop.
- **Personal.** Gates, and the principles as defaults. A departure needs a lite deviation record.
- **Shared.** Gates, and all relevant principles. A departure needs a full deviation record. If it holds other people's data, it needs a data protection review before release.

Unsure? Assume Shared. If the declared class fails the questions, the higher class applies. A project moves up the moment an answer becomes yes.

Examples. A throwaway analysis is Lab. Server Manager is Personal. The Lumen Observer app, which collects other people's observations, is Shared.

A product someone pays for is outside all three classes. Write a separate hardening standard before starting one.

## Non-negotiables

These apply to every class, Lab included. They are gates. Never trade them against speed or user need. Only Nino can change one.

- **N1 Secrets.** No keys, tokens, or passwords in source, logs, or AI prompts. If one leaks, rotate it first, then clean up.
- **N2 Narrow exposure.** Only the four kinds of thing listed in REFERENCE_ARCHITECTURE.md (Remote access) may be reachable from the internet. Databases and admin pages never, even behind a login. A hidden URL is not protection.
- **N3 Backup before destructive change.** Before any migration, bulk edit, deduplication, rebuild, or delete, take a fresh backup and confirm it opens. Data you can rebuild quickly from its source is exempt.
- **N4 Restore is proven.** Data you cannot recreate has a backup that meets the minimum in principle 3, and a restore has been tested.
- **N5 AI output is validated.** Check it against a schema before storing it. For free text, run a sanity check, store it beside its source, and label it machine-generated. Reject and log failures. A consequential action (hard to undo, or affecting other people, money, or reputation) also needs a person to confirm it.
- **N6 People's data has a purpose.** Before collecting data about other people, or sending it to a third party such as a cloud AI service, write down purpose, lawful basis, retention, and deletion (principle 5). Deriving new data about a person counts as collecting. Synthetic test data is fine.
- **N7 People are not ranked.** No ratings, scores, or league tables of named individuals. Counts of what happened, in a fixed order such as alphabetical, are fine. So are a person's own trend and group totals. Who may see a person's record is decided on the N6 page.

## 1. Start with the problem

- State the problem and what success looks like before proposing architecture.
- Build the smallest useful version that tests the riskiest assumption. For a personal product, test whether it becomes part of normal use.
- Add features from real use, not imagination.

## 2. Keep it simple, and reuse

- Design for one informed person to understand, operate, and repair.
- Search GitHub and Nino's own projects before building a substantial capability. Prefer mature, understandable components. Check licence, maintenance, and dependency cost.
- Use few components with clear responsibilities. Split into services only for a real reason: different operating needs, different exposure, or failure isolation.
- No speculative frameworks, plug-in systems, or platforms. Build the current product.
- Keep dependencies few and boring. Avoid mandatory cloud services where local meets the need.
- Use the simplest interface that does the job. Keep phone apps thin and quick.
- Start with the clearest implementation. Measure before optimising.

## 3. Design for failure and recovery

- Assume machines, networks, APIs, and models will fail. Save accepted work before acknowledging it. Make interrupted jobs resumable, retries bounded, and repeats harmless.
- Test recovery, not only the happy path.

Backup minimum, for data you cannot recreate (it exists nowhere else and cannot be rebuilt from other sources):

- One copy on a different device.
- One copy off-site, in a different building.
- RAID and mirroring are not backups.
- A tested restore at setup, then yearly (Personal) or twice yearly (Shared).
- Encrypted, if it holds personal data.
- Written down: how much you could afford to lose, and how long you could be down.

How to back up SQLite, and the register of where backups live, are in REFERENCE_ARCHITECTURE.md.

## 4. Treat security as architecture

- Keep the attack surface as small as possible (N2).
- A tunnel hides an address. It is not authentication. Authenticate remote actions in proportion to their risk.
- A page that shows other people's data needs a login, even on the local network.
- Least privilege for services, files, and tools. Keep secrets out of source and logs (N1). Keep an audit trail for sensitive actions.

## 5. Respect the people behind the data

This applies whenever a project holds data about anyone other than Nino.

- Before collecting, write one page: purpose, lawful basis (the legal reason you may hold it, such as consent), who is responsible for the data (the church, a charity, or Nino), who can see it, how long it is kept, how it is deleted.
- Collect the minimum. Never collect in case it becomes useful.
- Do not rank, rate, or profile people (N7). Show people their own data, and aggregates of others.
- Be able to find, correct, and delete one person's data. Say how deletion reaches backups, for example by letting them expire after a stated period.
- Church records can reveal religious belief, and free text often includes health details. UK law treats both as special category data. Ask whether free text is needed, limit who reads it, and set a retention period.
- Keep personal data local unless a cloud service gives a clear benefit. Before sending data to a provider, check whether it keeps or trains on it. If you cannot tell, do not send it.
- A Shared project needs a data protection review by a qualified person before release. Release means beyond a small named pilot group who have agreed to test. A pilot can start earlier, but N6 still applies. Start the review early.
- If personal data is exposed or lost, stop and tell Nino at once. Some breaches must be reported to the ICO within 72 hours. Take advice.
- This is a design rule, not legal advice. It assumes UK law and a UK church.

## 6. One source of truth

- Each fact lives in one place. Everything else is derived from it and can be rebuilt. Never hand-edit derived data.
- Shared rules or selection logic get one implementation. Copies drift.
- Version schemas. Every schema change is a scripted migration, tried on a copy, and run after a backup (N3).

## 7. Keep AI bounded

- Use deterministic code for deterministic work. Use AI for interpretation, generation, and classification.
- Keep AI out of the live path (the code that answers a user's request) unless AI is the product. Run it at ingest or in background jobs, store the result, and serve it with deterministic code. Record any exception.
- Keep model calls behind a clear interface. Use structured output. Validate it (N5).
- Every AI conclusion traces back to its source material.
- Keep a human in the loop where errors carry consequences. Prefer local models where privacy or economics justify them.

## 8. Make it observable, and verify it

- Use structured logs with stable names. Record state changes, failures, retries, and external calls. Give long-running services a health check. Keep secrets and needless personal data out of logs.
- Done means it ran against real data, or a realistic copy, and someone looked at the output. Passing tests alone is not done.
- Personal and Shared projects have a smoke test: one command that starts it and runs the main path. Test the failure paths too: restart, duplicate request, missing input, dependency down.
- Try risky changes on a copy. A copy of real personal data is still personal data. Keep it on the Mac and delete it when done.
- State what was checked and what was not. Never claim verification that did not happen.

## 9. Build with AI safely

- Start from the spec and the state document. Nino supplies it at the start of a session. It is authoritative on the intended design. The code shows what exists. If they disagree, stop and say so.
- Make small changes with one purpose each. Review the diff. Back up before any AI change that touches data (N3).
- The AI states its assumptions and what it did not verify. It adds nothing that was not asked for. It says when a request conflicts with these documents.
- At the end of a session, update the state document: what changed, what was decided, what is open. If a decision belongs in a deviation record, write it.

## 10. Keep documents true, and challenge them

- Document how to install, run, stop, recover, back up, and update. Update docs in the same change. That includes these files: mark anything not yet built as TARGET.
- Anyone may challenge a principle. Name it, say why following it creates risk, waste, or engineering gymnastics, offer the simplest alternative, and recommend: follow, make an exception, or revise the principle. The non-negotiables can be challenged, but only Nino changes them.
- Challenge a principle once per project. Record the outcome. Do not raise it again unless the facts change.

Consistency is not a substitute for judgement.
