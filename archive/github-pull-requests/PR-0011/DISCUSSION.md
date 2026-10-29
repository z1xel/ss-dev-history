# PR #11: conversations and reviews

[Original PR](https://github.com/z1xel/SoftSpoken/pull/11) | [Archive summary](README.md)

## General discussion

### z1xel on 2026-10-25T19:02:46Z

[Original comment](https://github.com/z1xel/SoftSpoken/pull/11#issuecomment-6401849469)

## Pre-merge audit ΓÇö 2026-10-25
**Scope:** reviewed the complete 124-commit history and the final 20-file PR diff against `main` (no commits behind). Code/metadata inspection only; **zero in-game tests or automated checks are assumed**. The project contains no checked-in test/workflow files. One confirmed legacy empty-minimap-mapping migration omission was fixed in `eb179634` before merge. The existing logo TGA also changed and needs visual inspection.

**Known incomplete work (not silently certified):** the unified Quests tab does **not** expose the legacy AceGUI sound-pack Load button or profile manager during normal operation. The old AceGUI implementation is kept as a fallback only if the shared page fails. Language/profile/report-visibility controls and voice-pack downloads are intentionally disabled; the current legacy window skin is unfinished. Report is hidden by default for profiles without a stored override. Minimap behavior and the J binding are intentionally changed.

### Post-merge 1.12 in-game acceptance tests
All unchecked by design; record precise error text, UI scale, installed packs and /reload outcome on failure.

**P0 ΓÇö startup, regression and persisted data**
- [ ] Back up WTF/SavedVariables and verify a cold login with SoftSpoken, Quests, Books, Zones and ClassicAPI; **no Lua errors** before or after `/reload`.
- [ ] Repeat with one or more optional voice packs absent/disabled; no startup error or missing-tab crash. Test another character/account profile.
- [ ] Confirm existing Spoken, Books, Zones and Quests preferences/history survive install, reload and character switch.
- [ ] Verify actual quest acceptance/progress/completion, NPC gossip, book-page and zone/subzone narration. Test play/stop/skip, combat gating and no clip/incorrect source fallback.
- [ ] Open/close unified Settings through slash, minimap and module Settings buttons; X/Escape close correctly; all four tabs open and switching doesn't crash.
- [ ] Verify existing native quest-log Play/Stop controls, Compendium tabs/pages and world-map lore viewer still work.

**P1 ΓÇö Home, navigation, bindings and minimap**
- [ ] Toggle Quests, Gossip, Books and Zones independently; verify their real audio gates and correct checkboxes after `/reload`.
- [ ] Toggle Large Window and Voice Only; verify narrator Preview, playback, narrator window display and persistence.
- [ ] Search by settings labels on Home, Quests, Books and Zones; clear search, Enter/click result, no-results, gold row highlight, and focus cleared on close/tab switch.
- [ ] Scroll each tab to the last control and back: no clipped labels, overshooting scrollbar, overlapping cards or disappearing rows at normal and unusual UI scales/resolutions.
- [ ] Test disabled Language, Profiles, Report visibility and Quests download controls: dim/red notice, no tooltip, hover, click, dropdown or invisible shield blocking neighboring rows.
- [ ] Confirm Compendium key defaults to **J** only if J is unbound; otherwise existing binding wins. Rebind/clear it and `/reload`, including action toggling Compendium.
- [ ] Rebind Play/Pause, Stop, Skip; verify conflict confirmation, clearing bindings, and `/reload` persistence.
- [ ] Reassign minimap Left/Middle/Right to Settings/Compendium/Menu; test all nine combinations and `/reload`. Test legacy saved PlayPause/Stop/**empty** mappings; no dead button.
- [ ] Check minimap click/context menu after reload: no Lua 5.0 closure or SOUNDKIT errors; retired duplicate Quests/Gossip menu entries do not reappear.
- [ ] Verify minimap custom icon cropping, dragging/locking, hide/show and correct click target.

**P1 ΓÇö module pages and voice packs**
- [ ] Quests: installed inventory includes **all seven** expected packs (including SoftSpoken), with correct addon/title/version/format/priority/language/load state. Expand/collapse all entries.
- [ ] Quests: disabled Voice Pack Downloads remain visible but inert; Test Voice Line and Diagnostics still function.
- [ ] Quests: verify `/spq options` and old AceGUI fallback behavior; note that old manual Load/profile controls are not exposed on the successful shared-tab route (follow-up work).
- [ ] Quests & Gossip: autoplay/followup/read-after-close/stop-on-close/greeting-first/original Thrall/Gossip Frequency change the intended behavior and persist.
- [ ] Books: pack inventory and recorded-page counts match installed data; no-pack fallback, audio language labels, expand/collapse and load status.
- [ ] Books: autoplay/read-once/stop-on-close/close-when-read behave in-world and in Compendium; disabled Books still allows reading-history and Diagnostics actions.
- [ ] Books: Forget What Was Read confirmation clears **read** history but keeps discoveries; Cancel doesn't mutate; Settings Defaults restores preferences only.
- [ ] Zones: installed pack count/metadata, zone and subzone counts, narration gates, discovery/unheard mode, combat hold, first greeting and map-panel visibility.
- [ ] Zones: Defaults confirmation resets only preferences/font/map-panel state; keeps visited/heard/greeted history and Enable Zones. Cancel doesn't mutate; verify after reload.
- [ ] Add/remove packs and reload; verify list counts, versions, scroll height and expansion state without UI errors/performance spikes.

**P2 ΓÇö visuals/less common cases**
- [ ] Inspect the changed `SoftSpoken/icon.tga` (brand/play/minimap artwork) and narrator/module icons.
- [ ] Inspect unfinished legacy top plaque, silver tab U-frame, solid background, outer metal corners, close button and scrollbar: no overlapping art, gaps or ghost backgrounds.
- [ ] Confirm full language/profile values remain unchanged after interacting with disabled UI, and restore/reopen neighboring inputs correctly after Voice Only toggles.
- [ ] Confirm older SavedVariables with unusual/missing option keys don't crash; test with clean SavedVariables separately.

### Gate for calling this stable
A successful GitHub merge is **not** a completed QA pass. Keep all boxes unchecked until verified in Turtle WoW 1.12. Any P0 failure should be fixed before distribution to other players; keep unfinished settings/legacy-manager work tracked separately.

---

## Reviews

_No reviews._
## Inline review comments

_No inline comments._
