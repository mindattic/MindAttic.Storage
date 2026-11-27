---
layer: bible
status: living
---
# MindAttic.Storage — Project Bible

> Single source of truth for what MindAttic.Storage IS, is NOT, and the rules that keep it coherent.
> README says how to build and run it; this says how to think about it. The finish line lives in
> `docs/RELEASE.md`.

## 1. The one sentence {#M.MDIA-§1}
MindAttic.Storage is drop-in media storage for ASP.NET Core apps: one upload API, one catalog row per
file, and a `/_media/{uid}` endpoint that serves the file back. It was called MindAttic.Media until
d73f2c1 (see `docs/MIGRATION.md`).

## 2. The product promise {#M.MDIA-§2}
- **Two lines to add uploads.** `AddMedia<TContext>()`, `MediaItemTypeConfiguration` and
  `MapMediaEndpoints()` are all an app needs.
- **One stable id per file.** Every file is referred to by its `MediaItem.Uid`; the storage layout can
  always be rebuilt from the catalog row.
- **Streaming, not buffering.** Size and SHA-256 are computed in one pass while the upload streams.
- **Small files inline, large files on disk.** Payloads up to `InlineThresholdBytes` (2 MB) live in
  the row; larger ones spill to `MediaRoot/{uid}/{file}` (`ThresholdSpillStream`).
- **Safe to serve.** Every response carries `nosniff`; active content (HTML, SVG, XML, script) only
  ever downloads, with a `Content-Security-Policy: sandbox`.
- **No orphans.** A cancelled or failed upload removes its spill file or partial blob.
- **Soft deletes.** A delete flags the row; the stored file stays.

## 3. What it is NOT {#M.MDIA-§3}
- **NOT a cloud service.** Azure was decommissioned on 2026-11-26. `MindAttic.Storage.Azure` stays in
  the repo for the consumers that still reference it, but it is frozen: no new Azure work.
- **NOT database-agnostic.** The column mapping (`varbinary(max)`, `nvarchar(max)`) targets SQL Server.
- **NOT an image processor.** It records width and height when given; it does not resize or transcode.

## 4. Architecture canon {#M.MDIA-§4}
```
 app ── UploadAsync ──> IMediaStore ──> LocalDiskMediaStore ──> row (inline) | MediaRoot/{uid}/{file}
                                    └─> AzureBlobMediaStore  ──> container/{uid}/{file}   (frozen)
 GET /_media/{uid} ──> IMediaUrlSigner? ── yes ──> 302 to signed URL                     (frozen)
                                        └─ no  ──> stream (ETag, Last-Modified, Range)
```
### 4.1 Projects
- `src/MindAttic.Storage` — abstraction, EF model, local-disk store, endpoint (package 1.0.0).
- `src/MindAttic.Storage.Azure` — Azure Blob store and URL signer (package 1.0.0, frozen).
- `src/MindAttic.Storage.Tests` — NUnit: local store, endpoint, naming, streams, spill stream.

### 4.2 Domain model (the nouns)
`MediaItem` (uid, tenant, media type, folder, file name, content type, size, SHA-256, inline bytes or
blob URI, width, height, notes, soft-delete flag, timestamps, `Extra`, row version),
`MediaStoreOptions`, `AzureMediaOptions`.

### 4.3 Key services (the verbs)
`IMediaStore` (`UploadAsync`, `ListAsync`, `GetMetaAsync`, `GetAsync`, `DeleteAsync`),
`IMediaUrlSigner`, `MediaStreams.CopyAndHashAsync`, `MediaNaming.SanitizeFileName`,
`MapMediaEndpoints`.

## 5. The Laws {#M.MDIA-§5}
Inherits `MindAttic.HouseRules.md` by reference.
1. **The catalog row is the truth.** {#M.MDIA-LAW-1} Every stored file is reachable from its row.
2. **Never render active content inline.** {#M.MDIA-LAW-2} Script-capable types always download.
3. **Existing names keep resolving.** {#M.MDIA-LAW-3} A naming change keeps a legacy path
   (`MediaNaming.LegacySanitizeFileName`) for rows stored under the old rule.
4. **Public type names and `/_media` are stable.** {#M.MDIA-LAW-4} They survived the rename unchanged.

## 6. Verified state {#M.MDIA-§6}
- 1.0.0 packages for both projects are on `C:\LocalNuGet`; Prose and IdeaCms consume them.
- Rename from MindAttic.Media: d73f2c1; consumer mapping in `docs/MIGRATION.md` (5c18aad).
- Security and orphan hardening: fe7d3cb, 4817589.

## 7. Active frontier {#M.MDIA-§7}
See `docs/RELEASE.md`: the local-disk store as the supported path, with the Azure backend documented
as frozen.

## 8. Quality bar {#M.MDIA-§8}
A change is done when the solution builds, the local-disk path is covered by NUnit tests, and README
and MIGRATION.md match the behaviour.

## 9. Glossary {#M.MDIA-§9}
- **Spill** — moving a payload over the inline threshold from memory to `MediaRoot`.
- **Inline** — payload bytes stored in the `MediaItem` row itself.
- **Signer** — an `IMediaUrlSigner` that turns a row into a direct storage URL.
