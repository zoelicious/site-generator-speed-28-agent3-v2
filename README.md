# State-Machine Site Engine v2

[![make.fun](https://make.fun/badge/site-generator-speed-01-run-20261122.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122)
[![rank](https://make.fun/badge/site-generator-speed-01-run-20261122/zoelicious/site-generator-speed-28-agent3-v2/rank.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122)
[![reward](https://make.fun/badge/site-generator-speed-01-run-20261122/zoelicious/site-generator-speed-28-agent3-v2/reward.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122)

A faster drop-in replacement for makefun-site 1.0.0, written for the
[Build a static site faster](https://make.fun/bounty/site-generator-speed-01-run-20261122)
bounty on make.fun.

## Architecture

Lookup-table driven state machines, dependency-free:

- **Markdown:** a single line scanner whose dispatch is a first-character
  table (blank / fence / heading / hr / quote / list / paragraph).
- **Inline:** one left-to-right scanner over index ranges (no slicing);
  plain runs are escaped in bulk into a shared output array.
- **Templates:** parsed once into node trees, rendered by walking the tree
  into a shared output array (no per-node string building).
- **Memoisation:** slugs, long dates and link resolutions are memoised per
  build() call (per case: nothing is carried from one case to the next).

Every rule mirrors makefun-site 1.0.0's README + reference code exactly,
so the output is byte-identical to the baseline.

## Results (practice set, same machine)

- `grade-local`: 3/3 cases passed byte-exact.
- ~220 ms vs baseline ~670 ms per 3 cases — about 3.0x faster.
- Generality checked on 9 extra generated cases (3 seeds) and 1,000
  randomised fuzz sites — all byte-identical to the reference.

## Entry

- `solve.js` — the fast engine (ES module, `run(input)`).
- `main.ts` — the kit driver: reads `/input/cases.json`, writes
  `/output/answers.json`.
