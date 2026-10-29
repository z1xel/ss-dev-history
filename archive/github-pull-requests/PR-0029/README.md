# PR #29: Beta preparation: issues #23ΓÇô#26 and #30ΓÇô#34

**Original:** https://github.com/z1xel/SoftSpoken/pull/29  
**State:** Merged  
**Author:** z1xel  
**Created:** 2026-10-29T01:50:04Z  
**Updated:** 2026-10-29T03:07:32Z  
**Closed:** 2026-10-29T03:06:18Z  
**Merged:** 2026-10-29T03:06:18Z  
**Head:** prep/beta-release (c7b00a7a6685f14b88f04dc7ad8149f8bea94261)  
**Base:** main (c7c738e7de9859d541c5b6d24945a0b17eac20bf)

## Original description

## Scope
Addresses the initial beta issues z1xel/SoftSpoken-History#1ΓÇô#26 and the later UI polish issues z1xel/SoftSpoken-History#5ΓÇô#34.

### Fixes
- **#23 ΓÇö Writings tree:** Fix location-specific type branches so clicking Books or another writing type toggles `expandedType[key]` to the true state the renderer expects.
- **#24 ΓÇö Discovery unlock settings:** Remove the unlock toggles from Places, Stories and Writings Compendium filter dropdowns; expose Stories' existing `SSCQuestState:Setting("unlock")` via tab registration; add the Stories checkbox to Settings and Reset All; re-lock a selected undiscovered story when unlock is switched off.
- **#25 ΓÇö Vanilla settings viewport:** Refresh the native scroll rectangle, range and scrollbar whenever active content changes or tabs are swapped; clamp stale offsets; size the Home window to fit Voice Only where possible and restore normal size for other styles/tabs.
- **#26 ΓÇö Fallback identity:** Register as `SSS_VOFallback` to match the installed addon folder and TOC; update pack-preference tier, regression tests, documentation, and workflow trigger.


### Beta UI consistency (#30ΓÇô#34)
- **#30 ΓÇö Unlock controls:** Rename all Settings checkboxes to `Unlock Undiscovered <module>`; arrange **Stories ΓåÆ Places ΓåÆ Writings**.
- **#31 ΓÇö Settings navigation:** Reorder Home module cards and top-level settings tabs to **Stories ΓåÆ Places ΓåÆ Writings** (Home remains first).
- **#32 ΓÇö Reclick the active Compendium tab:** Use a transparent sibling click target above the native disabled selected-tab button. The active tab invokes its module's `onReselect` to clear only the selected entry and return to its introduction. Switching tabs preserves selections, filters, expansions and scroll positions; programmatic navigation is unchanged.
- **#33 ΓÇö Introductory parchment:** Stories, Places and Writings now share the same module heading, short category subtitle, ornamental divider and the `Select a ΓÇª to read ΓÇª and replay available recordings.` instruction pattern. The existing missing-catalog error page retains `empty=true`.
- **#34 ΓÇö Search:** All three tabs use the placeholder `Search`.

### Additional Turtle WoW 1.12 visual checks (still required)
5. All Compendium unlock labels and settings cards/tabs appear in **Stories ΓåÆ Places ΓåÆ Writings** order, with every module Settings button opening the correct page.
6. Select a quest, place and writing; click the already-active bottom tab again to restore its landing page and clear the highlighted row. Switch between different tabs to verify selected entries are preserved.
7. Check the active-tab reselect action on native Vanilla tabs: selected tabs may be disabled by Blizzard; verify the invisible click target reliably receives clicks while retaining the native active appearance.
8. Inspect all three landing pages for equal title/subtitle/divider/description layout at different Compendium widths and confirm ordinary selected-entry pages remain unchanged.
9. Search fields for Stories, Places and Writings all show only `Search`; searching and clearing still work.

### Automated checks
- `lua5.1 tests/audio_pack_priority_guardrails.lua` (includes actual fallback `Module.lua` registration, source aliases and quest/gossip paths).
- `lua5.1 tests/audio_pack_registry_compatibility.lua`.
- `luac5.1 -p` on each changed addon Lua module.
- 1,121 bundled MP3 recordings were counted (700 quest and 421 gossip); audio files were not edited.

### Turtle WoW 1.12 acceptance tests (still required)
1. Writings > Arrange by Zone > Darnassus > Books: open/close entries; repeat in other locations and with search/filter.
2. Settings > Compendium: three independent unlock checkboxes; show/hide entries immediately, persist on /reload; Reset All turns all off while retaining discovery history; inspect filters for absence of unlock entries.
3. Settings: Voice Only ΓåÆ another tab ΓåÆ Home ΓåÆ Large Window; bottom Keys reachable, no stale scrollbar; repeat without changing tabs and verify all three other module pages.
4. Stories fallback: verify the pack appears in Voice Packs and one fallback-only quest and gossip recording plays. Verify Spoken > SSS_VOFallback > Turtle with overlapping recordings.

**Draft intentionally:** changes are committed to `prep/beta-release`, not merged into `main`. Keep the issues open until their in-game acceptance checks have passed.

## Archived records

- [PR metadata](pull_request.json)
- [General conversation comments](conversation_comments.json) (0)
- [Reviews](reviews.json) (0)
- [Inline review comments](inline_review_comments.json) (0)
- [Changed files and patch fragments](changed_files.json) (13)
- [Commit references](commits.json) (25)
- [Timeline events](timeline.json) (39)
- [Readable conversations and reviews](DISCUSSION.md)

## Changed files

| File | Status | + | - |
| --- | --- | ---: | ---: |
| .github/workflows/audio-pack-regression.yml | modified | 18 | 0 |
| Data_Packs/SSS_VOFallback/Module.lua | modified | 3 | 3 |
| README.md | modified | 2 | 2 |
| SoftSpoken/Compendium/UI/Compendium.lua | modified | 31 | 0 |
| SoftSpoken/Strings.lua | modified | 5 | 3 |
| SoftSpoken/UI/Options.lua | modified | 50 | 13 |
| SoftSpoken_Places/UI/LoreWindow.lua | modified | 6 | 4 |
| SoftSpoken_Places/UI/Options.lua | modified | 1 | 1 |
| SoftSpoken_Stories/Compendium/QuestsTab_112.lua | modified | 18 | 5 |
| SoftSpoken_Stories/DataModules.lua | modified | 2 | 2 |
| SoftSpoken_Writings/UI/Options.lua | modified | 1 | 1 |
| SoftSpoken_Writings/UI/Readables.lua | modified | 7 | 5 |
| tests/audio_pack_priority_guardrails.lua | modified | 26 | 8 |

