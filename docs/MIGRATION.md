# Migrating from MindAttic.Media

MindAttic.Media was renamed to MindAttic.Storage on 2026-11-24 (feature M.MDIA-T23). The API is unchanged: the public type names (`IMediaStore`, `MediaItem`, `AddMedia`, `AddMediaAzure`, `MapMediaEndpoints`) and the `/_media` endpoint stay as they were. Only the package ids, namespaces, folder and GitHub URL changed, and the package versions were reset to 1.0.0.

## Old to new

| What | Old | New |
|---|---|---|
| Core package | `MindAttic.Media` 1.0.0 / 2.0.0 | `MindAttic.Storage` 1.0.0 |
| Azure package | `MindAttic.Media.Azure` 1.0.0 / 2.0.0 | `MindAttic.Storage.Azure` 1.0.0 |
| Namespaces | `MindAttic.Media`, `MindAttic.Media.Azure` | `MindAttic.Storage`, `MindAttic.Storage.Azure` |
| Projects | `src\MindAttic.Media`, `src\MindAttic.Media.Azure`, `src\MindAttic.Media.Tests` | `src\MindAttic.Storage`, `src\MindAttic.Storage.Azure`, `src\MindAttic.Storage.Tests` |
| Solution | `MindAttic.Media.slnx` | `MindAttic.Storage.slnx` |
| Local folder | `D:\Projects\MindAttic\MindAttic.Media` | `D:\Projects\MindAttic\MindAttic.Storage` |
| GitHub | `github.com/mindattic/MindAttic.Media` | [github.com/mindattic/MindAttic.Storage](https://github.com/mindattic/MindAttic.Storage) |
| Tickets repo key | `MindAttic.Media` | `MindAttic.Storage` (prefix stays `M.MDIA`) |

The old `MindAttic.Media*.nupkg` files stay in `C:\LocalNuGet` so old commits still restore.

The names `MindAttic.Media.Viewer` and `MindAttic.Media.Capture` belong to other repos (the media viewer and the capture tool); they are not this library.

## Moving a consumer

1. In each `.csproj`, swap `PackageReference Include="MindAttic.Media"` for `MindAttic.Storage` 1.0.0 and `MindAttic.Media.Azure` for `MindAttic.Storage.Azure` 1.0.0. Point any Debug `ProjectReference` at `..\MindAttic.Storage\src\MindAttic.Storage\MindAttic.Storage.csproj` (and `MindAttic.Storage.Azure`), relative to your project.
2. Replace `using MindAttic.Media` with `using MindAttic.Storage`, and `using MindAttic.Media.Azure` with `using MindAttic.Storage.Azure`.
3. If an EF model snapshot names the entity `MindAttic.Media.MediaItem`, change it to `MindAttic.Storage.MediaItem` in the snapshot only; leave old migration `*.Designer.cs` files as they are. The table and columns don't change.
4. Build.

## Consumers moved

| Consumer | Change | Tickets | Commits |
|---|---|---|---|
| MindAttic.Storage (this repo) | Projects, solution, namespaces, package ids; GitHub repo, folder and tickets key; README | M.MDIA-T26, M.MDIA-T28, M.MDIA-T29 | d73f2c1, 0d2c5ec, 483acda |
| IdeaCms | `MindAttic.Media(.Azure)` 2.0.0 to `MindAttic.Storage(.Azure)` 1.0.0: package and Debug project references, namespaces; docs | IDEA-T72, IDEA-T73, IDEA-T74 | f84c0ac, a06eec2 |
| Prose | `MindAttic.Media(.Azure)` 1.0.0 to `MindAttic.Storage(.Azure)` 1.0.0: package ids, namespaces, EF snapshot entity name, vendored nupkgs | PRS-T1122, PRS-T1123 | 4805e2254 |
| Gadfly | Comments only (no package reference) | GAD-T732, GAD-T733 | 91e7a1f |
| MindAttic.Export | Docs and XML comments | M.EXPT-T45, M.EXPT-T46 | 1f4e2c5 |
| MindAttic.Log | `docs/MIGRATION.md` row | M.LOGS-T35, M.LOGS-T36 | d93f8f7 |
| MindAttic.Agency | Legacy rules: `MindAttic.Media__CLAUDE.md` became `MindAttic.Storage__CLAUDE.md` | MAT-T196 | 49d25cd |
| Workspace root | `docs/workspace-repos.json` entry, legacy rules mirror, command catalog | MAT-T199 | 30f3e95, 214d424 |

The closing sweep (M.MDIA-T30) searched every repo for `MindAttic\.Media(\.Azure)?([^.A-Za-z]|$)`. What still matches is dated record: ticket rows and descriptions, the Orchestrator's usage and test history, and design notes written before the rename.
