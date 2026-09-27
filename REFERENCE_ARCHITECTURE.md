# Reference Architecture

Version 3.0. Updated 21 September 2026.

## Purpose

The current stack and setup for Nino's projects. It supports DESIGN_PRINCIPLES.md. It goes stale when hardware or tools change, so it is a separate file. Update it in the same change that alters the environment.

The Mac means the Mac Studio. Local disk means storage inside, or directly attached to, the machine running the service. A network share is never local disk.

CURRENT means running today. TARGET means intended, not built. If this file and the running system disagree, the system is the truth. Correct this file.

## Known gaps

Recorded September 2026. A session cannot check them. Ask Nino to confirm before relying on them.

| Item | Document says | What runs | Action |
|---|---|---|---|
| Tunnel location | TARGET: Cloudflare Tunnel on the NAS in Docker | CURRENT: `cloudflared` runs on the Mac under Server Manager, because `cloudflared service install` proved unreliable | Move it to the NAS, or accept the Mac and record why |
| NAS work queue | TARGET: NAS accepts work while the Mac is down | Not confirmed as built | Confirm. If built, check it follows H3 |
| Off-site backup | Required by principle 3 | Destination not recorded | Choose it and record it in the backup register |
| Remote user login | Exposed endpoints must authenticate callers | How the Observer app authenticates is not recorded | Record the method as the default, or choose another |
| Mac location | Not stated | Where the Mac physically is is not recorded | Record it. Off-site means a different building |
| Speech to text | Not named | `wimber_transcribe.py` exists. Its engine is not recorded | Record the engine as the default |

Score N4 on each project's own evidence. If you cannot tell whether a copy is off-site, score it unverified and cite these gaps.

## Default stack

Use these unless one fails a material requirement.

| Need | Default |
|---|---|
| Language | Python |
| HTTP services and APIs | FastAPI |
| Browser interfaces | Server-rendered HTML with light JavaScript |
| iPhone and Mac apps | Swift, built in Xcode |
| Editor | VS Code |
| Performance-critical work | C, only when measurement supports it |
| Local database | SQLite, under the hard rules below |
| Data interchange | Structured JSON |
| Local AI models | Ollama on the Mac Studio (CURRENT) |

Leave SQLite only when more than one machine must write the same data, or measured write contention blocks the work. Record the move.

## The Mac and the NAS

The Mac hosts the FastAPI services, application logic, databases, AI workloads, and heavy computation. Do not move application work to the NAS without a clear operational reason.

The NAS does durable storage, backup, RAID-protected data, and shared files. Docker on the NAS hosts the tunnel (TARGET) and small services that gain from being always on. Use mirrored NVMe for databases that live on the NAS, and HDD RAID for large files.

## Server Manager

Server Manager is the menu bar app (DevServerShell) that starts, stops, restarts, monitors, and shows logs for Mac services. It restarts a service after a crash or file change. A new Mac service integrates with it, so normal running needs no terminal windows. It does so by meeting this contract:

- One start command, runnable from a clean shell. Working directory and environment are documented.
- Exits cleanly when told to stop. No orphan processes.
- HTTP services answer `GET /health` quickly with status and version. Others give a command that exits 0 when healthy.
- A fixed port from the register below.
- Logs to stdout and stderr in the structured format of principle 8.
- Safe to restart at any time. Recovers state and resumes interrupted work.
- Listens on 127.0.0.1 unless it must be reached from another machine.
- Secrets come from the environment or a protected file, never the command line.

Port register (recorded September 2026, ask Nino to confirm). Add a row for each new service in the same change.

| Service | Port | Exposure |
|---|---|---|
| devHX | 8010 | Local only |
| Lumen admin app | 8001 | Local only |
| Observer Catcher | 8002 | Internet-facing, through the tunnel |
| devDownload | 8020 | Local only |

## Remote access

The normal path is: iPhone or remote client, then Cloudflare Tunnel, then an ingress service on the Mac. See the known gaps for where the tunnel runs.

Four kinds of thing may be reachable from the internet (N2). Anything else is not exposed.

- **Narrow ingress service.** Does one job, accepts only the input it expects, offers no way to read or change anything else, and authenticates its callers.
- **Public form.** Rules below.
- **Protected page or service.** Shows or changes data for remote users, such as a dashboard. It sits behind Cloudflare Access, or an equivalent, with individual login.
- **Public read-only page with no personal data**, such as service times.

Databases and admin pages are never reachable, even behind a login. An admin page configures the system, manages users or access, or runs maintenance such as migrations. A page that only shows or edits business data for its users is a protected page, not an admin page.

The tunnel is not authentication. Authenticated means a named user proves who they are (a personal login, or a one-time code to their own email) and Nino can revoke that user alone. Nino counts as a user. To see what is exposed, read the tunnel's ingress rules, not the application's own configuration.

Phone apps and remote users:

- For phone data capture (an app or web form), reuse the Observer Catcher pattern unless a deviation is recorded.
- Where remote users also read or change data, use a protected page behind Cloudflare Access. Each user sees only what they need.
- No shared logins. Each person signs in themselves, even on a shared device.
- Use TestFlight while piloting and the App Store for wider release. An app only Nino uses can be installed from Xcode.

Public forms are for cases where submitters cannot be known in advance, such as a prayer request page. A known team uses an authenticated ingress service instead. A public form:

- accepts only the expected fields, with size limits, and validates them
- is rate limited, with a bot check if spam appears
- cannot read anything back
- stores each submission durably before acknowledging it
- is read only by authenticated pages, and nothing is shown publicly without a person's review
- still meets N6 if it collects data about people

## Hard rules

Gates, scored like the non-negotiables. H3 applies only if a NAS queue exists.

**H1 SQLite stays on local disk.** Open a SQLite file only from the machine whose local disk holds it. Never over SMB, NFS, or any network share. Network locking is unreliable and can corrupt the database.

**H2 Back up SQLite with the backup tools.** Never copy a live SQLite file. Use the Python `sqlite3` connection `backup()` method, `sqlite3 <db> ".backup <file>"`, or `VACUUM INTO '<file>'`. Write the backup to local disk first, never straight to a network share. Then copy it to the NAS, then off-site.

**H3 The NAS queue.** It is a small separate service with its own local storage. It stores a request durably, then acknowledges it. The Mac pulls queued work. Neither machine opens the other's databases. Every request has a unique key, so a duplicate is harmless.

## Backup register

One row per important dataset. An empty cell is a gap.

| Dataset | Working copy | Second device | Off-site | Last restore test |
|---|---|---|---|---|
| Lumen corpus database | Mac Studio | Not recorded | Not recorded | Not recorded |
| Observer submissions | Mac Studio | Not recorded | Not recorded | Not recorded |
