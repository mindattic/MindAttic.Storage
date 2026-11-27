---
codex: 1
project: MindAttic.Storage
code: M.MDIA
layer: stories
status: living
updated: 2026-10-12
---
<!-- GENERATED from MindAttic.Tickets/data/Tickets.db by `tickets db render`. Do not edit: this file is rewritten from the database; change stories and tickets with the tickets CLI. -->

# MindAttic.Storage — User Stories
> ✅ done (shipped & tested) · 🟡 partial · ⬜ planned. Every ✅ cites the test.

## Priority backlog
_Nothing recorded yet._

## Tickets
> Permanent log of every change and fix — this repo's ticket system. Rows are append-only: never
> delete or reword one; only move its Status (open → closed / wont-do) and fill in Ref. Status is
> a word, not a glyph, so tickets don't count toward the story index. Keep this section last.

| ID | Date | Type | Status | Summary | Ref |
|---|---|---|---|---|---|
| M.MDIA-T1 | 2026-10-12 | docs | closed | Start the permanent ticket log in USER_STORIES.md | — |
| M.MDIA-T2 | 2026-10-14 | chore | closed | NUnit tests for MediaStreams.CopyAndHashAsync and MediaNaming.SanitizeFileName: hash and length match SHA256 of the source for empty, single-byte, exactly-buffer-size, and multi-buffer payloads, lowercase hex output, bufferSize of 0 or negative falls back to the default, cancellation propagates, a non-seekable source is read once; SanitizeFileName strips directories, replaces invalid chars with _, and returns file for empty/whitespace/dir-only input - these back every upload's hash and stored file name | 2bc3ccf |
| M.MDIA-T3 | 2026-10-14 | chore | wont-do | NUnit tests for AzureBlobUrlSigner user-delegation path (no Azurite needed, mock BlobServiceClient/container via AzureBlobContainerFactory seam): when the blob cannot generate a key SAS, a user-delegation SAS is built with Read permission, Resource b, StartsOn 5 min back, ContentType from the item and the account name; the delegation key is fetched once and reused while it has more than 5 min of validity, refreshed after, and concurrent callers share a single fetch; a null key declines (returns null); PublicRead with PublicBaseUri rebases onto the CDN host keeping the blob path, without it returns the raw blob URI - Entra-configured deployments rely on this branch and it is untested | portfolio reset: Azure decommissioned 2026-11-26; Azure signer work is out of scope |
| M.MDIA-T4 | 2026-10-14 | chore | open | NUnit tests for AddMedia DI wiring on the supported local-disk path: AddMedia registers a scoped LocalDiskMediaStore as IMediaStore and applies configureOptions (or the MediaStoreOptions defaults when null). Azure registration (AddMediaAzure, AzureBlobContainerFactory) is frozen and out of scope | — |
| M.MDIA-T5 | 2026-10-15 | chore | closed | Remove duplicate command copies (/quickload, /quicksave) now centralized at the user level (~/.claude/commands) | M.FLOW-T14 |
| M.MDIA-T6 | 2026-10-16 | chore | closed | install.ps1 adds this repo's 9 .prose/commands stubs for the shared global commands, so any LLM client finds them | mindattic-agent-standard/install.ps1 run 2026-10-16 |
| M.MDIA-T7 | 2026-10-16 | chore | closed | Bug hunt: run an exhaustive /bughunt over the whole repo and fix every real bug found (each fix under its own fix ticket), then hunt again; close this only when a full pass finds nothing | 5c18aad36f45e5e6b51ecf652d017a4766eea62a |
| M.MDIA-T8 | 2026-10-16 | chore | wont-do | Run the full unit test suite and fix every failure (each under its own fix ticket) until it passes; put the passing run in --ref. No test project: close this wont-do saying so | portfolio reset: repo-sweep test loop; tests run only on explicit owner request |
| M.MDIA-T9 | 2026-10-16 | chore | wont-do | Clean up code and docs: remove dead code, stale comments and outdated or duplicated docs, and simplify where behaviour doesn't change (/simplify); tests stay green | portfolio reset: generic cleanup, audit nice-to-have |
| M.MDIA-T10 | 2026-10-16 | docs | open | README.md: present the local-disk store (AddMedia) as the supported path and mark MindAttic.Storage.Azure as frozen after the 2026-11-26 Azure decommission (kept only for existing consumers, no new work); drop the Azure-first framing from the intro, diagram and quick start | user request 2026-10-16: approved repo sweep |
| M.MDIA-T11 | 2026-10-16 | docs | closed | Update USER_STORIES.md | — |
| M.MDIA-T12 | 2026-10-19 | chore | wont-do | Convert tracked source files to LF line endings via .gitattributes, per HOUSE-LAW-13 (low priority) | — |
| M.MDIA-T13 | 2026-10-20 | docs | closed | Publish MindAttic.Media docs to GitHub Pages via the shared Agency DocFX pipeline: run adopt-docs-site.ps1 (README.md as home page, docs/*.md as articles, API reference only where the code builds in CI), enable Pages with build_type=workflow, set the repo homepage URL, retire tools/build-readme.ps1 and README.htm, confirm the docs Actions run is green and https://mindattic.github.io/MindAttic.Media/ returns 200. Depends on MAT-T8. | — |
| M.MDIA-T14 | 2026-10-22 | docs | closed | Update USER_STORIES.md | — |
| M.MDIA-T15 | 2026-10-24 | docs | closed | Update USER_STORIES.md | — |
| M.MDIA-T16 | 2026-11-02 | docs | closed | Adopt MindAttic.Agency docs-site pipeline, publish README to GitHub Pages (HOUSE-LAW-14) | — |
| M.MDIA-T17 | 2026-11-02 | fix | closed | Bughunt sweep, two passes: find and fix real bugs across tracked source (user request 2026-11-02) | — |
| M.MDIA-T18 | 2026-11-02 | fix | closed | Fix the open items the 2026-11-02 bughunt left for decision (stored XSS headers, Pkcs pin, SanitizeFileName on Linux, partial upload cleanup) | — |
| M.MDIA-T19 | 2026-11-10 | chore | closed | Crash recovery: commit the command-catalog sync (.agent/commands/*.md) and regenerated docs/USER_STORIES.md left uncommitted by the session crash | — |
| M.MDIA-T20 | 2026-11-10 | chore | closed | Adopt the shared docs-site pipeline: turn on GitHub Pages | — |
| M.MDIA-T21 | 2026-11-17 | docs | closed | Update USER_STORIES.md | — |
| M.MDIA-T22 | 2026-11-24 | fix | wont-do | Hunt the whole repo for real bugs with no time limit and fix every confirmed one | portfolio reset: open-ended bughunt with no concrete defect |
| M.MDIA-T23 | 2026-11-24 | chore | closed | Owner-approved rename: MindAttic.Media becomes MindAttic.Storage (packages MindAttic.Storage and MindAttic.Storage.Azure 1.0.0, namespaces, projects, solution, folder, GitHub repo, docs-site, tickets key; prefix M.MDIA kept) and every consumer moves to the new packages | children done |
| M.MDIA-T24 | 2026-11-24 | chore | closed | The ASP.NET Core file-storage library ships as MindAttic.Storage / MindAttic.Storage.Azure from a repo, folder and docs-site named MindAttic.Storage | children done |
| M.MDIA-T25 | 2026-11-24 | chore | closed | Projects, namespaces and NuGet package ids are MindAttic.Storage(.Azure) 1.0.0, published to C:\LocalNuGet | children done |
| M.MDIA-T26 | 2026-11-24 | chore | closed | Rename projects, solution, namespaces and package ids MindAttic.Media(.Azure) -> MindAttic.Storage(.Azure) 1.0.0, build, and pack both to C:\LocalNuGet | d73f2c1 |
| M.MDIA-T27 | 2026-11-24 | chore | closed | The GitHub repo, local folder, docs-site and tickets registry key are MindAttic.Storage, its docs describe it, and no live MindAttic.Media reference is left in the workspace | children done |
| M.MDIA-T28 | 2026-11-24 | chore | closed | Rename GitHub repo mindattic/MindAttic.Media -> MindAttic.Storage, move D:\Projects\MindAttic\MindAttic.Media -> MindAttic.Storage, rename its tickets registry key (prefix M.MDIA kept), re-run the docs-site | gh rename |
| M.MDIA-T29 | 2026-11-24 | docs | closed | README, docs and docs-site describe MindAttic.Storage: package names, install, the Debug dual-reference path, and a 'formerly MindAttic.Media' note with the old -> new package map | 483acda |
| M.MDIA-T30 | 2026-11-24 | docs | closed | After every consumer task: a workspace sweep confirms no live MindAttic.Media / MindAttic.Media.Azure reference remains, and docs/MIGRATION.md records old -> new ids and the consumers moved (with ticket IDs) | 5c18aad |
| M.MDIA-T31 | 2026-11-27 | feature | open | The supported local-disk path is wired and tested (portfolio reset, M.EXPT-T48) | — |
| M.MDIA-T32 | 2026-11-27 | feature | open | AddMedia registers and configures the local store correctly | — |
| M.MDIA-T33 | 2026-11-27 | feature | open | Docs reflect that Azure is gone and point consumers at the local store (portfolio reset, M.EXPT-T48) | — |
| M.MDIA-T34 | 2026-11-27 | feature | open | README presents local disk as the supported store and the Azure package as frozen | — |
| M.MDIA-T35 | 2026-11-27 | feature | open | Consumers still on MindAttic.Storage.Azure know how to move to AddMedia | — |
| M.MDIA-T36 | 2026-11-27 | docs | open | docs/MIGRATION.md: name Prose (Prose.Core) and IdeaCms (IdeaCms.Blazor) as still referencing MindAttic.Storage.Azure 1.0.0 although Azure was decommissioned 2026-11-26, and give the switch to AddMedia<TContext> with a local MediaRoot as their follow-up | — |
