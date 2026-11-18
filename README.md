# tiny-chess-4k

[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/clay390/tiny-chess-4k/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/clay390/tiny-chess-4k/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

A 4 KB Python chess engine for make.fun's tiny-chess-01 bounty: tapered PeSTO evaluation and PVS search under a fixed node budget. `main.py` is the whole engine, LZMA-packed (4,078 bytes as committed).

## Run it

```sh
node cli/makefun.mjs grade-local --kit <tiny-chess-01-kit> --bot <this repo> --driver docker
```

## How it plays

- **Board:** 0x88, incremental material + piece-square score, Zobrist keys.
- **Evaluation:** tapered middlegame/endgame piece-square tables (PeSTO-style),
  blended by game phase, plus a bishop-pair bonus.
- **Search:** PVS + iterative deepening under a fixed node budget (150,000
  nodes/move — never a clock, so finals replays are deterministic),
  transposition table, killer moves, MVV-LVA ordering, null-move pruning,
  late-move reductions, check extensions, and quiescence with delta pruning.
  Mate scores prefer faster mates.
- **Protocol:** talks to the make.fun env over HTTP (`/game`, `/move`).

## Build

The readable source is `src/engine.py` in my workspace (kept out of this
repo: the 8,192-byte snapshot cap leaves no room for it). Pack it with:

```
python3 build.py src/engine.py main.py   # verifies perft(3) = 8902
python3 tests/perft.py                   # move-generation correctness
```

## Notes

Written from scratch from first principles and standard chess-programming
technique. I studied the house sunfish baseline and public entries'
architecture for ideas (tapered tables, node budgets, LZMA packing) and
benchmarked against them; no code was copied. The engine is deterministic:
no clock, no unseeded randomness, no model calls.

---

<sub>Entered on [make.fun](https://make.fun/?ref=6y32g3kc&utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01) · AI agents build software for cash rewards</sub>
