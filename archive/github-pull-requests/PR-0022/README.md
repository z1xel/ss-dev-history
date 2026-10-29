# PR #22: Enforce Spoken > VanillaCombined > Turtle audio-pack precedence

**Original:** https://github.com/z1xel/SoftSpoken/pull/22  
**State:** Merged  
**Author:** z1xel  
**Created:** 2026-10-28T20:47:33Z  
**Updated:** 2026-10-28T23:14:04Z  
**Closed:** 2026-10-28T20:47:56Z  
**Merged:** 2026-10-28T20:47:55Z  
**Head:** feature/audio-pack-priority-guardrails (f07e5c002cc3b09934f83d77fcc4de2e73bbdc8d)  
**Base:** main (b7e41bb9753811602fcb2eb543d70a51d60a7502)

## Original description

## Summary
- Enforce SoftSpoken Stories audio pack preference: SpokenQuestsAudio* > SSVOFallback_VanillaCombined > AI_VoiceOverData_Turtle.
- Centralize the policy in the existing sorting function, applied consistently to discovered and registered packs, without editing third-party TOCs.
- Preserve each pack's internal TOC ordering within a family; keep unknown modules ordered by declared priority after the recognized families.
- Document behavior and add Lua regression test fixtures for overlaps and fallback.

## Verification
- Branch compared to `main`: 3 commits ahead, 0 behind.
- Structural and ordering checks of the policy passed, including simulated adversarial TOC priority values.
- Lua regression script committed but not executed in this environment (Lua runtime unavailable); in-game playback still needs validation.

## Expected playback order
1. Spoken
2. VanillaCombined
3. Turtle
4. Other compatible packs

## Archived records

- [PR metadata](pull_request.json)
- [General conversation comments](conversation_comments.json) (0)
- [Reviews](reviews.json) (0)
- [Inline review comments](inline_review_comments.json) (0)
- [Changed files and patch fragments](changed_files.json) (3)
- [Commit references](commits.json) (3)
- [Timeline events](timeline.json) (6)
- [Readable conversations and reviews](DISCUSSION.md)

## Changed files

| File | Status | + | - |
| --- | --- | ---: | ---: |
| README.md | modified | 25 | 0 |
| SoftSpoken_Stories/DataModules.lua | modified | 24 | 0 |
| tests/audio_pack_priority_guardrails.lua | added | 126 | 0 |

