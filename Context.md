# Context: Reading Practice App

Last updated 2026-11-24. Read this first when continuing in a new chat.

## Where things are

- Live artifact: https://claude.ai/artifact/2C2czPfsfcvGic9KEkS1Ky (version 26, icon "cards"). To update it from a new chat, read the artifact first, then publish with that `url` so the link stays the same.
- Source: `reading-practice-cards.html` in this folder. It's the single source of truth, and the word bank lives inside it.
- Archive: `archive/sound-contrast-sets.js`.

## Product decisions

- An adult grades each card while the child reads aloud. Two tabs: Select and Progress. Practice replaces the page content and the tabs stay visible (no tab highlighted). There is no Done link: tapping a tab leaves practice, an empty pile returns to Progress, and every grade is already saved.
- No rounds (changed 2026-11-24, version 16). Every grade saves right away, so leaving anytime loses nothing and "where you left off" is just the untried pile. Three modes, picked automatically by `nextMode()`: untried words if any are left, else missed words, else review all. Practice shows a progress bar with "12 / 50" at the top, and a small blue pill under it naming the mode: "Practicing untried words", "Reviewing missed words" or "Reviewing all words".
- In untried mode, a missed word comes back after every 4 new words (not one shown in the last 6 cards), with an "Another try" label. Missed and review modes are one pass each.
- Select page: heading "Reading Practice Cards" with "An app for practicing reading and sounding out common 2nd grade words" The bottom button (`startBtn()`) has a label and a second line: all untried "Start Practice / N words"; some untried "Continue Practice / N untried words"; none untried but some missed "Start Practice / Review N missed words"; all known "Review Words / Review all N words".
- One set at a time: tapping a set selects it and deselects the rest. The multi-select "mix" code was removed in version 16; old saves migrate to their first set.
- No tooltips (removed in version 24). Each card has one plain description in `note` (LISTS entries and PHONICS groups): what the words practice, a few examples, and the grade or source, in 1 to 2 friendly sentences. The phonics pattern list now shows only on Progress under By pattern. The `grade` field is kept in PHONICS but not shown.
- `meter(k, big)` draws the same known/practicing/not tried bar and legend everywhere: thin on Select cards, tall with counts on Progress.
- Card format: title, word count in small muted text, then the description.
- "Sight Word Sources" has the subheading "Common words kids learn to read on sight, from widely used classroom lists." The "Phonics Patterns" heading has the subheading: "Covers the core phonics patterns of 1st and 2nd grade, starting with the most common ones."
- A word's status comes from its most recent try: known, practicing, or not tried.
- Progress footer note: "Progress is saved in this browser on this device. Data is stored 100% locally and NEVER sent to servers."
- The in-round tally (✓/✗/left) was removed on purpose.
- No results screen and no round history (removed as confusing). Finishing a pile shows a one-line "Nice work" note on Progress. The rounds log (`S.sessions`) was dropped in version 16 and is deleted from old saves.
- Progress layout, top to bottom: one labeled bar (known/practicing/not tried with counts inside, plus a legend; this replaced three count tiles), action buttons (below the bar so the choice comes after seeing the status), By pattern (phonics only, "N of 10 known" per pattern with its rule), word chips (Practicing, Known, Not tried, with no per-word numbers), Reset, note.
- Progress actions: the main button is identical to the Select button (`startBtn()`, two lines). A second soft button offers the next pile down: "Review N missed words" while untried words remain, or "Review all N words" when missed words are next.
- Select keeps its scroll position when a set is tapped.
- My own list: everything lives in its set card (cards are div role=button so the card can hold a text field). Empty and selected: the card opens to the text field. Once filled: an "Edit words" link at the bottom of the card opens the field inside the card, with Save words and Cancel. Enter saves, Shift+Enter adds a line.
- Reset on Progress reads "Reset progress" in red with "(to start over)" in gray. Confirm: "Reset progress and start over?" with Reset progress / Cancel.
- Sight word sets are ordered newest to oldest, with UFLI first.
- The "Sounds that get mixed up" section was removed and archived. Bring it back for a student who needs it.

## Phonics bank design rules

- Patterns are keyed by spelling, not sound (a_e, ai/ay and eigh are separate), because the child decodes print.
- 10 words per pattern and 50 to 70 words per group. Groups are what gets picked.
- Cumulative: each word uses only its own pattern plus patterns from earlier groups, so a miss points at the target pattern. Single consonants have no set of their own because the short vowel words cover them.
- Every pattern has a one-line rule, shown to the adult on Progress under "By pattern".
- Checks that were run: exactly 10 words per pattern, no duplicates in the bank, and every word contains its pattern's spelling. Some words also appear in the sight lists (name, home, play...). Stats are per word, so those are shared on purpose.
- Rare patterns were merged or dropped to keep the bank simple: e_e dropped; ey and eigh merged; wor and war merged.
- Ideas not built yet: nonsense words (decoding without memory), contrast rounds between two patterns (hop/hope, pin/pen), and collapsing the group cards by tier.

## Technical notes

- Single HTML file with no doctype, html or body tags (the artifact host adds them). Fonts are Andika (cards) and Lexend (UI) from Google Fonts. Theme tokens are on `:root`, with dark mode under the prefers-color-scheme and `[data-theme="dark"]` blocks.
- State: `S = {list, custom, stats:{word:{c,a,last,t}}}`. Every storage call is wrapped in try/catch. The in-memory `session` = {mode, deck, i, cur, fromDeck, since, hist}; it is never saved.
- `LISTS`: ufli, fry2, dolch2, custom, ph1–ph10 (built from `PHONICS`, with `tags`, `rules` and a `sound` map from word to pattern index). Old saved selections of archived sets fall back to UFLI.
- Key functions: `nextMode()` and `startSub()` pick and describe the pile, `start(mode)` opens practice, `pickNext()` chooses the next card (including the mixed-in missed words), `answer()` saves a grade and a snapshot so `undo()` restores both, `stop()` returns to Progress, `setCards()` draws the Select cards, `byPattern()` draws the phonics panel, and `bindSwipe()` handles 80px swipes. Keys: → right, ← practice, Backspace undo, Esc done.
- "Undo last" always takes up its space (hidden with visibility), so the Practice and Got it buttons never move.
- Phonics cards start collapsed to one row (name, count, chevron). The chevron opens a card without selecting it (`opened` set, not saved); the selected card is always open. Prototype added in version 26: collapsed cards hide the progress bar.
- The file starts with charset and viewport meta tags and sets `body{margin:0}`, so the same file works on claude.ai and as a standalone page (for example GitHub Pages). It still has no doctype, so it renders in quirks mode when standalone; tested fine.
- The tab bar has no safe-area padding: the claude.ai page around the artifact already handles it.
- Bug pass (version 23): global `box-sizing:border-box` (div set cards overflowed sideways without it); the swipe timer checks the session is still current, so tapping a tab mid-swipe can't reopen the card; the counter shows the deck position (an "Another try" card keeps the last number); long custom words wrap in chips; "N of 10" no longer pokes past the By pattern panel. Checked at 320, 390, 560 and 1024px, light and dark, with old and corrupt saved data.
- Production pass (2026-11-24): no personal data in the file; old saved data migrates; undo is guarded during the card animation; no console errors; no horizontal overflow at 320px; long words fit the card; dark mode checked.

## Research notes

- UFLI heart words come from UFLI_Heart_Word_Printable_Cards.pdf (94 words, not split by grade).
- The myView Grade 1 Cold Reads PDF has no word list, and no public myView grade 2 list was found.
- CKLA grade 2 has almost no tricky words. Really Great Reading HD Word has 99 heart words, mostly from Dolch (offered, not added).
- The phonics order follows common scope and sequence (UFLI Foundations, Orton-Gillingham).

## Working style

Direct, plain language, no em dashes, no filler, active voice. Make small UI changes directly and confirm in a sentence.
