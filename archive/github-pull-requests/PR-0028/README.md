# PR #28: Fix compatibility with current and legacy Spoken audio packs

**Original:** https://github.com/z1xel/SoftSpoken/pull/28  
**State:** Merged  
**Author:** z1xel  
**Created:** 2026-10-29T01:32:50Z  
**Updated:** 2026-10-29T01:38:19Z  
**Closed:** 2026-10-29T01:34:28Z  
**Merged:** 2026-10-29T01:34:28Z  
**Head:** beta/audio-pack-compatibility (eaba4918d337d6f19b385b8a05513a5f31b6a784)  
**Base:** main (9358856cf6ac04e0fabd8d1093217133e2c3cee5)

## Original description

## Summary
- Recognize both current (`SpokenPlacesAudioPacks`, `SpokenReadablesAudioPacks`) and legacy (`SpokenZonesAudioPacks`, `SpokenBooksAudioPacks`) pack registries.
- Include both generations in optional addon dependencies so sound packs load before the corresponding SoftSpoken module.
- Keep the Places Compendium voice detection and the Writings pack inventory aligned with playback.
- Add Lua regression coverage and a GitHub Actions workflow; preserve the existing Stories voice-pack priority tests.
- Document 1.12 compatibility considerations (ClassicAPI and out-of-date audio packs).

## Verification
- Synced with the latest `main` (including the fallback data packs and lore-scroll fixes) without overlap.
- Static file and registry-name checks completed; **new Lua regression workflow and clean in-game testing for Places/Writings remain to be verified**.
- User separately verified the untouched official `SpokenQuestsAudioAll` pack plays successfully in Stories.

No official third-party sound packs are modified or bundled.

## Archived records

- [PR metadata](pull_request.json)
- [General conversation comments](conversation_comments.json) (0)
- [Reviews](reviews.json) (0)
- [Inline review comments](inline_review_comments.json) (0)
- [Changed files and patch fragments](changed_files.json) (9)
- [Commit references](commits.json) (10)
- [Timeline events](timeline.json) (13)
- [Readable conversations and reviews](DISCUSSION.md)

## Changed files

| File | Status | + | - |
| --- | --- | ---: | ---: |
| .github/workflows/audio-pack-regression.yml | added | 32 | 0 |
| README.md | modified | 2 | 0 |
| SoftSpoken_Places/Audio.lua | modified | 4 | 1 |
| SoftSpoken_Places/SoftSpoken_Places.toc | modified | 1 | 1 |
| SoftSpoken_Places/UI/LoreWindow.lua | modified | 1 | 1 |
| SoftSpoken_Writings/Audio.lua | modified | 15 | 5 |
| SoftSpoken_Writings/SoftSpoken_Writings.toc | modified | 1 | 1 |
| tests/audio_pack_priority_guardrails.lua | modified | 6 | 0 |
| tests/audio_pack_registry_compatibility.lua | added | 82 | 0 |

