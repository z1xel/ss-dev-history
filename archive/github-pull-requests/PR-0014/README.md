# PR #14: Centralize missing voice-pack notification in SoftSpoken core

**Original:** https://github.com/z1xel/SoftSpoken/pull/14  
**State:** Merged  
**Author:** z1xel  
**Created:** 2026-10-26T16:34:17Z  
**Updated:** 2026-10-26T17:43:59Z  
**Closed:** 2026-10-26T16:49:13Z  
**Merged:** 2026-10-26T16:49:13Z  
**Head:** fix/centralized-missing-voice-pack-warning (6855a4c13c01fb581d25c1be4130896be143994b)  
**Base:** main (5a79acdfadecaacca4848c2dbb13a06042634c11)

## Original description

## Why
Quests showed a blocking **Spoken Quests** popup on login whenever its own registered sound packs were absent, even if Books or Zones had working packs. Its message was module-specific and suggested only Quests installation paths.

## Changes
- Remove the Quests-only startup popup and its gamepad-specific helper.
- Add **one** SoftSpoken-owned warning that lists only installed, enabled modules (Quests, Books, Zones) without usable audio packs.
- Use Quests' registered packs rather than merely discovered load-on-demand addon metadata; wait for its deferred data loader to finish before warning.
- Reuse Books and Zones' existing compatible-pack providers. A failed provider returns unknown and is not mistaken for missing audio.
- Preserve gamepad safety: use chat instead of a static popup when the gamepad UI is active.
- Keep warning one-time per login; no runtime playback logic changed.

## Manual test plan (Turtle WoW 1.12)
- [x] All three modules enabled, zero compatible packs: one **SoftSpoken** popup naming Quests, Books, Zones; no Quests popup.
- [x] Quests missing, Books pack present, Zones pack present: one popup naming Quests only.
- [x] Quests pack present, Books/Zones missing: popup names Books and Zones only.
- [x] All packs present: no popup.
- [x] An addon entirely uninstalled or module switched off: don't list it as missing.
- [x] Quests deferred load-on-demand succeeds: no premature warning; on failed load warn once after attempts.
- [x] `/reload`: one notification at most, never a popup flood.
- [ ] Gamepad UI: chat message, not a static popup.
- [x] Settings inventories and quest/book/zone narration still work normally.

**Not yet in-game tested.** Keep distinct from the ongoing legacy cleanup branch.

## Archived records

- [PR metadata](pull_request.json)
- [General conversation comments](conversation_comments.json) (0)
- [Reviews](reviews.json) (0)
- [Inline review comments](inline_review_comments.json) (0)
- [Changed files and patch fragments](changed_files.json) (5)
- [Commit references](commits.json) (5)
- [Timeline events](timeline.json) (10)
- [Readable conversations and reviews](DISCUSSION.md)

## Changed files

| File | Status | + | - |
| --- | --- | ---: | ---: |
| SoftSpoken/API.lua | modified | 77 | 0 |
| SoftSpoken/Core.lua | modified | 12 | 0 |
| SoftSpoken/Sources.lua | modified | 4 | 0 |
| SoftSpoken_Quests/Player.lua | modified | 5 | 0 |
| SoftSpoken_Quests/VoiceOver.lua | modified | 0 | 44 |

