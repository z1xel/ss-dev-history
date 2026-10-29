# PR #7: Integrate QuestData and QuestText into SoftSpoken_Quests

**Original:** https://github.com/z1xel/SoftSpoken/pull/7  
**State:** Merged  
**Author:** z1xel  
**Created:** 2026-10-23T18:00:55Z  
**Updated:** 2026-10-23T18:14:28Z  
**Closed:** 2026-10-23T18:04:43Z  
**Merged:** 2026-10-23T18:04:43Z  
**Head:** refactor/merge-quest-databases (a3d8709ca446e66b53061f5ed733a5391c6c622f)  
**Base:** main (088289b3af4b301eb4595619931bf6f30fe55dd3)

## Original description

## Summary
- Move the original quest catalog and quest dialogue datasets into `SoftSpoken_Quests/Data/`, preserving both Git blob SHAs exactly (no database edits).
- Include the datasets through `addon.xml` before the Compendium UI scripts.
- Remove `LoadAddOn("SoftSpoken_QuestData")` and `LoadAddOn("SoftSpoken_QuestText")` in favor of normal startup loading.
- Remove the two obsolete companion `.toc` files and document the unified layout.

## Verification
- GitHub recognizes both database moves as exact **renames, 0 additions/deletions**.
- Checked `addon.xml` ordering and the removed companion folder paths.
- No overlap with the newer `main` commit that edited `SoftSpoken/Compat.lua`, `SoftSpoken_Quests/Compatibility.lua`, and `SoftSpoken_Quests/Version.lua`.
- **Not yet tested inside Turtle WoW 1.12.** Please keep this PR in draft until in-game validation.

## Manual test plan
1. Check out this branch in the local SoftSpoken repository, with Turtle WoW's four SoftSpoken addon junctions pointing at it.
2. Remove the obsolete `SoftSpoken_QuestData` and `SoftSpoken_QuestText` junctions from `Interface/AddOns` (do not delete the corresponding repository data).
3. Start the client; ensure Quests, gossip and player initialize without Lua errors.
4. Open the Quests Compendium; test categories, filters, counts, chains, discovered/completed states.
5. Open a quest and verify description, progress, and completion text, plus narration/playback where voice packs are installed.
6. Repeat with no audio packs enabled to ensure the Quest Compendium still works.

**Expected tradeoff:** the two static databases now load at startup instead of upon opening their respective Compendium views.

## Archived records

- [PR metadata](pull_request.json)
- [General conversation comments](conversation_comments.json) (0)
- [Reviews](reviews.json) (0)
- [Inline review comments](inline_review_comments.json) (0)
- [Changed files and patch fragments](changed_files.json) (8)
- [Commit references](commits.json) (1)
- [Timeline events](timeline.json) (10)
- [Readable conversations and reviews](DISCUSSION.md)

## Changed files

| File | Status | + | - |
| --- | --- | ---: | ---: |
| SoftSpoken_QuestData/SoftSpoken_QuestData.toc | removed | 0 | 8 |
| SoftSpoken_QuestText/SoftSpoken_QuestText.toc | removed | 0 | 8 |
| SoftSpoken_Quests/Compendium/QuestPage_112.lua | modified | 0 | 5 |
| SoftSpoken_Quests/Compendium/QuestsTab_112.lua | modified | 5 | 6 |
| SoftSpoken_Quests/Data/Compendium.lua | renamed | 0 | 0 |
| SoftSpoken_Quests/Data/QuestText.lua | renamed | 0 | 0 |
| SoftSpoken_Quests/addon.xml | modified | 3 | 0 |
| docs/ARCHITECTURE.md | modified | 3 | 0 |

