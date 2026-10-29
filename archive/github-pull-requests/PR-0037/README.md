# PR #37: docs: add licensing and third-party attribution for beta

**Original:** https://github.com/z1xel/SoftSpoken/pull/37  
**State:** Merged  
**Author:** z1xel  
**Created:** 2026-10-29T05:47:34Z  
**Updated:** 2026-10-29T06:09:34Z  
**Closed:** 2026-10-29T05:48:08Z  
**Merged:** 2026-10-29T05:48:08Z  
**Head:** docs/license-attribution-beta (3fa42b9a733dec96f33102abccda2ea8c1210fbb)  
**Base:** main (a4ac154bbfaa5ff99cce4ae2e8ac02692dbcd726)

## Original description

## Summary

Prepare the v0.8.0 beta for public attribution without implying that third-party content is MIT licensed.

- Add root `LICENSE` with the MIT terms and upstream copyright attribution, scoped to rights owned/licensed by the project and original contributors.
- Preserve exact source license texts: Spoken MIT, AI VoiceOver Unlicense, Ace3 and GPL v2.
- Add `THIRD_PARTY.md` covering upstream code lineage, actual bundled Lua libraries, game data and Warcraft Wiki lore, zone screenshot credits, Blizzard art, service marks and voice packs.
- Add a README licensing and credits link.
- Retain the existing per-image `SoftSpoken_Places/Textures/Pictures/CREDITS.md` catalog unchanged.

## Verified

- Branch is one commit ahead, zero commits behind `main`.
- Only documentation/licenses changed (seven files); no Lua, TOC, binary or audio file was edited.
- The bundled `LibDBIcon-1.0` explicitly declares **GPL-2.0-or-later**; the root MIT license does not override it.
- Original Spoken MIT, original AI VoiceOver Unlicense, and the Ace3/GPL license texts were copied from their respective sources.

## Outstanding before publishing *all* release assets

- [ ] Confirm permission/terms for redistributing the **1,121 recordings** in optional `Data_Packs/SSS_VOFallback`. Do not assume code licenses cover TTS/audio or Blizzard dialogue.
- [ ] Verify legacy **AceCore-3.0** provenance and confirm compliance/compatibility for the combined distributed addon including GPL LibDBIcon.
- [ ] Check individual image license / uploader attribution and modifications for the source screenshots, along with CC BY-SA 4.0 obligations for wiki-derived lore.
- [ ] Audit any newly merged asset folders and confirm release ZIPs include license documents and picture credits.

**This PR adds notices and establishes a release rights checklist; it does not certify that all third-party rights have been cleared.**

No functional tests were required because there are no runtime changes.

## Archived records

- [PR metadata](pull_request.json)
- [General conversation comments](conversation_comments.json) (0)
- [Reviews](reviews.json) (0)
- [Inline review comments](inline_review_comments.json) (0)
- [Changed files and patch fragments](changed_files.json) (7)
- [Commit references](commits.json) (1)
- [Timeline events](timeline.json) (4)
- [Readable conversations and reviews](DISCUSSION.md)

## Changed files

| File | Status | + | - |
| --- | --- | ---: | ---: |
| LICENSE | added | 31 | 0 |
| LICENSES/AI-VoiceOver-UNLICENSE.txt | added | 24 | 0 |
| LICENSES/Ace3.txt | added | 29 | 0 |
| LICENSES/GPL-2.0.txt | added | 117 | 0 |
| LICENSES/Spoken-MIT.txt | added | 29 | 0 |
| README.md | modified | 15 | 0 |
| THIRD_PARTY.md | added | 161 | 0 |

