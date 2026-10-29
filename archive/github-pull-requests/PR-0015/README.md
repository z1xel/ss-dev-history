# PR #15: Hide Compendium tabs when their modules are disabled

**Original:** https://github.com/z1xel/SoftSpoken/pull/15  
**State:** Merged  
**Author:** z1xel  
**Created:** 2026-10-26T17:03:00Z  
**Updated:** 2026-10-26T17:44:02Z  
**Closed:** 2026-10-26T17:16:42Z  
**Merged:** 2026-10-26T17:16:42Z  
**Head:** fix/compendium-tabs-follow-module-switches (d8a31287f024ef831c55cb13a5986fce94c6bd6d)  
**Base:** main (eddfece9fe7945193e2f9c9a06ad56b4ca961e5b)

## Original description

## Behavior
- **Quests** and **Places** now follow their main module enable switches, matching Books' existing behavior. Gossip remains independent.
- Changing a module switch immediately updates the Compendium's tab bar (no reload).
- Disabling the active tab switches to the first remaining enabled tab; disabling all three closes the Compendium.
- Re-enabling a module restores its tab. No quest history, discoveries, recordings, or settings are deleted.
- Direct Books/Places entry points and the shared tab opener refuse to open disabled modules, even through slash commands or map buttons.
- An enabled module **without a voice pack** still has a Compendium tab, since text browsing is independent of narration.

## In-game verification (Turtle WoW 1.12)
- [ ] Open each tab, turn off its module while active, verify automatic selection and immediate disappearance.
- [ ] Turn off a non-active module, verify current tab stays selected and remaining tabs shift with no visual glitches.
- [ ] Disable all modules, verify Compendium closes and global minimap/binding opener doesn't bring it back.
- [ ] Re-enable one module, open Compendium; re-enable all and verify correct order Quests ΓåÆ Places ΓåÆ Books.
- [ ] With one module enabled, verify its content remains accessible even if the single-tab button is hidden by existing 1.12 styling.
- [ ] Disable Zones, try zone map lore shortcut and /ssz; disable Books, try /ssb. They must not reopen a hidden tab.
- [ ] Disable Quests but leave Gossip enabled: Quests tab remains hidden, Gossip audio preference is unaffected.
- [ ] Enable a module with no sound pack: tab stays visible and its lore text is browsable.
- [ ] /reload and character switch preserve module switches and discovery histories.
- [ ] Check no Lua errors with any combination of installed/missing Quests, Books and Zones addons.

Static review: 4 changed files; no new assets, SavedVariables fields or audio engine modifications. This change has **not been tested in-game**.

## Archived records

- [PR metadata](pull_request.json)
- [General conversation comments](conversation_comments.json) (0)
- [Reviews](reviews.json) (0)
- [Inline review comments](inline_review_comments.json) (0)
- [Changed files and patch fragments](changed_files.json) (4)
- [Commit references](commits.json) (4)
- [Timeline events](timeline.json) (9)
- [Readable conversations and reviews](DISCUSSION.md)

## Changed files

| File | Status | + | - |
| --- | --- | ---: | ---: |
| SoftSpoken/Compendium/UI/Compendium.lua | modified | 41 | 3 |
| SoftSpoken_Books/UI/Readables.lua | modified | 2 | 0 |
| SoftSpoken_Quests/Compendium/QuestsTab_112.lua | modified | 3 | 1 |
| SoftSpoken_Zones/UI/LoreWindow.lua | modified | 6 | 3 |

