# PR #39: docs: simplify credits for v0.8 beta

**Original:** https://github.com/z1xel/SoftSpoken/pull/39  
**State:** Merged  
**Author:** z1xel  
**Created:** 2026-10-29T06:05:02Z  
**Updated:** 2026-10-29T06:09:49Z  
**Closed:** 2026-10-29T06:05:15Z  
**Merged:** 2026-10-29T06:05:14Z  
**Head:** docs/concise-addon-credits (5c73ea51889cf32bf4c05d55c348959be7c10dc3)  
**Base:** main (40433174a4c8a1f28912d4ab978459e02691c3fc)

## Original description

## Summary
Simplifies the beta licensing documentation into normal WoW-addon-style credits without removing necessary copyright and license information.

- Cut `THIRD_PARTY.md` from roughly 9.6k to 3.9k characters; focused on Spoken, AI VoiceOver, relevant bundled Lua libraries, Blizzard/Wiki art and lore, and recording credits.
- Remove the lengthy speculative release-rights checklist and alarmist warnings about withholding the optional fallback audio.
- Make the README credits section brief.
- Remove the reference to ΓÇ£outstanding checksΓÇ¥ from the root MIT file.

## Preserved
- Root MIT license and original Rusty Key copyright notice.
- Unmodified upstream, Ace3, GPL and AI VoiceOver license files in `LICENSES/`.
- Accurate mention that the bundled LibDBIcon file is GPL-2.0-or-later, the original source headers, and the existing per-picture credits.
- The simple distinction between code licensing and third-party game assets/recordings.

## Scope
Documentation-only; 3 files changed, no runtime or assets altered.

## Archived records

- [PR metadata](pull_request.json)
- [General conversation comments](conversation_comments.json) (0)
- [Reviews](reviews.json) (0)
- [Inline review comments](inline_review_comments.json) (0)
- [Changed files and patch fragments](changed_files.json) (3)
- [Commit references](commits.json) (1)
- [Timeline events](timeline.json) (4)
- [Readable conversations and reviews](DISCUSSION.md)

## Changed files

| File | Status | + | - |
| --- | --- | ---: | ---: |
| LICENSE | modified | 1 | 1 |
| README.md | modified | 7 | 12 |
| THIRD_PARTY.md | modified | 70 | 158 |

