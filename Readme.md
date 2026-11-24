# Reading Practice App

Flash cards for early readers (1st and 2nd grade). The child reads each word out loud, and the adult marks it right or wrong. The app tracks which words are known and which still need practice.

**Live app:** https://claude.ai/artifact/2C2czPfsfcvGic9KEkS1Ky (shared as "Anyone with the link")

## How to use it

1. **Select:** Pick one word set. Each card says what it practices. Tap **Start practice**. It picks up where you left off: untried words first, then missed words, then a review of the whole set. The line under the button says which.
2. **Practice:** Swipe right or press → when the word is read correctly. Swipe left or press ← when it isn't. Backspace or "Undo last" takes back the last answer. Tap Select or Progress to stop anytime. Every answer is already saved. While working through untried words, a missed word comes back every few cards for another try.
3. **Progress:** Shows known, practicing and not tried for the picked set, buttons to continue or practice missed words, a breakdown by phonics pattern with the rule for each pattern, and the word lists.

## Word sets

**Sight Word Sources** (newest to oldest)
- UFLI heart words, 94 words (UF Literacy Institute, 2022)
- Fry words 101–200, 100 words (1980)
- Dolch 2nd grade, 46 words (1948)
- My own list: type or paste any words

**Phonics Patterns:** 10 groups, 59 patterns, 10 words each (590 words)

| # | Group | Grade | Patterns |
|---|---|---|---|
| 1 | Short vowels | Start of 1st | short a, i, o, u, e |
| 2 | Digraphs | 1st | sh, ch, th, ck, ng |
| 3 | Blends and endings | 1st | ff ll ss zz, beginning blends, ending blends, nk, wh, -s/-es |
| 4 | Silent e and soft c, g | 1st | a_e, i_e, o_e, u_e, soft c, soft g |
| 5 | Open vowels, y, -ed, -ing | 1st | open vowels, y as long i, y as long e, -ed, -ing |
| 6 | Long vowel teams | 1st | ai/ay, ee, ea, oa/oe, ow (snow), igh/ie |
| 7 | Bossy r | 1st to 2nd | ar, or/ore, er/ir/ur, wor/war, air/are/ear, ear/eer |
| 8 | More vowel teams | 2nd | oo (moon), oo (book), ew/ue/ui, ou/ow (cow), oi/oy, au/aw, al/all |
| 9 | Tricky spellings | 2nd | a after w, ea (head), -tch, -dge, silent letters, ph, ey/eigh |
| 10 | Longer words | 2nd | compound, two closed syllables, open first syllable, closed first syllable, consonant + le, schwa |

## Privacy and storage

Progress is saved only in the browser on the device that uses it (localStorage key `sightwords.v1`). Nothing is sent to a server, and people who use the shared link never see each other's data. Clearing browser data erases progress. Kids sharing one device and browser share progress. Use "Reset" on the Progress tab between kids.

## Files

- `reading-practice-cards.html`: the whole app in one file (HTML, CSS, JS, and all word lists). The live artifact is published from this file.
- `archive/sound-contrast-sets.js`: archived "Sounds that get mixed up" sets (short o vs short e, j vs w) with look-alike pairs, plus the code to bring them back for a specific student.
- `Context.md`: decisions, data model and history for continuing work in a new chat.
