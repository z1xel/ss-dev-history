# PR #11: Refactor settings into shared Vanilla module pages

**Original:** https://github.com/z1xel/SoftSpoken/pull/11  
**State:** Merged  
**Author:** z1xel  
**Created:** 2026-10-23T18:53:25Z  
**Updated:** 2026-10-26T06:10:06Z  
**Closed:** 2026-10-25T19:03:04Z  
**Merged:** 2026-10-25T19:03:04Z  
**Head:** feat/unified-settings-112 (eb17963400f95145ed19ee1e3b9d5f10ba5f7f04)  
**Base:** main (38b8024008f62090748f9e29a5fdd994ea16d525)

## Original description

### Zones Defaults button (October 25)
- Added a matching **Defaults** button to Zones, using the same `Layout:Defaults` header placement and confirmation workflow as Books.
- The reset restores the supported Zones narration and map-panel preferences, plus lore font size, without touching this character's `heard`, `visited`, or `greeted` history.
- Module Enable Zones is a shared-player toggle and is intentionally unchanged, matching Books' preference-only reset behavior.
- Restored map-panel visibility and narration gates are refreshed immediately.
- [ ] Verify the button sits in the Zones header, shows the confirmation, restores defaults on Reset, and leaves settings unchanged on Cancel.
- [ ] Verify previously discovered/heard zones and the Enable Zones switch remain unchanged after reset and reload.

### Compendium hotkey (October 25)
- Added **Open Azeroth's Compendium** to `SoftSpoken/Bindings.xml` and the Home **Keys** list. The binding uses `/spoken compendium` and the same Compendium action as the minimap button.
- On first initialization, assign **J** if it is unbound and no other key is already assigned to this Compendium action. Do not replace an existing WoW/addon J binding. Store a one-time account-wide seed marker so manually clearing/rebinding the shortcut will not restore J every reload.
- The user can change or clear this shortcut through the existing key picker, which prompts before replacing another action.
- No changes to window artwork, module settings, profiles, or narration behavior.

#### Validate in WoW 1.12
- [ ] On an unused J key, Keys shows **Open Azeroth's Compendium ΓÇö J** and J opens/closes Compendium.
- [ ] Changing it to another key works, persists after `/reload`, and clearing it doesn't automatically reinstate J.
- [ ] If J belongs to an existing keybind, it remains untouched; manually setting J uses the normal replacement prompt.
- [ ] The existing three audio keybinding controls and minimap Compendium action remain functional.

### Minimap click assignments and Vanilla menu crash (October 25)
- Fixed the reported `SoftSpoken/UI/MinimapButton.lua:155: attempt to index a nil value`: 1.12's UIDropDownMenu callbacks must capture a stable per-menu-entry variable instead of the generic `for` loop variable.
- Fixed the same Lua 5.0 closure bug in `SpokenLayout:Dropdown` affecting click-mapping choices: selecting an item could write `nil` instead of that command. Shared layout version is now 68.
- Direct minimap clicks no longer assume the modern `SOUNDKIT` constant exists; a legacy-safe click sound is used.
- Minimap mouse-button assignments now offer only **Open Settings**, **Open Compendium**, **Open Menu**. Retired Play / Pause and Stop Reading choices are no longer in the settings dropdown. Existing Stop or Replay / Stop All entries in the contextual minimap menu are untouched.
- Existing profiles with `PlayPause`, `Stop`, or a blank mouse assignment are migrated on read to Left=Settings, Middle=Compendium, Right=Menu. The same command reader is used by the settings dropdown and click dispatcher.
- No change to narrator playback, report settings, other module UI, or saved preferences outside these intentionally retired minimap assignments.

#### Verify in Vanilla 1.12
- [ ] Change Left, Middle and Right Click mappings one at a time; the selected label should immediately update and persist after `/reload`.
- [ ] Try mapping every button to each of the 3 remaining commands. Each should perform the chosen action.
- [ ] Open minimap's context menu and select Settings, Open Compendium, Quests Settings and Gossip Settings. No `MinimapButton.lua:155` error.
- [ ] Confirm clicking the minimap button to open settings/Compendium no longer produces a separate `SOUNDKIT` error.
- [ ] Existing profiles with retired mappings should display and use supported replacements (Left Settings, Middle Compendium, Right Menu).

### Unified setting label typography (October 25)
- The earlier adaptive-label fix chose `GameFontNormalSmall` per row, causing visibly inconsistent text sizes across Sound, Minimap, and other sections. This is corrected: every ordinary setting label now uses `GameFontNormal`, without conditional font shrinking.
- Labels still measure their full text and move controls right when there is room. On legacy sliders only, the track can shorten to a minimum usable 170-unit frame width to give a longer label room without changing font size.
- Section headings and secondary information retain their intentionally different Blizzard font hierarchy.
- No saved-variable, narration, input/disable, window geometry, or profile changes. Only `SoftSpoken/UI/Layout.lua` changed.

#### Confirm in-game
- [ ] All gold setting labels use the same font size (e.g. Silence NPC Voices, Pause Between Lines, Minimap Button controls, and key bindings).
- [ ] Ordinary labels remain complete; truly long names may still truncate.
- [ ] Sliders remain usable with their value labels visible and within the frame.
- [ ] Disabled Language/Profiles/Report settings still show correctly.

### Settings labels: adaptive width (October 25)
- Replaced the original fixed caption cutoff (`85` units before the midpoint), which prematurely displayed ellipses on ordinary labels such as **Smooth Caption Changes**, **Lock Position**, and **Hide Report Button**.
- The shared Vanilla layout now measures the full text width and moves that row's control to the right **only when there is available space**, respecting the control's own width and the page edge. Extremely long strings can still truncate.
- If a label cannot fit at the standard font size, the code tries Blizzard's small settings font before clipping.
- Applies to checkbox, slider, dropdown, keybinding, badge and voice-pack rows in **all** settings pages; no per-page patches or changes to window size.
- No change to settings behavior, persistence, narrator functionality, group backdrop, shields or profile handling.

#### Verify in-game
- [ ] Labels like **Smooth Caption Changes** and **Lock Position** display in full; confirm longer Quests, Books and Zones labels too.
- [ ] Sliders and dropdowns remain inside the parchment/frame and do not overlap the scrollbar.
- [ ] Controls and tooltips are still interactive where enabled; the previously disabled Report/Language/Profiles entries remain inert.
- [ ] Check at your normal UI scale and with a shorter game window if applicable.

### Disable Hide Report Button UI (October 25)
- The registered optional action with ID `report` now has its **Hide Report Button** setting dimmed and fully blocked (no row highlight, tooltip or checkbox click), matching the unavailable Language and Profiles sections.
- Displays a red `Report button visibility is currently unavailable` explanation below the setting.
- The original `HiddenActions.report` stored preference and Report action/playback logic are preserved; a separate guard prevents writes from an old UI callback.
- Added the invisible shield to the setting's layout regions so it hides automatically when Voice Only mode hides that optional-action row; neighboring controls remain usable.
- No changes to Quests, profiles, language preferences or the actual Report button behavior.

#### Validate in 1.12
- [ ] Hover and click the **Hide Report Button** row/checkbox: no highlight, tooltip or toggle.
- [ ] Verify other checkboxes (especially Hide Portrait) still work and display tooltips.
- [ ] Switch to Voice Only and back: the report-row shield must not block controls occupying the same screen space.
- [ ] After `/reload`, the previous Report button preference remains unchanged.

### Narrator settings group layering correction (October 25)
- Fixed the dark group panel rendering over its own section headings and setting labels in Vanilla 1.12.
- Moved the grey translucent fill onto the **scroll page's BACKGROUND texture layer**, behind its ARTWORK labels and controls. The group keeps the same color, opacity, existing tooltip border, dimensions, spacing and hide/show behavior.
- The child group frame now draws only the border, preventing its background from obscuring parent-frame text regardless of frame-level quirks.
- Bumped the shared layout version to 64. Only `SoftSpoken/UI/Layout.lua` changed.

#### Verify in game
- [ ] Narrator Style Settings ΓåÆ Words in the Window and Window headings should appear *in front of* the dark background.
- [ ] Show Words, Text Size, Smooth Caption Changes and Window Size remain as before, with no displaced controls.
- [ ] The boxed dark panel still appears, disappears and scrolls with its content correctly; other sections are unaffected.

### Disabled row hitbox correction (October 25)
- Widened the invisible unavailable-control shield 12 units horizontally and 7 vertically beyond the existing hit row. This covers the standalone 1.12 dropdown arrow and its decorative bounds.
- On Vanilla, the `UIDropDownMenuTemplate` arrow is a separately named `<DropdownName>Button`; this patch also calls `Disable()`, `EnableMouse(false)` and turns off disabled-button motion scripts on that child.
- The shield uses `TOOLTIP` strata above the dropdown and raises its frame level during settings refresh. No new texture, setting, SavedVariables change, or change to the Profile/Language backend.
- Only `SoftSpoken/UI/Layout.lua` changed in this patch.

#### Confirm in-game
- [ ] Hover the arrow of each Profile dropdown and both Language dropdowns: no blue hover state, tooltip, highlight, or openable menu.
- [ ] Hover the label area and empty space across those rows: no hover band or tooltip.
- [ ] Click New Profile: no popup.
- [ ] Check immediately adjacent settings still work, including mouse-wheel scrolling.

### Hard-disable unsupported sections (October 25)
- Language and Profiles are now intentionally read-only. Both sections are greyed out and display a red note explaining their current unavailability.
- New shared `SpokenLayout:BlockInteraction` overlays an invisible mouse shield across a disabled row, including the embedded Vanilla dropdown arrow; it suppresses hover bands, tooltips, clicks and menu opening. The usual `Requires` visual dimming remains.
- Language write callbacks, profile switch/copy/delete handlers, New Profile button and popup acceptance are separately guarded against stale/previously open interactions.
- All existing language accessors, AceDB profile operations, saved preferences and account data remain untouched. No reset or migration.
- Profile and language sections can be restored later by changing their availability flags; the shield is applied only while unavailable.
- The only edited files are `SoftSpoken/UI/Layout.lua`, `SoftSpoken/UI/Options.lua`, and `SoftSpoken/Strings.lua`.

#### In-game checks
- [ ] Hover anywhere over Voice Language and If a Line Is Missing, including the gold dropdown arrow: no highlight or tooltip, no menu on click.
- [ ] Hover Profile in Use, Copy From, Delete Profile, and New Profile: no highlight, tooltip, menu or popup on click.
- [ ] Both headings are grey; the red explanation below each section is readable.
- [ ] Adjacent Sound and Minimap controls remain fully interactive; scrolling continues to work.
- [ ] Language and profile selections remain unchanged after `/reload`.

### Upstream-inspired module clarity pass (October 24)
- Books and Zones now render a **read-only list of compatible installed voice packs** in the existing Vanilla settings layout. Books uses installed addon Version metadata when available; Zones shows validated names without inventing content versions.
- The inventory computes its own row height from the current pack registry, so optional packs can appear after the window was built and the scrollbar can reflow correctly.
- Books displays the existing module status and clarifies that installed packs do not guarantee every book is recorded in the selected Home voice language.
- Existing Quests test-line and diagnostics actions, plus Books diagnostics, are grouped under **Fix a Problem**. No new playback or diagnostic actions were added. Zones has no equivalent verified diagnostic action, so none was invented.
- Search gains a small native Blizzard-style clear control; original search ranking, results and navigation are preserved.
- Shared `SpokenLayout` bumped to version 61. The window frame, coordinates, Quests AceGUI fallback, SavedVariables, profiles, audio playback and source discovery were not edited in this pass.

#### In-game checks
- [ ] Books: installed pack names and optional addon versions display correctly; no-pack message appears when appropriate.
- [ ] Zones: installed pack names match the Home count; no-pack message appears when appropriate.
- [ ] Check that adding/removing optional packs and reloading updates the lists without content clipping or scrollbar overshoot.
- [ ] Quests: Test Voice Line and Diagnostics still execute from Fix a Problem.
- [ ] Books: Show Diagnostics still works; reading history and reset are unaffected.
- [ ] Search: enter a query, clear it with the small X, and verify search navigation still works on all four pages.
- [ ] Check that disabled dependent narration controls still explain why they cannot be used.


### Follow-up compatibility and layout pass (October 24)
- Verified all deferred narrator style tile callbacks use a per-tile value after the reported 1.12 `item.value` crash.
- Hardened the older module-card renderer and optional third-party module switches against the same Lua 5.0 generic-for closure issue.
- Added a Vanilla hover-boundary fallback for framed module and narrator cards where `Frame:IsMouseOver()` is unavailable; hover between the row and its child button/checkbox should remain stable.
- Fixed the status text so an installed module without a pack-count callback is no longer mislabelled `Not installed`.
- Removed a doubled nine-unit visual gap from the compact module rows.
- Kept the Quests module/fallback, audio behavior, profile code, and outer settings window unchanged.

#### Verify after pulling this pass
- [ ] Open `/spoken options` without a Lua error; switch narrator styles and click their illustrated tiles.
- [ ] Move the mouse across the module row, its checkbox and Settings button; no flashing or sticky highlight.
- [ ] Toggle Quests/Gossip/Books/Zones by both the row and checkbox, and open each module page.
- [ ] Check installed voice pack counts, especially no-pack and missing-module statuses.
- [ ] Confirm Home scrolling and text spacing at normal 1.12 UI scale.
- [ ] Report any additional Lua error line verbatim or supply an in-game Home screenshot before more geometry changes.

### Runtime fix ΓÇö narrator tiles (October 24)
- Fixed the Vanilla Lua 5.0 `for`-variable capture in `SpokenLayout:Tiles`: deferred style refreshes and click handlers now reference a stable per-tile value instead of the expired `item` loop variable.
- Reported client exception: `SoftSpoken/UI/Layout.lua:1982: attempt to index a nil value`.
- Only `SoftSpoken/UI/Layout.lua` changed in this fix; existing module and Quests code remains intact.
- [ ] Confirm `/spoken options` opens without a Lua error, then switch Large Window / Voice Only and verify both selections remain correct after reload.

### Vanilla-native upstream-inspired redesign (October 24)
- **Home module list:** replaces the plain Vanilla switches with four compact framed rows, each showing the module icon, description, voice-pack count, Settings button, and enable checkbox. Uses the existing Blizzard tooltip border, font and button art; no Retail template dependencies.
- **Narrator selection:** replaces the Large Window / Voice Only dropdown on Vanilla with two illustrated tiles, reusing the existing `SpokenLayout:Tiles` renderer and its sketch artwork. The selected style receives the established gold selection treatment and the Preview action remains available.
- Existing Sources toggles, player-style setting, registered module pages and search indexing stay the same. The modern-client cards remain on their original path.
- Bumped the shared `SpokenLayout` version for the new renderer and sized Settings buttons for localized labels.
- Did not change the 550-unit window width, top tab navigation, profile code or Quests AceGUI fallback.

#### Redesign checks before additional visual changes
- [ ] On Home, check the four module rows for icon/text/button/checkbox overlap at native 1.12 UI scale.
- [ ] Verify that module enabled states and voice-pack counts reflect actual installations, including when disabled.
- [ ] Click both a module row and its checkbox to toggle that module; use Settings to open its correct page (Gossip shares Quests).
- [ ] Search for Quests, Books, Zones and Large Window; verify navigation and gold highlighting.
- [ ] Toggle between Large Window and Voice Only, test Preview, close the window, and confirm style persists after `/reload`.
- [ ] Scroll Home to the bottom and confirm no clipping or scrollbar overshoot.
- [ ] Capture a Home screenshot to refine proportional spacing before any further changes to the outer Blizzard frame.


## Scope
Unified settings for Turtle WoW 1.12, adapting upstream Spoken's shared layout, page registration, and search to the Vanilla client. No new narration settings or SavedVariables.

## Implemented
- One Blizzard-style settings window with Home, Quests/Gossip, Books, and Zones pages.
- Quests shared page is working in-game after guarded registration during AceAddon initialization. Original AceGUI window remains as a fallback.
- Quests and Gossip retain independent enable switches and their existing audio configuration.
- Books shares the layout; read history, discovery access, diagnostics, and reset remain accessible even if narration is disabled.
- Zones exposes existing narration/map controls plus installed-pack count from the already-supported audio registries.
- Search indexes labels, sections and tooltips across pages. It does not refresh every module on each keystroke, and clears focus when the window hides.
- Existing minimap mouse click mappings are visible; Open Compendium now resolves the consolidated SSCCompendium entry.
- Misleading Vanilla narrator/language descriptions are corrected. Advanced unsupported upstream options remain hidden.
- Latest main has been merged into this branch (including native quest-log playback controls/assets) while preserving the shared settings UI.

### Final UI polish and safe cleanup (October 24)
- Fixed window title and Blizzard-texture divider stay visible above the scroll viewport; the active page uses a gold selection rule rather than a disabled, greyed-out button.
- Escape closes the shared window via the native `UISpecialFrames` mechanism; closing already clears search focus and preview.
- Shared window dimensions, tab offsets and scrollbar positioning now use named constants.
- Removed dead comments and redundant category-routing logic, and reused the existing localized message for searches with no results.
- Quests initialization, the original AceGUI fallback, SavedVariables, and profile management remain unchanged.

#### UI verification before merging
- [ ] Active Home/Quests/Books/Zones tab has a thin gold underline and is readable, with no grey disabled-looking selection.
- [ ] Heading, search field, tab row and divider fit inside the top frame without overlapping the close button.
- [ ] Scroll from top to bottom on each page. Check no content gets cut off at the bottom.
- [ ] Escape dismisses the settings window. Search focus/overlay and the narrator preview should disappear; reopen and test again.
- [ ] Search for Quests, Books and Zones controls. Confirm result highlights and the localized no-results text.
- [ ] Verify the separate /spq options fallback and normal quest playback still work.

## Test next: no profile work yet
- [x] Quests shared tab loads and can be opened separately (reported working).
- [ ] Open and close the settings window with X, minimap, and /spoken options. Ensure search focus disappears.
- [ ] Search for an item on each page, press Enter, and verify tab switch, scroll, gold highlight; try unknown queries.
- [ ] Verify the bottom control and scrollbar stop on each of the four tabs.
- [ ] Disable Books and verify narration controls grey out while reading history and diagnostics still work.
- [ ] Verify the Zones installed audio-pack count, narration dependencies and independent world-map panel toggle.
- [ ] Set a minimap mouse button to Open Compendium; verify it opens Azeroth's Compendium and persists on /reload.
- [ ] Confirm /spq options, /spq test, quest narration, and the native quest-log play/stop buttons still work after syncing main.
- [ ] Verify Book and Zones preferences persist after /reload; verify reset only affects the advertised scope.
- [ ] Review screenshots for native frame proportions, header and tab spacing.

## Intentionally last
Profiles: defer UI and behavior changes, including create/switch/copy/delete testing, until the window and module settings are stable. Do not remove or rewrite current AceDB profile code in this phase.

Draft until these in-game regressions are checked; there is no local WoW client available to run UI tests.

## Archived records

- [PR metadata](pull_request.json)
- [General conversation comments](conversation_comments.json) (1)
- [Reviews](reviews.json) (0)
- [Inline review comments](inline_review_comments.json) (0)
- [Changed files and patch fragments](changed_files.json) (20)
- [Commit references](commits.json) (124)
- [Timeline events](timeline.json) (129)
- [Readable conversations and reviews](DISCUSSION.md)

## Changed files

| File | Status | + | - |
| --- | --- | ---: | ---: |
| SoftSpoken/Bindings.xml | modified | 1 | 0 |
| SoftSpoken/Core.lua | modified | 28 | 4 |
| SoftSpoken/Strings.lua | modified | 16 | 9 |
| SoftSpoken/UI/Layout.lua | modified | 524 | 73 |
| SoftSpoken/UI/MinimapButton.lua | modified | 38 | 9 |
| SoftSpoken/UI/MinimapIconFit.lua | added | 31 | 0 |
| SoftSpoken/UI/Options.lua | modified | 433 | 133 |
| SoftSpoken/UI/Search.lua | added | 214 | 0 |
| SoftSpoken/addon.xml | modified | 2 | 1 |
| SoftSpoken/icon.tga | modified | 0 | 0 |
| SoftSpoken_Books/UI/Options.lua | modified | 135 | 205 |
| SoftSpoken_Quests/Options.lua | modified | 23 | 4 |
| SoftSpoken_Quests/Player.lua | modified | 4 | 6 |
| SoftSpoken_Quests/UI/SettingsPanel.lua | added | 238 | 0 |
| SoftSpoken_Quests/addon.xml | modified | 2 | 0 |
| SoftSpoken_Zones/Audio.lua | modified | 45 | 0 |
| SoftSpoken_Zones/Core.lua | modified | 3 | 0 |
| SoftSpoken_Zones/Settings.lua | modified | 20 | 0 |
| SoftSpoken_Zones/SoftSpoken_Zones.toc | modified | 1 | 0 |
| SoftSpoken_Zones/UI/Options.lua | added | 173 | 0 |

