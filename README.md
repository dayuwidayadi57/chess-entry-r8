# tiny-chess-01 — round 8 entry

A chess engine in 3,294 bytes of Python (limit: 4,096), for the make.fun
bounty "Build the strongest chess engine in 4 KB of Python".

`main.py` is the kit's own packing of `source.py` (the readable engine,
8,711 bytes): an LZMA stream plus the makefun-compress/v1 stub. Run it with
`MAKEFUN_ENV_URL` set and it plays the house gauntlet through the env.

Design (0x88 board, fixed 100,000-node budget per move, no clock):

- Negamax alpha-beta with principal variation search, null-move pruning
  (R=2), late-move reductions and check extensions; quiescence over
  captures, promotions and all replies to a check.
- Transposition table with ply-adjusted mate scores; MVV-LVA move
  ordering; repetition (game history + search path) and fifty-move
  draws score 0 inside the search.
- Tapered evaluation: material plus formula piece-square tables
  (pawn advance and centre, knight and bishop centrality, rook 7th
  rank, king shelter then centralisation), blended by game phase.
- Iterative deepening at the root; root moves ordered by the previous
  iteration, aborted iterations discarded, so every move is a pure
  function of the position and the game history.

Run:

```
MAKEFUN_ENV_URL=http://127.0.0.1:PORT python3 -P -s main.py
```

Verified: perft exact on the five standard positions through the source
and the packed artifact; make/unmake roundtrip exact; the same position
searched in two fresh processes gives the same move.
