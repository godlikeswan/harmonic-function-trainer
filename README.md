# Harmonic Function Trainer

A single-file, offline web app for drilling the conversion between **chord names** and
**roman numerals** in any key — e.g. `Dm` in A minor is `iv`, `B°` in C major is `vii°`.

No build step, no dependencies, no network. Open `index.html` in a browser.

![status](https://img.shields.io/badge/build-none-brightgreen)

## What it trains

Two directions, chosen randomly per question:

| Prompt | Answer |
| --- | --- |
| `Dm` in A minor | `iv` |
| `iv` in A minor | `Dm` |

Only diatonic triads are drilled — **major, minor and diminished**. Seventh chords are
deliberately out of scope.

## Features

- **28 keys** — 14 major and 14 minor, individually selectable, plus presets
  (common keys, circle of fifths, all major, all minor).
- **Two answer modes** — pick from choices, or type the answer. Typing accepts
  `vii°`/`viio`/`viiø`/`vii dim`/`7°` for numerals and `Dm`/`Dmin`/`d minor`/`D-` for chords.
- **Adaptive scheduling** — each `(key, direction, degree)` pair is scheduled on its own
  history rather than at random. Priority blends error rate, consecutive misses,
  response time, and staleness. Review intervals are per-item and derived from your own
  pace, so items you keep fumbling keep returning and items you know fade out.
- **Non-diatonic traps** (optional) — chords borrowed from the parallel mode, where the
  correct answer is "not in this key".
- **Correct spelling** — triads are spelled from the key signature, so `C♯` major yields
  `E♯` and not `F`, and extreme keys produce real double accidentals where the theory
  demands them.
- **Progress tracking** — per-key accuracy and a live "up next" queue showing why each
  item is prioritised. Stored in `localStorage`; nothing leaves the browser.
- Keyboard: `1`–`5` to choose, `Enter` to check/continue, `H` for a hint, `K` keys,
  `S` settings.

## Notation conventions

- **Case is significant.** Uppercase is major (`I`, `IV`, `V`), lowercase is minor
  (`i`, `iv`, `v`), and diminished takes a degree sign (`vii°`) or a plain `o`.
- **Minor means natural minor.** The diatonic set is `i ii° III iv v VI VII`. The
  harmonic-minor dominant (`V`, e.g. E major in A minor) is not part of the drilled set —
  it only appears via the trap mode.
- **Enharmonic tolerance in typing mode.** `D♭dim` is accepted where `C♯dim` was
  expected, with a note that the spelling differs. Roman-numeral case is graded strictly
  but explained when wrong.

## Running it

```
open index.html          # macOS
start index.html         # Windows
```

Or serve it over HTTP if you prefer:

```
python -m http.server 8000
```

## Project layout

```
index.html    the entire app — markup, styles, theory engine, scheduler
```

The `<script>` section is divided into numbered sections: theory, state, helpers,
scheduler, question generation, answer parsers, rendering, panels.

## Known limitations

- Natural minor only (see above).
- Triads only; no sevenths, inversions, or secondary dominants.
- Progress is per-browser; there is no account or sync.
- Storage is keyed by `roman-numeral-trainer.v1` in `localStorage` and survives schema
  migrations for the older stat format.

## License

MIT — see [LICENSE](LICENSE).
