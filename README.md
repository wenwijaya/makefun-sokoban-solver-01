# Sokoban solver

A push-optimal Sokoban solver, built for make.fun's
`sokoban-solver-01-run-20261121` bounty (program/v1, Node 22).

## How it works

The levels decompose into independent rooms off a hallway: a box pushed out
of a room's door can never come back, so each room is a problem of its own
and a level's push-optimal count is the sum of its rooms' optima. The solver

1. cuts the hallway's dead squares out of the floor and flood-fills the
   remaining parts into rooms;
2. solves each room push-optimally with a breadth-first search over pushes
   (a state is the boxes' squares plus the player's reachable region), pruning
   dead squares and frozen 2x2 blocks — neither prune can remove a solvable
   state, so the first solved state met is push-optimal;
3. stitches the room solutions together with the player's walks between rooms.

A whole-level push search is the fallback when a level is no set of rooms.
Everything is deterministic: no clock, no randomness, budgets counted in
states, so two runs write the same bytes.

On the kit's practice set it scores 1,000,000 (every instance solved in its
push-optimal count P\*), in under a second for the whole set — and the same
on freshly drawn sets from the published generator.

## Run

```sh
node main.ts            # reads /input/<id>/level.txt, writes /output/<id>/solution.txt
```

`MAKEFUN_INPUT_DIR` and `MAKEFUN_OUTPUT_DIR` override the two directories.

## License

MIT — see `LICENSE`.
