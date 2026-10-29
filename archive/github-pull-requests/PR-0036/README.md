# PR #36: Integrate Darnassus terrain-based map fallback and native-style highlights

**Original:** https://github.com/z1xel/SoftSpoken/pull/36  
**State:** Closed (unmerged)  
**Author:** z1xel  
**Created:** 2026-10-29T05:19:18Z  
**Updated:** 2026-10-29T16:13:14Z  
**Closed:** 2026-10-29T16:13:13Z  
**Merged:**   
**Head:** feature/darnassus-terrain-map-fallback (a1de2c4aae3b1143a90fcd80db060f8085434e4d)  
**Base:** main (9673bf020d2add2bfab12ea6b85b65d3af0513bd)

## Original description

## Scope (Darnassus only)

Integrates the terrain lookup proven in `SSZTerrainProbe` directly into SoftSpoken Places, without changing upstream map geometry, map navigation or discovery writes.

- Add `UI/TerrainMap.lua` with the Turtle ADT-derived 33x22 area grid and five Darnassus district keys.
- Add a last-resort `terrain` result to `SSZMapCompat.ResolveAt` **after** POI/native/precise/city/unique-overlay resolution.
- Extend the existing `SSZMapHighlight` hover manager to render one smoothed BLP2/DXT1 additive highlight at 100% texture alpha, while leaving the existing ClassicAPI overlay path at its original 0.35 frame alpha.
- Bundle five generated area masks under `Textures/TerrainHighlights`, using the terrain-derived silhouette and a Gaussian round/feather pass, with original-1.12-style dark additive RGB light.
- Preserve the existing `SSZMapPanel.SelectSubzone` and `SSZLoreWindow.IsLocked` checks. Map hover and clicks never write exploration/visited state.
- Add static source/BLP header tests and manual acceptance steps in `tools/map/DARNASSUS_TERRAIN_FALLBACK.md`.

## Validation

- [x] Verified the row grid dimensions/area counts against the independently tested probe source.
- [x] Verified the previously tested Cenarion sample `(0.3543013,0.0978802)` resolves to the Cenarion Enclave chunk.
- [x] Confirmed the module loads before `MapCompat` and the existing native/overlay path has precedence in source.
- [ ] **In-game on Turtle WoW 1.12:** confirm all five names, clicks, lore locks, hover highlights, native POI precedence and fullscreen/minimized switching.
- [ ] **In-game:** confirm BLP2/DXT1 highlight textures load and light properly, and ordinary overlay zones remain unchanged.

**Do not merge until the in-game verification passes.** The current branch intentionally does not attempt to fix the entire 219-place map-gap audit; that work should follow this proven integration.

## Archived records

- [PR metadata](pull_request.json)
- [General conversation comments](conversation_comments.json) (0)
- [Reviews](reviews.json) (0)
- [Inline review comments](inline_review_comments.json) (0)
- [Changed files and patch fragments](changed_files.json) (2)
- [Commit references](commits.json) (111)
- [Timeline events](timeline.json) (113)
- [Readable conversations and reviews](DISCUSSION.md)

## Changed files

| File | Status | + | - |
| --- | --- | ---: | ---: |
| tools/map/DARNASSUS_TERRAIN_FALLBACK.md | added | 60 | 0 |
| tools/map/test_darnassus_fallback.py | added | 86 | 0 |

