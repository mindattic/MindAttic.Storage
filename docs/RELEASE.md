---
layer: release
status: living
---
# MindAttic.Storage — Release charter

> The one finish line this repo is working toward. Every open ticket in this repo sits under one of the epics
> below, and every epic is part of the milestone. Work that does not fit is out of scope until this milestone is
> reached (owner, 2026-11-27, MAT-T319).

## 1. What this is
Drop-in media storage for ASP.NET Core apps (formerly MindAttic.Media): one upload API, a catalog row per file,
and the `/_media/{uid}` endpoint, with a local-disk store and a now-frozen Azure Blob backend.

## 2. The milestone: MindAttic.Storage 1.x {#milestone}
- **Milestone:** MS-57, "MindAttic.Storage 1.x: local-disk store, Azure frozen"
- **Published means:** `MindAttic.Storage.1.0.0.nupkg` on `C:\LocalNuGet` (a finished internal library).
- **The natural stopping point:** Azure was decommissioned on 2026-11-26, so the local-disk store is the only
  live path. Once its DI wiring is tested and the README and MIGRATION.md say plainly that Azure is frozen and how
  consumers switch, the library is complete for what the portfolio actually runs.

## 3. Done when (acceptance checks)
1. build — `MindAttic.Storage.slnx` builds clean on main.
2. exists — `README.md` states the Azure backend is frozen.

## 4. Epics, in priority order
| Priority | Epic (ticket) | What it delivers | Features |
|---|---|---|---|
| 900 | M.MDIA-T31 Local-disk contract | The supported path is wired and tested | M.MDIA-T32 DI wiring (990: M.MDIA-T4) |
| 800 | M.MDIA-T33 Azure retirement | Docs reflect that Azure is gone | M.MDIA-T34 README marks Azure frozen (890: M.MDIA-T10), M.MDIA-T35 Consumer switch note (880: M.MDIA-T36) |

## 5. Out of scope (not before this milestone)
- Any Azure Blob work or tests (signer, SAS, container factory) — Azure is decommissioned.
- Deleting `MindAttic.Storage.Azure` — Prose and IdeaCms still reference it; they switch first.
- New storage backends; non-SQL-Server column mappings.
- Generic cleanups, bughunts and repo-sweep test runs.

## 6. Current verified state
- 1.0.0 packages on `C:\LocalNuGet`; consumers Prose (Prose.Core) and IdeaCms.
- Rename from MindAttic.Media: d73f2c1; MIGRATION.md: 5c18aad.
- Security and orphan hardening: fe7d3cb, 4817589.

## 7. Decision record
- 2026-11-27 — charter drafted from README, MIGRATION.md, git log and tickets; Legion vote: **Approve**, 6 of 9
  voters answered, plurality quorum reached, consensus 1.0, no dissent.
- Triage: 6 open → done 0, duplicate 0, wont-do 4 (T3, T8, T9, T22), kept 2 (T4 reworded to the local path,
  T10 reworded), new tasks 1 (T36).
