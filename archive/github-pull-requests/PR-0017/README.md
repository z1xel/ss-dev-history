# PR #17: refactor(1.12): reduce chat commands and clean legacy settings routes

**Original:** https://github.com/z1xel/SoftSpoken/pull/17  
**State:** Merged  
**Author:** z1xel  
**Created:** 2026-10-26T17:31:35Z  
**Updated:** 2026-10-26T18:09:25Z  
**Closed:** 2026-10-26T18:07:53Z  
**Merged:** 2026-10-26T18:07:53Z  
**Head:** chore/cleanup-legacy-112 (abbb13d6b434eebf88daa6556db048b655004372)  
**Base:** main (29200e1ea80b7040c24e94d96d8edc9eaaef4ed6)

## Original description

## Summary

Consolidates SoftSpoken's 1.12-facing command and settings surfaces without yet retiring the legacy Quests AceGUI fallback.

This PR combines the completed **cleanup Phases 1, 2, and 4**. **Phase 3 is intentionally deferred.** It is not a feature-complete unified-settings release.

### Phase 1 ΓÇö only two public commands
- `/spoken` opens/toggles unified Settings (no longer toggles playback).
- `/compendium` opens/toggles the shared Compendium.
- Removes public aliases/subcommands `/sp`, `/spq`, `/spokenquests`, `/spqread`, `/ssb`, `/ssz` and the old `/spoken ...` command dispatcher.
- Redirects Play/Pause, Skip, Stop All, and Compendium keybindings directly to the player API, so playback controls no longer depend on slash dispatch.
- Keeps minimap Settings and Compendium actions; Compendium entry points use the same guarded API.

### Phase 2 ΓÇö canonical settings routing
- Removes pending/dynamic standalone settings-link UI and redundant Quests settings-link registration.
- Uses the unified Home / Quests / Books / Zones page registration as the normal route.
- Keeps a harmless `Spoken:AddSettingsLink` compatibility shim for external callers.

### Phase 4 ΓÇö no diagnostic spam in chat
- Adds a read-only, scrollable/copyable diagnostic report window for Quests and Books.
- Diagnostic generators return lines internally; Quests developer collector no longer temporarily overrides `print`.
- Removes the unreachable `/spq followup` simulation helper and command-only labels (real NPC/quest follow-up handlers remain).
- Updates help/messages mentioning retired commands and displays the Books pack download address in a copyable window.

## Scope and deliberate exclusions
- **No Phase 3:** the old Quests AceGUI fallback, AceConfig/AceGUI libraries, and pack-management internals are retained. Do not delete them in this PR.
- SavedVariables, quest/book/zone discovery logic, sound pack registration and actual narration event handlers are intended to remain unchanged.
- The shared settings UI is unfinished; this is *cleanup*, not a full behavior release.

## Synchronized with current `main`

The cleanup branch was updated via merge commit [`abbb13d`](https://github.com/z1xel/SoftSpoken/commit/abbb13d6b434eebf88daa6556db048b655004372), incorporating all **three** upstream commits that landed after the cleanup work began:
- #14 ΓÇö centralized missing voice-pack notification (retained, including removal of the old Quests popup).
- Quests addon-list metadata fix (retained).
- #15 ΓÇö hide/retarget Compendium tabs when modules are disabled (retained).

At synchronization commit [`abbb13d`](https://github.com/z1xel/SoftSpoken/commit/abbb13d6b434eebf88daa6556db048b655004372), the branch had caught up with `main` at `29200e1`. **Since then, `main` advanced by one unrelated commit** (`3463a4b`, transparent Horde crest in the Quests Compendium) touching only `SoftSpoken_Quests/Assets/HordeCrest.tga` and `SoftSpoken_Quests/Compendium/QuestPage_112.lua`. These do not overlap the cleanup changes, and GitHub reports this PR **cleanly mergeable**. The five feature commits are preserved; the sixth commit synchronized earlier `main` work. The obsolete edit to the removed Quests popup's wording was intentionally dropped rather than resurrecting that popup.

## In-game verification ΓÇö NOT PERFORMED

- [x] Cold-login and `/reload` on Turtle WoW 1.12 with Quests, Books and Zones; no Lua errors.
- [x] `/spoken` opens/toggles Settings; `/compendium` opens/toggles Compendium.
- [x] Retired aliases and command subcommands no longer perform actions.
- [x] Play/Pause, Stop All, Skip and Compendium keybindings still function; Compendium default J behavior is unchanged.
- [x] Minimap left/middle/right click mappings and context menu work.
- [x] Quests/Books/Zones tabs open normally without duplicate settings links.
- [x] Quest/gossip/book/zone audio, installed pack enumeration, and real quest follow-up playback still work.
- [ ] Quests Diagnostics and Books Diagnostics open and copy correctly (especially long reports and scroll).
- [x] Test Voice Line still plays; missing pack notifications still appear correctly.
- [x] Disabling modules hides Compendium tabs as in #15; no reintroduced tabs after reload.
- [ ] Legacy Quests AceGUI fallback remains available when shared page setup fails.
- [x] Existing settings and histories survive `/reload` and character switch.

### Merge gate
**Do not merge solely on static checks.** No WoW client test or CI validation has been performed for this branch. GitHub currently reports a clean merge into `main`; recheck against the newest head and run the unperformed in-game tests before integration.

## Archived records

- [PR metadata](pull_request.json)
- [General conversation comments](conversation_comments.json) (0)
- [Reviews](reviews.json) (0)
- [Inline review comments](inline_review_comments.json) (0)
- [Changed files and patch fragments](changed_files.json) (20)
- [Commit references](commits.json) (6)
- [Timeline events](timeline.json) (10)
- [Readable conversations and reviews](DISCUSSION.md)

## Changed files

| File | Status | + | - |
| --- | --- | ---: | ---: |
| SoftSpoken/API.lua | modified | 23 | 6 |
| SoftSpoken/Bindings.xml | modified | 6 | 9 |
| SoftSpoken/Compendium/UI/Compendium.lua | modified | 0 | 6 |
| SoftSpoken/Core.lua | modified | 9 | 66 |
| SoftSpoken/Developer.lua | modified | 18 | 20 |
| SoftSpoken/Strings.lua | modified | 3 | 4 |
| SoftSpoken/UI/Diagnostics.lua | added | 113 | 0 |
| SoftSpoken/UI/MinimapButton.lua | modified | 1 | 4 |
| SoftSpoken/UI/Options.lua | modified | 3 | 36 |
| SoftSpoken/addon.xml | modified | 1 | 0 |
| SoftSpoken_Books/Core.lua | modified | 0 | 10 |
| SoftSpoken_Books/UI/Options.lua | modified | 27 | 13 |
| SoftSpoken_Quests/Debug.lua | modified | 8 | 15 |
| SoftSpoken_Quests/Followup.lua | modified | 0 | 52 |
| SoftSpoken_Quests/Options.lua | modified | 31 | 101 |
| SoftSpoken_Quests/Player.lua | modified | 3 | 8 |
| SoftSpoken_Quests/Strings.lua | modified | 3 | 16 |
| SoftSpoken_Quests/UI/DialogPlayButton.lua | modified | 2 | 2 |
| SoftSpoken_Quests/UI/SettingsPanel.lua | modified | 1 | 1 |
| SoftSpoken_Quests/VoiceOver.lua | modified | 0 | 10 |

