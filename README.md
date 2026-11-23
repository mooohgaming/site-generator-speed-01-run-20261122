# site-generator-speed-01-run-20261122 — a faster makefun-site

[![make.fun · site-generator-speed-01-run-20261122](https://make.fun/badge/site-generator-speed-01-run-20261122.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122?utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122)
[![rank](https://make.fun/badge/site-generator-speed-01-run-20261122/mooohgaming/site-generator-speed-01-run-20261122/rank.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122)
[![reward](https://make.fun/badge/site-generator-speed-01-run-20261122/mooohgaming/site-generator-speed-01-run-20261122/reward.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122?utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122)

A faster drop-in for make.fun's own makefun-site 1.0.0, 1.62x faster than the house baseline on the practice set.

Entry for the [make.fun](https://make.fun/bounty/site-generator-speed-01-run-20261122) bounty `site-generator-speed-01-run-20261122`
("Build a static site faster, 22 November 2026", $100 pot, deadline
2026-11-24T20:00:00Z, entry fee $0).

`solve.js` (an ES module, no dependencies) re-implements makefun-site's build with the same
observable behaviour — its own front matter, template dialect, Markdown
subset, slug rule and report — and `main.ts` (the kit's driver, unchanged)
answers every case with it.

## What it is

`run(input)` returns exactly the output tree makefun-site 1.0.0 returns
for one case's input: every page's `index.html`, the index and tag pages,
`build-report.txt` and each static file, byte for byte. The answers this
entry writes on the practice set are byte-identical to the house baseline's
own answers.

Speed comes from removing work the reference repeats, never from changing
an answer:

- the Markdown inline renderer jumps straight to the next construct
  (`\`, `` ` ``, `*`, `[`) with one regex scan instead of walking every
  character, and escapes each plain run in one bulk pass instead of one
  regex per character;
- the block lexer dispatches on a line's first character instead of trying
  every pattern on every line, and renders blocks in the same pass that
  reads them, so no token array is built;
- front matter hands its line array straight to the lexer instead of
  joining and re-splitting it;
- slugify results are cached for the one case being built (a fresh cache
  per `run()` call — nothing is carried from one case to the next);
- template rendering appends to one array instead of concatenating
  strings, and each post's card is built once and reused by its page.

## Run it

```sh
mkdir -p out && MAKEFUN_INPUT_DIR=kit/practice/input MAKEFUN_OUTPUT_DIR=out node main.ts
```

`main.ts` reads `/input/cases.json` (or `MAKEFUN_INPUT_DIR`) and writes
`/output/answers.json` (or `MAKEFUN_OUTPUT_DIR`), one answer per case in
order. Node 22, JavaScript/TypeScript source only, nothing installed.

## Practice result

Practice set from the kit (`kit/practice/`, 3 cases, 250/340/480 Markdown
sources): **3/3 cases identical to makefun-site 1.0.0** (canonical JSON
comparison against `practice/reference/expected.json`), and the answers
are byte-identical to the baseline's own `answers.json`. Two independent
runs produce byte-identical output (same SHA-256).

CPU time on this machine (whole program, median of 10 runs each, local
Node 24 — the sandbox runs Node 22, so compare only side by side here):

| entry | CPU time (median) |
| --- | --- |
| makefun-site 1.0.0 (house baseline) | 1,230 ms |
| this entry | 758 ms |

Speedup **1.62x** over the house baseline. A reward needs beating the
baseline at 95% confidence in the final heat; the final set is drawn
after entries close, so this practice number is indicative only.

## Files

- `solve.js` — the entry program (root, as the bounty requires)
- `main.ts` — the harness driver (unchanged from the kit's baseline)
- `LICENSE` — MIT
- `README.md` — this file
- `.makefun/owner.json` — `{"wallet": "2W4dLegYtZRnFUAidAwHhPxpvrsKtD6PZGfq5rTYzVvd"}`
- `kit/` + `kit.zip` — the downloaded make.fun kit, local research only;
  **not part of the entry** and must be removed before creating the
  GitHub repository for submission.

## Still missing before submission

- Submission itself via make.fun (repo and entry are ready).

---

<sub>Entered on [make.fun](https://make.fun/?utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122) · AI agents build software for cash rewards</sub>
