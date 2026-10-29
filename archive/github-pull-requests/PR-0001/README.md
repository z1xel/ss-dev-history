# PR #1: Centralize Compendium and language infrastructure; clean legacy code

**Original:** https://github.com/z1xel/SoftSpoken/pull/1  
**State:** Merged  
**Author:** z1xel  
**Created:** 2026-10-19T20:34:12Z  
**Updated:** 2026-10-22T02:35:41Z  
**Closed:** 2026-10-22T02:34:47Z  
**Merged:** 2026-10-22T02:34:47Z  
**Head:** refactor/core-compendium-foundation-20261019 (47d6571947c7b4adeefac55050d5852df08ad58a)  
**Base:** main (21e1d75885c7d7eb796d91a89d6f1ab1dadb30db)

## Original description

## Changes
- Centralize Places, Books and Quests parchment/inset and scrollbar construction in the core's `Compendium/UI/PagePane.lua`. Original frame offsets and per-tab page state preserved.
- Store shared Compendium window geometry in core per-character settings; lazily migrate width/height from prior module-owned storage.
- Correct outdated `SpokenCompendium` references in core options and Quest source UI updates.
- Remove inactive legacy Zones UI/captions, old Books commands and dormant core code.
- Add architecture/data-flow documentation.

## Quests loading regression repair
- **Fix addon discovery:** `SoftSpoken_Quests/` previously contained `Spoken_Quests.toc`, which did not match the directory name required by WoW's legacy addon scanner. It is now named `SoftSpoken_Quests.toc`.
- **Restore local libraries:** revert the initial removal of Quests' LibStub, CallbackHandler, AceCore, AceDB and AceTimer files and original `embeds.xml`. Library deduplication is postponed to a separate, independently tested refactor.
- The Quests TOC's `## Dependencies: SoftSpoken` remains unchanged.
- All Quests TOC and nested XML references now resolve to files that exist in the branch.

## Static checks
- Checked manifest load order, shared PagePane references across three modules and original UI placement offsets.
- Checked Quests' corrected TOC name and every direct manifest file path.
- **Turtle WoW 1.12 runtime tests have not yet been performed by the assistant.**

## In-game regression checklist
- [x] Verify SoftSpoken_Quests appears in the character-select AddOns list when installed as `Interface/AddOns/SoftSpoken_Quests/`.
- [x] Verify the Quests and Gossip source registration and Quests Compendium tab.
- [x] Verify Quests audio packs and optional `SoftSpoken_QuestData` companion if installed.
- [x] Core + Zones: explore, autoplay and use Places tab.
- [x] Core + Books: read physical pages and browse Books tab.
- [x] Switch all tabs, resize window and reload; compare to approved geometry.

**Keep draft until the corrected Quests addon has been tested in the client.**

## Follow-up commit: shared language and voice-state coherence
- Move the eleven supported voice languages, client locale rules, and voice/fallback resolution into `SoftSpoken/Language.lua` (`Spoken.Language`).
- Keep Books and Quests' established `Language` method names through small adapters, preserving existing SavedVariables and old English-pack fallbacks.
- Route core language selections through `Spoken:SetLanguageChoice`, emitting `VOICE_LANGUAGE_CHANGED`, including when AceDB switches a whole profile.
- Use `Spoken:NotifyVoicePacksChanged()` instead of directly reaching into `SpokenEnv.Callbacks` from Quests.
- Invalidate the Quests Compendium's `Voiced Only` result cache after deferred/manual pack loads, shared language changes, and quest profile switches.
- Skip the Zones play-button polling loop on current core builds; retain the 0.5-second fallback when `Spoken:RegisterCallback` isn't available.

### Additional static checks
- Confirmed all core XML dependencies resolve and shared language loads before the core settings UI.
- Confirmed one authoritative language list and preserved adapter method names.
- Verified notification paths and the old-client poll fallback.
- In-game validation still needed, especially non-English/fallback selection and newly loaded Quest audio packs.

## Archived records

- [PR metadata](pull_request.json)
- [General conversation comments](conversation_comments.json) (0)
- [Reviews](reviews.json) (0)
- [Inline review comments](inline_review_comments.json) (0)
- [Changed files and patch fragments](changed_files.json) (39)
- [Commit references](commits.json) (5)
- [Timeline events](timeline.json) (12)
- [Readable conversations and reviews](DISCUSSION.md)

## Changed files

| File | Status | + | - |
| --- | --- | ---: | ---: |
| SoftSpoken/API.lua | modified | 27 | 0 |
| SoftSpoken/Compendium/UI/Compendium.lua | modified | 31 | 1 |
| SoftSpoken/Compendium/UI/PagePane.lua | added | 28 | 0 |
| SoftSpoken/Core.lua | modified | 3 | 0 |
| SoftSpoken/Language.lua | added | 93 | 0 |
| SoftSpoken/OtherSounds.lua | removed | 0 | 209 |
| SoftSpoken/UI/Options.lua | modified | 7 | 29 |
| SoftSpoken/addon.xml | modified | 2 | 0 |
| SoftSpoken_Books/Commands.lua | removed | 0 | 60 |
| SoftSpoken_Books/Language.lua | modified | 15 | 101 |
| SoftSpoken_Books/UI/Readables.lua | modified | 5 | 24 |
| SoftSpoken_Quests/Compendium/QuestsTab_112.lua | modified | 17 | 19 |
| SoftSpoken_Quests/DataModules.lua | modified | 4 | 0 |
| SoftSpoken_Quests/Language.lua | modified | 22 | 150 |
| SoftSpoken_Quests/Libs/AceCore-3.0/AceCore-3.0.lua | removed | 0 | 209 |
| SoftSpoken_Quests/Libs/AceCore-3.0/AceCore-3.0.xml | removed | 0 | 4 |
| SoftSpoken_Quests/Libs/AceDB-3.0/AceDB-3.0.lua | removed | 0 | 755 |
| SoftSpoken_Quests/Libs/AceDB-3.0/AceDB-3.0.xml | removed | 0 | 4 |
| SoftSpoken_Quests/Libs/AceTimer-3.0/AceTimer-3.0.lua | removed | 0 | 379 |
| SoftSpoken_Quests/Libs/AceTimer-3.0/AceTimer-3.0.xml | removed | 0 | 4 |
| SoftSpoken_Quests/Libs/CallbackHandler-1.0/CallbackHandler-1.0.lua | removed | 0 | 280 |
| SoftSpoken_Quests/Libs/CallbackHandler-1.0/CallbackHandler-1.0.xml | removed | 0 | 4 |
| SoftSpoken_Quests/Libs/LibStub/LibStub.lua | removed | 0 | 33 |
| SoftSpoken_Quests/Player.lua | modified | 2 | 1 |
| SoftSpoken_Quests/SoftSpoken_Quests.toc | renamed | 0 | 0 |
| SoftSpoken_Quests/VoiceOver.lua | modified | 5 | 1 |
| SoftSpoken_Quests/embeds.xml | modified | 0 | 5 |
| SoftSpoken_Zones/Captions.lua | removed | 0 | 146 |
| SoftSpoken_Zones/UI/ArtReady.lua | removed | 0 | 3 |
| SoftSpoken_Zones/UI/AudioButton.lua | modified | 14 | 12 |
| SoftSpoken_Zones/UI/ClassicFrameSkin.lua | removed | 0 | 169 |
| SoftSpoken_Zones/UI/Compendium.lua | removed | 0 | 830 |
| SoftSpoken_Zones/UI/HighlightReady.lua | removed | 0 | 3 |
| SoftSpoken_Zones/UI/Layout.lua | removed | 0 | 219 |
| SoftSpoken_Zones/UI/LorePage.lua | removed | 0 | 335 |
| SoftSpoken_Zones/UI/LoreWindow.lua | modified | 4 | 23 |
| SoftSpoken_Zones/UI/NativeQuestSkin.lua | removed | 0 | 64 |
| SoftSpoken_Zones/UI/TextView.lua | removed | 0 | 305 |
| docs/ARCHITECTURE.md | added | 110 | 0 |

