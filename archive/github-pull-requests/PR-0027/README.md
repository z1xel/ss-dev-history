# PR #27: Fix World Map lore text overflowing its parchment on WoW 1.12

**Original:** https://github.com/z1xel/SoftSpoken/pull/27  
**State:** Merged  
**Author:** z1xel  
**Created:** 2026-10-29T01:11:31Z  
**Updated:** 2026-10-29T01:38:20Z  
**Closed:** 2026-10-29T01:17:50Z  
**Merged:** 2026-10-29T01:17:50Z  
**Head:** fix/vanilla-lore-scroll-height (586ccb0c33cd1b9fb8a6811cb726d70b95272a6e)  
**Base:** main (289665b9cf77a5a237a8944d10bc49e77d246bf1)

## Original description

## Problem
On Turtle WoW 1.12, long Zone Lore text in the World Map's attached panel renders past the bottom of the parchment/frame. A manual `page:Resize(true)` and `UpdateScrollChildRect()` did not resolve it.

## Suspected root cause
The shared `TextView:Reflow()` calls `FontString:GetStringHeight()` unconditionally. That API was introduced in WoW 2.3, so a native 1.12 client may error before the scroll child's height and range are updated. The same post-1.12 method is used when measuring wrapped map-panel subtitles.

## Changes
- Prefer `GetStringHeight` where available and otherwise measure the auto-sized text with `GetHeight` (1.12-compatible).
- Refresh the scroll child's rectangle after setting its height and before restoring the scroll offset.
- Give only the standalone/map subtitle an auto-height font string and measure its wrapped height without requiring a 2.3 API.
- No changes to map geometry, skins, lore data, discovery, or audio.

## Live verification required
1. Pull/checkout this branch, `/reload`, open World Map > Zone Lore > Darnassus.
2. Confirm long text is clipped at the parchment bottom and can be revealed by scrolling instead of spilling onto the world.
3. Check other long zone and subzone entries; scroll to bottom and back; change maps; resize/switch fullscreen/windowed map if supported.
4. Check that the same text reader inside Azeroth's Compendium still scrolls properly and the breadcrumb remains positioned above the divider.

The patch has been source-reviewed only; in-game verification is outstanding.

## Archived records

- [PR metadata](pull_request.json)
- [General conversation comments](conversation_comments.json) (0)
- [Reviews](reviews.json) (0)
- [Inline review comments](inline_review_comments.json) (0)
- [Changed files and patch fragments](changed_files.json) (2)
- [Commit references](commits.json) (2)
- [Timeline events](timeline.json) (5)
- [Readable conversations and reviews](DISCUSSION.md)

## Changed files

| File | Status | + | - |
| --- | --- | ---: | ---: |
| SoftSpoken/Compendium/UI/LorePage.lua | modified | 16 | 2 |
| SoftSpoken/Compendium/UI/TextView.lua | modified | 14 | 1 |

