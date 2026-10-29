# PR #21: Use Blizzard Spellbook side-tab artwork for Places map button

**Original:** https://github.com/z1xel/SoftSpoken/pull/21  
**State:** Merged  
**Author:** z1xel  
**Created:** 2026-10-26T23:26:14Z  
**Updated:** 2026-10-26T23:34:34Z  
**Closed:** 2026-10-26T23:28:16Z  
**Merged:** 2026-10-26T23:28:16Z  
**Head:** ui/spellbook-map-side-tab (19a04814271695ffd4e9290b465bb1e4c5e092e1)  
**Base:** main (119fd528205f356bcacba80e6a939b3f724dd2e3)

## Original description

## Summary
- Replace the generic quickslot border on the World Map's Places toggle with Blizzard's native `SpellBook-SkillLineTab` metal housing.
- Keep the original bundled `INV_Misc_Map02.tga` Places icon.
- Align the button to the map's right edge at the user-tuned `(-2, -75)` offset, keeping the fullscreen fallback and existing collapse/tooltip behavior.

## Integration
- Reconcile this branch with the latest `main`, including recent standalone Zone Lore compact-frame/parchment changes, before merging.
- No new assets, no change to the map's native geometry.

## Verification
- Confirm that only `SoftSpoken_Zones/UI/MapPanel.lua` changes against `main`.
- Verify on the client after merging: World Map side tab, collapsed panel reopen, windowed/fullscreen positioning.
- Visual confirmation of tab positioning has been user-reviewed during branch iteration; no automated in-game test is available.

## Archived records

- [PR metadata](pull_request.json)
- [General conversation comments](conversation_comments.json) (0)
- [Reviews](reviews.json) (0)
- [Inline review comments](inline_review_comments.json) (0)
- [Changed files and patch fragments](changed_files.json) (1)
- [Commit references](commits.json) (6)
- [Timeline events](timeline.json) (9)
- [Readable conversations and reviews](DISCUSSION.md)

## Changed files

| File | Status | + | - |
| --- | --- | ---: | ---: |
| SoftSpoken_Zones/UI/MapPanel.lua | modified | 12 | 10 |

