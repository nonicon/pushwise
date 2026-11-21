# Pushwise

[![make.fun · sokoban-solver-01-run-20261121](https://make.fun/badge/sokoban-solver-01-run-20261121.svg)](https://make.fun/bounty/sokoban-solver-01-run-20261121?utm_source=github&utm_medium=readme&utm_campaign=sokoban-solver-01-run-20261121)
[![rank](https://make.fun/badge/sokoban-solver-01-run-20261121/nonicon/pushwise/rank.svg)](https://make.fun/bounty/sokoban-solver-01-run-20261121/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=sokoban-solver-01-run-20261121)
[![reward](https://make.fun/badge/sokoban-solver-01-run-20261121/nonicon/pushwise/reward.svg)](https://make.fun/bounty/sokoban-solver-01-run-20261121?utm_source=github&utm_medium=readme&utm_campaign=sokoban-solver-01-run-20261121)

Pushwise is a push-optimal Sokoban solver for make.fun's sokoban-solver-01. It solves every level of a set in one run — in as few pushes as possible, not just as few steps.

## How it works

Generated levels are a one-square-wide hallway with 1–4 small rooms off it (3–4 boxes each). A box pushed out of a room's door into the hallway can never come back, so each room is an independent problem and the level's push-optimal count is the sum of the rooms' optima. Pushwise exploits that:

- **Room decomposition** — the hallway row's dead squares (squares from which a box alone can never reach a goal) are cut out of the floor; each remaining connected part holding boxes is one room.
- **Breadth-first search over pushes** — a state is the boxes' squares plus the player's reachable region, named by its lowest square. BFS expands states in order of pushes made, so the first solved state found is push-optimal.
- **Safe deadlock pruning** — a box on a dead square, or inside a frozen 2×2 block of walls/boxes off its goal, can never be solved, so those states are never searched.
- **Deterministic by construction** — budgets are counted in states searched, never wall-clock time, and every tie is broken by fixed square/direction order, so two runs on the same set write byte-identical outputs.

## Run it

```sh
node main.ts
```

`main.ts` reads every instance from `/input/<id>/level.txt` (or `$MAKEFUN_INPUT_DIR`), writes `/output/<id>/solution.txt` (or `$MAKEFUN_OUTPUT_DIR`). Node 22 or later, no dependencies, no install step, no network, no clock.

## Results

Practice set (kit): **1,000,000 / 1,000,000** — all 5 instances solved in exactly P* pushes (13, 16, 29, 47, 60), in ~0.8 s for the whole set. The house baseline, plain-astar 1.0.0, scores 200,000 on the same set (only i001 solved). Pushwise also scored 1,000,000 on 7 freshly drawn sets from the kit's generator (different seeds), each solved in ~1 s.

## License

MIT — see [LICENSE](LICENSE).
