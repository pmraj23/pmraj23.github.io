# Draw & Bracket

Browser-based team draw and knockout scheduler. Paste (or upload) a list of names, generate randomized teams, then click through the bracket to advance winners. Runs entirely in your browser — no server, no signup, no data leaves the tab.

**Live:** <https://mandeep-pitta.github.io/team-gen/>

## Two modes

| Mode | File | What it does |
|---|---|---|
| **Classic** | [index.html](index.html) | Pure single-elimination. Odd rounds get one bye assigned at random — no positional advantage for top seeds. |
| **Hybrid** | [hybrid.html](hybrid.html) | Round 1 is knockout. From round 2 onward, any round with an odd team count plays as a round-robin group with the top 2 advancing. Avoids compounding byes in later rounds. |

Switch between them via the "Hybrid mode →" / "← Classic knockout" link in each page's masthead.

## Features

- **Roster input** — paste names one per line, or upload a `.txt` / `.csv` file
- **Team size** — configurable (default 2)
- **Additive draw** — if you add names to an existing roster, the new names fill any partial teams first, then form new teams appended after the existing ones. Existing team assignments and seed numbers are protected.
- **Rolling byes** — the classic bracket gives a bye only when a round has an odd count, with the recipient picked at random each round. 11 teams → 2 byes total, not 5.
- **Persistent state** — teams, bracket, and every winner pick save to `localStorage`. Survives reloads until you hit **Clear**.
- **Export / Import** — a **Backup** cluster in the masthead:
  - **Export JSON** — full state dump. Can be re-imported to restore teams, bracket, and winner picks after an accidental Clear or a browser wipe.
  - **Export text** — human-readable teams + schedule + results, for printing or sharing.
  - **Import** — pick a previously-exported JSON to restore.
- **Light / dark / system** theming — auto-follows the OS by default, click the pill in the masthead to override.
- **Print-friendly bracket** — the SVG connector rails between rounds render cleanly in print.

## Local use

Clone or download the repo and open either `index.html` or `hybrid.html` directly in a browser. No build step, no dependencies, no `npm install`.

```
git clone https://github.com/mandeep-pitta/team-gen.git
cd team-gen
open index.html
```

## Backup / recovery

State lives in your browser's `localStorage`. That means:

- It survives reloads and tab restarts.
- It does **not** survive **Clear**, private/incognito mode, browser data wipes, or moving to a different browser or device.

For anything you care about, click **Export JSON** in the masthead periodically. If disaster strikes, click **Import** on the fresh page and pick the file — teams, bracket structure, and every winner pick come back.

## Design notes

Single-file HTML per mode. Inline CSS, inline JS, no external requests, no fonts pulled from CDNs. State is one `state` object; the bracket is a list of rounds where each match points at its parents by index (or, in hybrid, by `{rrRank: N}` when the parent is a round-robin standing). Winner propagation walks the rounds forward, so a click in round 1 immediately re-populates every downstream match.

The palette (warm paper `#F2EFE7`, ink `#111311`, vermillion accent `#E24A1B`, court green `#0B4F30`) and the Impact display face are the same in both modes.

## License

MIT — see [LICENSE](LICENSE).
