# PR #35: Support both ClassicAPI creature portrait model implementations

**Original:** https://github.com/z1xel/SoftSpoken/pull/35  
**State:** Merged  
**Author:** z1xel  
**Created:** 2026-10-29T04:13:29Z  
**Updated:** 2026-10-29T06:09:53Z  
**Closed:** 2026-10-29T05:47:37Z  
**Merged:** 2026-10-29T05:47:37Z  
**Head:** feature/classicapi-model-compat (c82298ba72dea4e997f5576b2cf5c20ed1b962c0)  
**Base:** main (a4ac154bbfaa5ff99cce4ae2e8ac02692dbcd726)

## Original description

## Places narrator portraits: zone emblems (additional change)

- Restores upstream Spoken's **zone emblem** in the compact Places narration portrait, instead of repurposing a scenic Compendium illustration.
- Zone and subzone narration share the zone's icon; cities, Moonglade and battlegrounds have their own upstream emblems; an unmapped map can inherit its parent via `Spoken:ZoneIcon`, or fall back to the bundled book texture if no icon can be found.
- Expands `SoftSpoken/API.lua`'s `ZONE_ICONS` map to upstream's complete **57** badges. The repository already contained 47 identical upstream assets; added the 10 missing city/battleground/Moonglade `.tga` files with SHA verification against upstream commit `59a3445d`.
- Keeps Places' scenic illustrations in the Compendium (`SSZPicturesUI`), without cropping a landscape into the round narrator face. No audio pack, queue, map highlights or Compendium layout changes.
- Adds `SoftSpoken/Textures/Zones/SOURCE.md` with attribution, plus `tests/places_zone_portrait_emblems.lua` and CI validation of the 57 packaged icon files and narration fallbacks.
- The temporary binary import workflow was **removed** after it committed the verified assets; no scheduled import or new runtime network dependency remains.

**In-game verification:** play any zone and subzone (e.g., Durotar and Valley of Trials), then a capital (e.g., Darnassus) and a zone with no icon. Both zone and subzone should display the same emblem; a capital should use its own; scenic pictures should remain visible in the Places Compendium. Check the circular crop and the Azeroth globe, which uses whole UVs.

## Why

ClassicAPI's October 19, 2026 model update moved `SetDisplayInfo` and `SetCreature` from `Model` to `PlayerModel`, with native camera, light, skins and `SetPortraitZoom`. SoftSpoken's optional NPC renderer still assumed a plain `Model` with manual lighting, which cannot display portraits using the updated API.

## Changes

- Keep the existing `kind="creature"` presentation contract, quest speaker lookup, legacy `kind="model"` fallback and audio queue untouched.
- Detect actual frame methods at runtime rather than DLL version strings. Prefer `PlayerModel:SetDisplayInfo` when supported; otherwise select the original `Model:SetDisplayInfo`.
- Use native `PlayerModel:SetPortraitZoom(1)` when available; retain manual `Model:SetCamera(0)` and lighting for older ClassicAPI.
- Assign IDs before making an NPC visible; hide and clear the prior speaker on changes, and refresh native display records on reuse. Never depend on `GetModel()` immediately returning a path on the new asynchronous `PlayerModel`.
- Keep the original Vanilla model path available when no ClassicAPI creature ID can be resolved, and preserve the original non-creature item/object artwork.
- **New:** use the exact faction crest already shown on the Stories Compendium as the final fallback for NPC portraits instead of the book icon; once a ClassicAPI creature ID resolves, a failed enhanced portrait falls directly to the crest rather than an old NPC model.
- Retain *all* creature cache completions within one update tick instead of overwriting a single `refreshID`; only refresh the current speaking NPC without touching narration state.
- Add `tests/classicapi_portrait_compatibility.lua` and execute it in GitHub Actions alongside existing audio checks.

### Shared player-faction crest (additional pre-merge polish)

- Added `SoftSpoken_Stories/FactionCrest.lua`, loaded before `Player.lua` and `Compendium/QuestPage_112.lua`. Both callers invoke `SSCQuestFactionCrest.Resolve()`, so Alliance/Horde artwork and faction/race detection cannot drift apart.
- Uses the existing 256├ù256 transparent `Assets/AllianceCrest.tga` and `Assets/HordeCrest.tga`, with **full (0,1,0,1) texture coordinates** (no icon cropping).
- Keeps the exact Compendium priority of `UnitFactionGroup("player")` and legacy `UnitRace("player")`, including Turtle races. Unknown faction retains the Compendium's existing book icon.
- Quest objects/items still show their actual item icon or note, not a faction crest.
- Adds `tests/stories_faction_crest.lua` to CI to verify faction selection, nonlocalized race fallback, replay/live-NPC/legacy fallback, and shared Compendium wiring.

### Automated verification

Run `lua5.1 tests/classicapi_portrait_compatibility.lua` and `lua5.1 tests/stories_faction_crest.lua` to exercise the real portrait module against mocked **older API**, **updated API**, and **no API** frames, including stale model replacement, async cache event coalescing, optional-disable fallback and deferred modern appearance loading.

This is a simulation, **not proof of in-game rendering on a particular ClassicAPI release**.

### Turtle WoW / real-ClassicAPI checks before merging

1. With older ClassicAPI: start live NPC dialogue and quest-log replay, verify NPC is framed and lit using `Model`.
2. With newer ClassicAPI (v1.18+): repeat with `PlayerModel`; verify native head/shoulders camera, textures and equipment, no black portrait.
3. Repeat with an uncached NPC/quest giver and observe texture fallback followed by a model when the cache data arrives.
4. Switch rapidly among speaking NPCs; verify no stale faces or old models remain.
5. Hide/show player, pause/resume, play a quest object/item and switch back to a creature, including the same creature after a different one.
6. Run `/run SoftSpokenClassicPortraits=false` then repeat a dialogue: if no live model is available, it must show your faction crest. Restore with `/run SoftSpokenClassicPortraits=nil` and reload to reenable.
7. Test **both an Alliance and a Horde character**: the audio-player fallback must show the same crest as Stories' Compendium, uncropped, in quest-log replay and when a model can't load; verify objects/items keep their own icons.
8. Check there is no change to playback, queue, quest lookup or Compendium behavior.

**Scope:** Optional NPC portrait rendering only; no 3D book/game-object rendering and no changes to public data packs. Keep draft until both ClassicAPI versions have been tested in game.

## Archived records

- [PR metadata](pull_request.json)
- [General conversation comments](conversation_comments.json) (0)
- [Reviews](reviews.json) (0)
- [Inline review comments](inline_review_comments.json) (0)
- [Changed files and patch fragments](changed_files.json) (22)
- [Commit references](commits.json) (22)
- [Timeline events](timeline.json) (28)
- [Readable conversations and reviews](DISCUSSION.md)

## Changed files

| File | Status | + | - |
| --- | --- | ---: | ---: |
| .github/workflows/audio-pack-regression.yml | modified | 30 | 0 |
| SoftSpoken/API.lua | modified | 6 | 4 |
| SoftSpoken/Textures/Zones/AlteracValley.tga | added | 0 | 0 |
| SoftSpoken/Textures/Zones/ArathiBasin.tga | added | 0 | 0 |
| SoftSpoken/Textures/Zones/Darnassus.tga | added | 0 | 0 |
| SoftSpoken/Textures/Zones/Ironforge.tga | added | 0 | 0 |
| SoftSpoken/Textures/Zones/Moonglade.tga | added | 0 | 0 |
| SoftSpoken/Textures/Zones/Orgrimmar.tga | added | 0 | 0 |
| SoftSpoken/Textures/Zones/SOURCE.md | added | 19 | 0 |
| SoftSpoken/Textures/Zones/Stormwind.tga | added | 0 | 0 |
| SoftSpoken/Textures/Zones/ThunderBluff.tga | added | 0 | 0 |
| SoftSpoken/Textures/Zones/Undercity.tga | added | 0 | 0 |
| SoftSpoken/Textures/Zones/WarsongGulch.tga | added | 0 | 0 |
| SoftSpoken/UI/Portrait.lua | modified | 92 | 42 |
| SoftSpoken_Places/Audio.lua | modified | 14 | 26 |
| SoftSpoken_Stories/Compendium/QuestPage_112.lua | modified | 9 | 33 |
| SoftSpoken_Stories/FactionCrest.lua | added | 43 | 0 |
| SoftSpoken_Stories/Player.lua | modified | 12 | 11 |
| SoftSpoken_Stories/addon.xml | modified | 1 | 0 |
| tests/classicapi_portrait_compatibility.lua | added | 193 | 0 |
| tests/places_zone_portrait_emblems.lua | added | 114 | 0 |
| tests/stories_faction_crest.lua | added | 124 | 0 |

