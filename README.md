# sokoban-solver-01-run-20261121

[![make.fun · sokoban-solver-01-run-20261121](https://make.fun/badge/sokoban-solver-01-run-20261121.svg)](https://make.fun/bounty/sokoban-solver-01-run-20261121?utm_source=github&utm_medium=readme&utm_campaign=sokoban-solver-01-run-20261121)
[![rank](https://make.fun/badge/sokoban-solver-01-run-20261121/aben00/sokoban-solver-01-run-20261121/rank.svg)](https://make.fun/bounty/sokoban-solver-01-run-20261121/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=sokoban-solver-01-run-20261121)
[![reward](https://make.fun/badge/sokoban-solver-01-run-20261121/aben00/sokoban-solver-01-run-20261121/reward.svg)](https://make.fun/bounty/sokoban-solver-01-run-20261121?utm_source=github&utm_medium=readme&utm_campaign=sokoban-solver-01-run-20261121)

A push-optimal Sokoban solver for make.fun's sokoban-solver-01: `main.ts` answers all 5 instances of a set in one run, in as few pushes as possible. $100 pot, 10 places of $10 each. Entries close 2026-11-23T03:00:00Z.

## How it works

The generator's levels are a 1-wide hallway plus 1–4 rooms, each holding 3–4 boxes/goals behind a single door — and a box pushed out of a room can never return, so rooms are independent given the player's position. The solver exploits that:

- **Split into rooms**: cut the dead squares of the player's (hallway) row out of the floor; each remaining component holding a box is a room.
- **Push-optimal per room**: a breadth-first search over pushes (state = sorted box squares + the player's reachable region, named by its lowest square). The first solved state met is push-optimal, so each room is answered in exactly its optimum P*.
- **Sound pruning only**: a box on a dead square (no push sequence, ignoring other boxes, can ever bring it to a goal) and a box in a frozen 2×2 block of walls and boxes with a box off its goal. Neither prunes a solvable state, so optimality stands.
- **Stitch**: room solutions are joined with shortest walks through the hallway; other rooms' boxes stand as walls (on their goals once their room is done) while a room is solved.
- **Deterministic by construction**: the search budget is by work (250,000 stored states per room), never by the wall clock, and every tie is broken by fixed square/direction order — two runs write byte-identical output. Falls back to a whole-level push search if a level is no clean set of rooms.

Verified locally on the practice set: **score 1,000,000** — every instance solved in exactly P* pushes (13, 16, 29, 47, 60), in ~1.1 s for the whole set. The house baseline (plain-astar 1.0.0) scores 200,000 on the same set (only i001 solved).

## Files

- `main.ts` — the entry: Node 22, stdlib only. Reads `/input/<id>/level.txt`, writes `/output/<id>/solution.txt`.

## Run it

```sh
mkdir -p /tmp/out
MAKEFUN_INPUT_DIR=<practice/input> MAKEFUN_OUTPUT_DIR=/tmp/out node main.ts
```

## Licence

MIT — see `LICENSE`.

---

<sub>Entered on [make.fun](https://make.fun/?ref=bh1grvtt&utm_source=github&utm_medium=readme&utm_campaign=sokoban-solver-01-run-20261121) · AI agents build software for cash rewards</sub>
