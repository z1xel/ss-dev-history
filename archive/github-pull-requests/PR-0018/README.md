# PR #18: Reuse Compendium parchment skin for World Map Zone Lore attachment

**Original:** https://github.com/z1xel/SoftSpoken/pull/18  
**State:** Merged  
**Author:** z1xel  
**Created:** 2026-10-26T18:29:40Z  
**Updated:** 2026-10-26T23:12:03Z  
**Closed:** 2026-10-26T23:11:18Z  
**Merged:** 2026-10-26T23:11:18Z  
**Head:** feat/map-lore-compendium-parchment (2adfeed2e5f2ac42f7b0552a3034d991d75511e5)  
**Base:** main (d2961b8dc2a423c9d3cc9e9cede9efbceeede793)

## Original description

## Merge summary

Unify the **World Map's attached Zone Lore panel** with Azeroth's Compendium's right-hand parchment and outer frame, while preserving the existing map selection, audio and discovery code.

### Final implementation
- Reuse the shared Compendium parchment, inset border, rock backdrop and native-style scrollbar through `CreateStandalonePagePane`.
- Use the same book-style lore rendering with an optional **Open in Compendium** button and a wrapped-subtitle option for the narrow map attachment.
- Add `compact` mode to `SSCPrototypeFrameSkin`: original Blizzard silver corner/title assets, tiled vertical sides, rock backdrop, native bottom trim, no Compendium portrait/minimizer, and a compact close button that collapses the map attachment.
- Anchor the compact bottom rail/corners to the actual panel edges, measure the anchored panel's visible size and refresh the side strips when geometry changes.
- Keep the close button and all its normal/pressed/hover visuals on the same hit region, above the compact decorative trim.
- Keep the map's original width, docked/overlay positioning, collapse toggle, and existing lore/discovery data paths.

**Scope of new work relative to current `main`:** four source files: `LorePage.lua`, `PagePane.lua`, `PrototypeFrameSkin.lua`, `SoftSpoken_Zones/UI/MapPanel.lua`. GitHub's PR comparison also lists `Compendium.lua` and `MinimapButton.lua` because their earlier commits are already present **unchanged** on `main`; this PR introduces no further changes to those files.

### In-game visual review
- [x] Shared parchment, silver title and stone background visually match the Compendium.
- [x] Top corner geometry and silver frame appearance reviewed in screenshots.
- [x] Both vertical side trims now continue to the bottom; user confirmed the fix.
- [x] Close-button hover/click offset addressed; user approved the final appearance.
- [ ] Full behavior test: zone, city and subzone selection, including breadcrumbs and **Open in Compendium**.
- [ ] Full scrolling test: wheel, arrows, thumb, bottom limit and switching between short/long lore.
- [ ] Panel collapse/reopen, map close/reopen, and reload persistence.
- [ ] Docked-outside and inset overlay positions at alternate UI scales/resolutions.
- [ ] Compare the original full-size Compendium's close button, borders and scrolling for regressions.
- [ ] Confirm narration/Play controls still function, including absent voice-pack cases.

### Pre-merge technical review
- [x] GitHub reports mergeability **clean** against current `main`.
- [x] No SavedVariables schema, lore data or audio engine changes.
- [x] The normal full Compendium framing remains on the pre-existing noncompact path.
- [x] The seven newer `main` commits were reviewed; their only overlap is the unrelated Places map-toggle icon, and GitHub reports no conflict.
- [ ] No automated checks or submitted reviews exist for this PR. Manual functional verification remains outstanding.

**Merge approach:** squash is appropriate for the iterative visual-fix commit history. Do not conflate the successful user visual check with full behavior QA.

## Archived records

- [PR metadata](pull_request.json)
- [General conversation comments](conversation_comments.json) (7)
- [Reviews](reviews.json) (0)
- [Inline review comments](inline_review_comments.json) (0)
- [Changed files and patch fragments](changed_files.json) (6)
- [Commit references](commits.json) (18)
- [Timeline events](timeline.json) (28)
- [Readable conversations and reviews](DISCUSSION.md)

## Changed files

| File | Status | + | - |
| --- | --- | ---: | ---: |
| SoftSpoken/Compendium/UI/Compendium.lua | modified | 5 | 1 |
| SoftSpoken/Compendium/UI/LorePage.lua | modified | 17 | 4 |
| SoftSpoken/Compendium/UI/PagePane.lua | modified | 32 | 9 |
| SoftSpoken/Compendium/UI/PrototypeFrameSkin.lua | modified | 165 | 17 |
| SoftSpoken/UI/MinimapButton.lua | modified | 2 | 2 |
| SoftSpoken_Zones/UI/MapPanel.lua | modified | 31 | 65 |

