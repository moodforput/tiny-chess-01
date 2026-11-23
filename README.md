# Tiny Chess — a 4 KB Python chess engine

[![make.fun](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01)
[![make.fun](https://make.fun/badge/tiny-chess-01/moodforput/tiny-chess-01/rank.svg)](https://make.fun/bounty/tiny-chess-01)
[![make.fun](https://make.fun/badge/tiny-chess-01/moodforput/tiny-chess-01/reward.svg)](https://make.fun/bounty/tiny-chess-01)

Entry for the [make.fun bounty "Build the strongest chess engine in 4 KB of Python"](https://make.fun/bounty/tiny-chess-01): a complete chess engine in a single `main.py` of 4,051 bytes (Python 3.13 standard library only, no dependencies).

## What it is

`main.py` is the packed form of `engine.py` (packed with the kit's `compress` command). It plays a full game through the house environment (`MAKEFUN_ENV_URL`): 0x88 board, Zobrist hashing, alpha-beta with iterative deepening, quiescence search, transposition table, MVV-LVA + killer move ordering, null-move pruning, late-move reductions and check extensions. The evaluation is material + formula piece-square tables (tapered king: middlegame vs endgame) + bishop pair + passed-pawn bonus + king pawn-shield bonus.

Search is fully deterministic: moves are chosen by a fixed node budget (80,000 nodes), never by wall-clock time, so the same position always yields the same move and a trial replays byte-for-byte.

## Run

```sh
MAKEFUN_ENV_URL=http://127.0.0.1:8080 python3 -P -s main.py
```

The engine reads the game from the environment, plays its moves, and resigns only when it sees a forced mate against itself.

## Test

```sh
python3 -m unittest discover -s test
```

13 tests: move-legality perft on five known positions (startpos, Kiwipete, positions 3/4/5 — exact node counts), eval sanity (startpos is 0, advancing a pawn favours the mover, mate-in-1 found, stalemate/checkmate detected), make/unmake round-trip, and determinism (same position, same move).

## Practice result

Practice trial-0 (6 games vs the house anchors): **Elo 1,279** — beat depth-3 (1,073) and depth-4 (1,199), drew depth-2 (855) and nodes-100k (1,392), lost to nodes-250k (1,497) and nodes-500k (1,596). Above the 1,154 practice floor; below the ~1,365 house baseline (sunfish, 50k nodes). See `PLAN.md` in the bounty directory for the full trial log.
