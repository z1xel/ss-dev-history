# PR #16: Keep Books Compendium lore visible without voice packs

**Original:** https://github.com/z1xel/SoftSpoken/pull/16  
**State:** Closed (unmerged)  
**Author:** z1xel  
**Created:** 2026-10-26T17:09:40Z  
**Updated:** 2026-10-26T17:43:55Z  
**Closed:** 2026-10-26T17:18:29Z  
**Merged:**   
**Head:** fix/books-lore-without-voice-packs (6622e434ad86fc8b7b7a2d4e2aafddec530cb62b)  
**Base:** main (eddfece9fe7945193e2f9c9a06ad56b4ca961e5b)

## Original description

## Problem
Removing the optional Books sound-pack addon can make the Books Compendium look empty when a prior `Voiced Only` filter preference is enabled. Quests and Zones' bundled lore text continues to work without recordings; Books' text is also bundled in `SoftSpoken_Books/Data/Books.lua`.

## Fix
- Make Books' **Voiced Only** filter ineffective when there are no compatible packs with any recordings, without changing the stored user preference.
- Grey out the filter entry when unavailable, re-enabling it automatically when a pack returns.
- Invalidate cached pack availability when refreshing the list, without re-rendering the catalogue for every audio playback event.
- Extend the shared Compendium filter menu with an **optional** disabled callback. Other modules' filters are unchanged.
- Keep `Discovered Only`, `Unlock Unfound`, existing discovery/save data, and ordinary narration behavior as before.

## In-game test on WoW 1.12
- [ ] Set Books **Voiced Only** while a voice pack is installed, then disable/remove that pack and `/reload`. Books should still list writings; the Voiced Only option should be greyed out.
- [ ] Select a *discovered* writing. Its bundled text should appear on the parchment and the play control should be unavailable.
- [ ] With **Discovered Only** on, only discovered writings should show; with **Unlock Unfound** off, undiscovered writings should remain locked.
- [ ] Reinstall the pack and reload: Voiced Only becomes available and the prior saved choice applies again.
- [ ] Verify Quest and Places browsing, search, filter menu and playback remain unchanged.
- [ ] Confirm no Lua errors and no excessive list redraws during playback.

**Note:** this targets the identifiable audio-dependent *filtering* path. If Books is still empty with Voiced Only already off, troubleshoot corpus loading / discovery separately rather than weakening discovery restrictions.

**Status:** source review completed; in-game test outstanding.

## Archived records

- [PR metadata](pull_request.json)
- [General conversation comments](conversation_comments.json) (0)
- [Reviews](reviews.json) (0)
- [Inline review comments](inline_review_comments.json) (0)
- [Changed files and patch fragments](changed_files.json) (2)
- [Commit references](commits.json) (3)
- [Timeline events](timeline.json) (5)
- [Readable conversations and reviews](DISCUSSION.md)

## Changed files

| File | Status | + | - |
| --- | --- | ---: | ---: |
| SoftSpoken/Compendium/UI/Compendium.lua | modified | 6 | 1 |
| SoftSpoken_Books/UI/Readables.lua | modified | 24 | 1 |

