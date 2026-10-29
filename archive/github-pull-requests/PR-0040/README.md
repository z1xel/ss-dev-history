# PR #40: Show recorded book discovery locations in Writings narration headings

**Original:** https://github.com/z1xel/SoftSpoken/pull/40  
**State:** Merged  
**Author:** z1xel  
**Created:** 2026-10-29T06:05:33Z  
**Updated:** 2026-10-29T06:19:51Z  
**Closed:** 2026-10-29T06:12:24Z  
**Merged:** 2026-10-29T06:12:24Z  
**Head:** fix/writings-audio-discovery-heading (4c7b35c2c41a65c87c94dbe74bee33ef4b4e84f2)  
**Base:** main (3a2d5972693881bb5fd3e1b56d72b854e99bb937)

## Original description

## What changes

When a book, letter, note, scroll or other readable has a **recorded character discovery location**, add that location to the narrator's white heading:

- `Book ┬╖ Discovered in Darnassus` above `The World Tree and the Emerald Dream`.
- `Scroll ┬╖ Discovered in Orgrimmar` for a carried readable acquired in Orgrimmar, even if read later elsewhere.
- Multiple recorded sites are listed alphabetically, just as in the Compendium.
- A book discovered in an older save **without** origin information retains its old narrator heading (`Book`), with no invented location.
- When **Read Book as One Line** is off, the existing book-title heading gains the discovery label; page-number labels remain unchanged.

## Implementation

- `SoftSpoken_Writings/DiscoveryLocations.lua`: expose the recorded-location summary already used conceptually by the Compendium as `B:KnownDiscoverySummary(bookID)` (nil when unknown), plus `B:DiscoverySummary(bookID)` for the Compendium's existing explicit unknown-origin text.
- `SoftSpoken_Writings/UI/Readables.lua`: replace its duplicate local location-formatter with the shared function. No changes to listing, book placements or discovery events.
- `SoftSpoken_Writings/Audio.lua`: append known origin to the **presentation heading only**. Pack choice, queue logic, file path, playback, portraits and captions are untouched.
- Adds `tests/writings_narrator_discovery.lua` to CI. Tests live Writings code with mocks for known/unknown/legacy/acquired/multiple origins and both narration modes.

## In-game verification

Find/replay *The World Tree and the Emerald Dream* from the Writings Compendium (where its subtitle already says `Discovered in Darnassus`). The narrator heading should show `Book ┬╖ Discovered in Darnassus`. Replay a book with unknown recorded location: it should still say `Book`. Toggle **Read Book as One Line** off and confirm that page numbering stays intact.

## Archived records

- [PR metadata](pull_request.json)
- [General conversation comments](conversation_comments.json) (0)
- [Reviews](reviews.json) (0)
- [Inline review comments](inline_review_comments.json) (0)
- [Changed files and patch fragments](changed_files.json) (5)
- [Commit references](commits.json) (5)
- [Timeline events](timeline.json) (8)
- [Readable conversations and reviews](DISCUSSION.md)

## Changed files

| File | Status | + | - |
| --- | --- | ---: | ---: |
| .github/workflows/audio-pack-regression.yml | modified | 5 | 0 |
| SoftSpoken_Writings/Audio.lua | modified | 13 | 1 |
| SoftSpoken_Writings/DiscoveryLocations.lua | modified | 25 | 0 |
| SoftSpoken_Writings/UI/Readables.lua | modified | 1 | 9 |
| tests/writings_narrator_discovery.lua | added | 129 | 0 |

