# PR #38: Make Places narrator headings consistently name the parent location

**Original:** https://github.com/z1xel/SoftSpoken/pull/38  
**State:** Merged  
**Author:** z1xel  
**Created:** 2026-10-29T05:55:58Z  
**Updated:** 2026-10-29T06:19:53Z  
**Closed:** 2026-10-29T06:06:11Z  
**Merged:** 2026-10-29T06:06:11Z  
**Head:** fix/places-audio-parent-headings (c595a6b748e04b50b6ad1275a941d296e1c1b3cd)  
**Base:** main (40433174a4c8a1f28912d4ab978459e02691c3fc)

## Original description

## Problem

The Places narrator's white heading was always `zoneName`, so the header duplicated the yellow label for zone, city and continent recordings. It only happened to be correct for subzones.

## Change

- Use `SSZLoreWindow.PlaceAbove(mapID)` and `SSZLoreWindow.ZoneName(parentID)`, the same **existing** static hierarchy and labels that the Places Compendium uses on the 1.12 client.
- **Subzones:** white = containing zone, yellow = subzone.
- **Cities:** white = containing zone, yellow = city.
- **Zones:** white = continent, yellow = zone.
- **Continents:** white = Azeroth, yellow = continent.
- **Azeroth:** white = Azeroth, yellow = Azeroth, deliberately, until we choose a better world-root title.
- A missing/failed Compendium UI helper gracefully falls back to the old zone name rather than interrupting any audio. No changes to clips, pack priority, queue, portrait, player layout or Compendium.

## Regression coverage

`tests/places_zone_portrait_emblems.lua` now uses the real Compendium map tables and real `SSZLoreWindow` parent resolver and tests Durotar/Valley of Trials, Darnassus/Teldrassil/Kalimdor/Azeroth, original yellow labels, and fail-safe behavior when the UI helper is missing or raises.

## In-game verification

Replay `Craftsmen's Terrace`, `Darnassus`, `Teldrassil`, `Kalimdor` and `Azeroth` from Places; compare white headings with the screenshot examples. The text and portraits should be otherwise unchanged.

## Archived records

- [PR metadata](pull_request.json)
- [General conversation comments](conversation_comments.json) (0)
- [Reviews](reviews.json) (0)
- [Inline review comments](inline_review_comments.json) (0)
- [Changed files and patch fragments](changed_files.json) (2)
- [Commit references](commits.json) (2)
- [Timeline events](timeline.json) (5)
- [Readable conversations and reviews](DISCUSSION.md)

## Changed files

| File | Status | + | - |
| --- | --- | ---: | ---: |
| SoftSpoken_Places/Audio.lua | modified | 27 | 1 |
| tests/places_zone_portrait_emblems.lua | modified | 56 | 1 |

