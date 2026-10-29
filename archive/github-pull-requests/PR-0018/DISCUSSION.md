# PR #18: conversations and reviews

[Original PR](https://github.com/z1xel/SoftSpoken/pull/18) | [Archive summary](README.md)

## General discussion

### z1xel on 2026-10-26T18:48:17Z

[Original comment](https://github.com/z1xel/SoftSpoken/pull/18#issuecomment-6424543507)

## Follow-up implementation pass

Initial parchment appearance was reported **looking okay in-game**; this is a limited visual check, not a full functional QA pass.

Additional changes pushed on this branch:
- Make the **map-only** subtitle/breadcrumb row expand when its text wraps, rather than overlapping the decorative divider on narrow screens. Single-line subtitles keep the original 16-unit height, and the normal Compendium remains untouched.
- Remove a redundant standalone `OnSizeChanged` handler: `SSCTextView` already reflows when its page frame is resized, so there is now only one resize path.
- Static review confirmed unchanged 338-unit width, original World Map anchoring, zone/city/subzone selection, map audio binding, Compendium shortcut, and the standard Compendium split-page coordinates.

### Remaining focused in-game verification
- [ ] Long titles/breadcrumbs (e.g. Eastern Kingdoms, zone names with long subtitles) wrap above the divider, not into the content.
- [ ] Scroll via wheel/arrows/thumb to the end of long stories, then switch to a short story and back; no overshoot or stale offset.
- [ ] Open in Compendium button correctly targets zone, city and subzone where available.
- [ ] Collapse/reopen map panel, change map modes/UI scale, and verify the page and scrollbar resize correctly.
- [ ] Compare regular Places/Books/Quests Compendium page layout with previous `main`: no regression.

PR remains unmerged pending this functional testing.

---

### z1xel on 2026-10-26T20:34:29Z

[Original comment](https://github.com/z1xel/SoftSpoken/pull/18#issuecomment-6426692557)

## World Map outer frame follow-up

The latest screenshots confirmed that the inner parchment/scrollbar matched, but the attachment still had the old `SSZUI.AddBorder` root. The **Compendium shell** is owned separately by `SSCPrototypeFrameSkin.Apply`, which supplies the Blizzard metal title-bar frame, close-button surround, and tiled `UI-Background-Rock-Classic`.

Pushed two changes:
- `PrototypeFrameSkin.Apply(frame, title, options)` now supports a **compact** mode using the same metal/rock texture code, with a mirrored top-left corner but no large book portrait or minimizer. The normal Compendium call path and its original title dimensions remain unchanged.
- `MapPanel.NewPanel` calls the shared shell with `title="Zone Lore"`, `compact=true`, and a Close callback that still collapses the map panel rather than hiding it permanently. Removed the redundant generic outer border, title and close button.

**In-game verification needed**
- [ ] Rock texture fills the map attachment around its parchment, with no purple/transparent gaps.
- [ ] Top-left and top-right metal corners connect to the title bar; bottom edges/sides are intact.
- [ ] The single close button sits inside the top-right frame corner and collapses to the existing map toggle; reopening still works.
- [ ] Compendium's original portrait, title bar, corner rendering, close and minimize controls remain unchanged.
- [ ] Check overlapping-map mode and exterior docked mode, including at different UI scales.

Only the shell rendering and map's wrapper were changed in this pass; map lore/docking/voice logic is unchanged. **Not yet visually tested in-game.**

---

### z1xel on 2026-10-26T20:51:25Z

[Original comment](https://github.com/z1xel/SoftSpoken/pull/18#issuecomment-6427031244)

## Compact-frame corner fix

Commit `88a7614` updates `PrototypeFrameSkin.lua` for the **map attachment's compact mode only**:
- Replace the reversed top-right image with Blizzard's real `UI-Frame-TopLeftCorner` 32├ù32 atlas region (`0.6328125, 0.8828125, 0.28125, 0.53125`).
- Move the compact title strip to meet the left corner at x=26 (the native corner's right edge) and finish at the existing right corner.
- Raise the original 14├ù14 bottom-left and 11├ù11 bottom-right corners above the border strips, and move both bottom caps up to the map panel's lower edge to avoid clipping.
- Recalculate compact lower rail and vertical lengths to meet the corner sprites.
- **Leave the normal Compendium geometry unchanged.**

Static checks passed against the current branch tip. **In-game visual checks remaining:** confirm the top-left seam and both bottom corners at the World Map's bottom edge; check the close button, compact frame when overlaid/docked, and ensure Compendium's full-size frame is unchanged.

---

### z1xel on 2026-10-26T21:06:10Z

[Original comment](https://github.com/z1xel/SoftSpoken/pull/18#issuecomment-6427319207)

## Compact bottom-corner follow-up ΓÇö commit `fae6c62`

The previous bottom caps were real Blizzard `UI-Frame` crops (14├ù14 left, 11├ù11 right), but moving them from `BORDER` to `OVERLAY` on the **parent frame** did not place them above the parchment/scrollbar **child frames**. The child frames were still covering the top of the corner art, leaving a flat/broken lower border.

This pass fixes the draw hierarchy rather than stretching/mirroring corner textures:
- Adds one compact-only mouse-pass-through foreground frame, `sszCompactTrim`, above the page.
- Draws both native lower corner sprites, the matching horizontal rail, and both connecting vertical rails on that same frame.
- Reuses pooled texture segments during size changes.
- Leaves the approved top corners/title bar, full Compendium frame, parchment, map width, docking, and gameplay logic unchanged.

### In-game check
- [ ] Both bottom caps are completely visible and connected to the horizontal rail.
- [ ] Both vertical borders join their bottom caps with no bright seam or extra overhang.
- [ ] Bottom frame is visible at full-map and windowed scale without obscuring the scrollbar down arrow, blocking clicks, or covering parchment text.
- [ ] Compare the top/title area to the prior screenshot: identical.
- [ ] Confirm the full Compendium's frame remains unchanged.

**Status:** pushed and statically reviewed; not yet tested in-game.

---

### z1xel on 2026-10-26T22:12:08Z

[Original comment](https://github.com/z1xel/SoftSpoken/pull/18#issuecomment-6428553156)

## Map attachment follow-up ΓÇö misplaced bottom rail (2026-10-26)

The latest screenshot reveals a more serious regression: a **horizontal metal bottom rail appears partway up the parchment**, while the attachment continues below it. This matches the compact trim's old dependence on `h-9` during `Reflow`; changes in map-anchor height could leave a previously measured border position in the middle.

Two focused commits on this PR:
- `fade03bc`: anchor the **compact-only bottom rail directly to the frame's BOTTOMLEFT**, not to `TOPLEFT` with a cached `h-9` offset. The native trim sprite and 9px thickness remain unchanged.
- `9d98e79`: add `Skin.RefreshCompact(frame)`, which compares the measured width/height with the last compact reflow. The map's **existing 0.25s watch** invokes it, but full texture reflow only occurs if geometry actually changes. This covers map clients that do not reliably fire `OnSizeChanged` after anchor-driven resizes.

The approved **top/title chrome is untouched**. The regular Compendium's bottom layout and frame code remain in the original noncompact path. No changed SavedVariables, map selections or audio logic.

**Required in-game check**
- [ ] The stray horizontal line across the *middle* of the parchment is gone, including after toggling map sizes and /reload.
- [ ] Bottom trim follows the real panel bottom at all map heights.
- [ ] Left/right verticals meet the bottom ornamentation cleanly.
- [ ] Scrollbar arrows, mouse wheel, and text remain interactive.
- [ ] Full Azeroth's Compendium frame and the map's title bar remain unchanged.

Static checks passed; exact in-game outcome remains unverified.

---

### z1xel on 2026-10-26T22:47:52Z

[Original comment](https://github.com/z1xel/SoftSpoken/pull/18#issuecomment-6429152319)

## Compact side borders: layering vs. geometry (commit `c344d1b`)

Examined the current screenshot and the actual shipped `!UI-Frame.tga` (64├ù256 with nontransparent vertical rail artwork through the tile). The side decorations on **both** sides stop abruptly at roughly two 256-unit repeats; the bottom rail and corners remain visible. Since the compact foreground trim already draws both verticals and the bottom on the **same foreground frame**, changing layer again would not account for a missing final tile on both sides.

The shared `PrototypeScrollSkin.lua` uses `GetTop()-GetBottom()` instead of trusting `GetHeight()` on anchored World Map UI. The compact outer shell was still using `GetHeight()` for tile counts and resize checks. This commit:
- measures the *compact-only* panel from its actual left/right and top/bottom edges; leaves the full Compendium measurements unchanged;
- makes the geometry refresh compare that same edge-measured size;
- pins the **final left/right side tile directly to the bottom corner** (14px left / 11px right), with all other tiles still starting from the top, avoiding an unfilled terminal strip;
- preserves the top title and corners, bottom rail, 256-unit source texture, foreground layering, map docking, page and scrollbar.

For a 765-unit-tall example both sides need 3 tiles; calculated last start exactly equals the previous segment end, with zero gap. This is a source/geometry check only.

**In-game validation needed:** inspect the last 300 pixels of both borders at the previous map scale; verify they run continuously down to their native corner caps, do not introduce a repeat seam and survive toggling map size / UI scale. Verify Compendium frame unchanged.

---

### z1xel on 2026-10-26T23:00:59Z

[Original comment](https://github.com/z1xel/SoftSpoken/pull/18#issuecomment-6429354811)

## Compact Zone Lore close-button hover / hitbox (commits `dae4e38`, `2adfeed`)

User confirmed the vertical frame side fix is now correct, but the native red X has an **offset hover highlight and click target** in the World Map attachment.

Changed **only the compact variant**:
- Explicitly anchor the native button's normal, pushed, and highlight textures to the **same 32├ù32 button frame**.
- Confine its clickable rectangle to the 24├ù24 central graphic (4-unit insets), without shifting the visible X or the shared title frame.
- Promote the close button to `panel + 15` *after* constructing the map parchment/scrollbar, above the `panel + 10` compact border overlay. This should prevent children/trim from interfering with the button.
- Keep its existing `M.SetMapPanelCollapsed(true)` callback and full Compendium close button behavior unchanged.

**In-game check:** hover directly over the red X (highlight should sit on the same sprite), click its visible center to collapse, click surrounding silver metal (should do nothing), reopen via the map toggle, repeat after /reload and under a different UI scale. Check full Compendium close button unchanged.

Static checks passed, **not yet verified in-game**.

---

## Reviews

_No reviews._
## Inline review comments

_No inline comments._
