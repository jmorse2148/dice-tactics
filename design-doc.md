# Dice Tactics — Design Doc (Draft 1)

## Concept
A 2-player tactical board game where dice ARE the pieces. Instead of larger
dice simply being "stronger" (as in D&D, where a d12 weapon just hits
harder than a d4 dagger), every die type is viable — they just win in
different ways. "Choose your weapon" should actually mean something.

## The Board
- Core grid: **5 columns × 8 rows**
- One additional square at **top-center** and **bottom-center**, outside
  the main grid but connected to it — this is each player's **throne
  square**, where their d20 sits.
- The d20 does not move off the throne square (TBD — confirm it's
  immobile, or can it retreat within the throne area?).

## The Pieces
Each player fields a full set of standard polyhedral dice:
- 1× d20 — **not a game piece**, represents the player's **life total**.
  Sits on the throne square.
- d4, d6, d8, d10, d12 — the actual playable pieces ("pawns").

## Combat & HP
Each die (including the d20) has its own HP pool, starting at its max
face value (a d12 starts at 12 HP, a d4 at 4 HP, etc). Damage dealt by
an attack roll is subtracted from the defending die's current HP.

- **HP persists across turns** — it does not reset between turns or
  between attacks. If a d4 swings 3 times at a d12 and rolls a 1 each
  time, the d12 is now sitting at 9 HP, and stays there into the
  opponent's next turn and beyond, until it's healed (if healing exists
  — TBD) or destroyed.
- A die is destroyed/removed from the board when its HP reaches 0.
- The d20's HP pool IS the life total — same mechanic, just on the
  throne square instead of the main grid.

## Healing
**Status: deferred from the prototype for now.** The ruleset below is
fully decided and was briefly implemented and playtested in the HTML
prototype, but it's been pulled back out ahead of moving the prototype
online (two-player over a shared link) — simpler to get the core loop
solid for multiplayer first, then layer healing back in once state is
synced between two browsers. The rules themselves aren't in question,
just the build order.

Each player has a small sideboard of **3× d4 healing potion dice**,
sitting outside the main grid.

**RESOLVED — full healing ruleset:**
- **Own full turn.** On your turn you either heal, or move/attack with
  a piece — never both. A turn spent healing uses none of your
  move/attack budget, and vice versa. (This also resolves the earlier
  "order of operations for a combined turn" question — there's no
  combined turn to order, since healing and move/attack are mutually
  exclusive.)
- **Single-use.** Each of the 3 potion dice is consumed once rolled.
  Once a player has used all 3, they have no more healing for the rest
  of the round.
- **Any piece, including the d20.** The life-total die can be healed
  just like any pawn.
- **No range/proximity requirement.** Choose any piece on the board,
  roll a d4, add that result to its current HP (capped at that piece's
  max HP — TBD confirm the cap still applies, but assumed yes since HP
  pools are fixed to each die's max face value).

## Core Balancing Philosophy
Smaller dice are **not** weaker — they trade raw hit size for frequency
and mobility. Bigger dice hit harder but act less often and move less.

### Full Dice Comparison (RESOLVED — draft 2 values, pending playtest)
| Die | Max HP / dmg | Attacks/turn | Total potential dmg | Movement | Direction |
|-----|---|---|---|---|---|
| d4  | 4  | 3 | 12 | 6 squares | Diagonal only |
| d6  | 6  | 2 | 12 | 4 squares | Orthogonal only (N/S/E/W) |
| d8  | 8  | 1 | 8  | 3 squares | Omnidirectional |
| d10 | 10 | 1 | 10 | 2 squares | Omnidirectional |
| d12 | 12 | 1 | 12 | 1 square  | Omnidirectional |

**The resulting gradient:**
- **d4 / d6** — "sprinters." Fast and hit many times per turn, but each
  locked to a single axis of movement — the d4 moves diagonally only,
  the d6 moves orthogonally only. This gives them distinct flanking
  identities rather than just being "the same but smaller": the d4 can
  cut corners and slip around blockers but can't approach in a straight
  line, while the d6 is the reverse. Highest total potential damage per
  turn relative to their own HP, but both are easy to out-position by a
  player who understands which axis they're restricted to.
- **d8 / d10 / d12** — "bruisers." Omnidirectional but slow, one attack
  per turn. As they get bigger, damage per hit goes up but movement
  drops — d8 trades raw damage for extra reach (3 vs. d10's 2, vs.
  d12's 1).
- Notably, d8 and d10's *total potential damage per turn* (8 and 10) no
  longer hits the "12" target the d4/d6 formula used — this is an
  intentional break from the original ×12 rule, compensated by movement
  instead of damage. Flagging this clearly since it's a deliberate
  asymmetry, not an oversight, in case it needs to be revisited during
  playtesting.

## Movement Preview
When a die is selected, the game must show the legal destination squares
before the player commits to a move — similar to a chess move-highlight.
Flagged as a **core requirement for the prototype**, not a later polish
item, since the balancing itself (how far/how flexibly each die can move)
needs to be felt and tested directly, not just reasoned about on paper.

## Attack Range
Base game is **melee only** — a die must end its move adjacent to the
target to attack it. Ranged attacks are a good idea but are being
deferred: likely implemented later as a **power-up/upgrade mechanic**
rather than a baseline trait of any die. Not part of the core
prototype loop.

## Win Condition
Reduce the opponent's d20 (life total) to 0 by landing attacks on it.
**RESOLVED: the d20 can be attacked directly at any time** — there's no
requirement to clear defenders or "open up" the throne first.

## The d20 (Throne Piece)
**RESOLVED: the d20 moves, using the same movement pattern as a d12** — one
square, any direction (including diagonals) — **but it never attacks.** It
starts on the throne square and can step off it onto the board, and back
on, like any other move.
- The throne square itself only connects to the board at one point: the
  single square directly in front of it (center column, nearest row). From
  the throne, that square is the d20's only possible move — if that square
  is occupied (including by one of its own pieces, which is the case in
  the default starting formation, where the d8 sits directly in front of
  the throne), the d20 cannot move that turn.
- Once on the board, it moves like a normal d12 — one square any direction
  — and can step back onto its own throne from the connecting square,
  exactly like any other board move.
- **Confirmed intentional:** the default starting formation puts the d8
  directly on that connecting square, so the king is boxed in and can't
  move at all until the d8 relocates. The throne is meant to be naturally
  guarded by your own front line, not freely mobile from turn one.
- Because it has 0 attacks, selecting the d20 and using its one move
  always ends the turn immediately (no attacks left to spend afterward).

## Turn Structure
Each player selects one die per turn and can **both move and attack
with it in the same turn** (not mutually exclusive). Turns alternate
chess-style.

**RESOLVED — move-then-attack lock, but order before that is flexible:**
a piece can move (using any or all of its remaining movement budget,
across multiple separate moves) any number of times before it attacks,
but **once it attacks, it cannot move again for the rest of that turn**.
It can still use any additional attacks it has (e.g. a d4's remaining
swings), just not reposition. This gives attacking real weight as a
commitment — you can't strike and then retreat out of danger in the
same turn. **Confirmed: a piece does not have to move first — it can
attack immediately on selection if it starts the turn already adjacent
to a target** (attack-only turns are legal, not just move-then-attack).

## Open Questions Summary
1. ~~Can the d20 be attacked directly, or must the throne square be "opened up" first?~~ **RESOLVED: attackable directly, any time.**
2. ~~Is the d20 fully immobile?~~ **RESOLVED: no — moves like a d12 (1 square, any direction), never attacks.**
3. ~~Within a turn, does the die have to move before attacking, or is order flexible?~~ **RESOLVED: flexible — a piece can attack without having moved first.**
4. ~~Is using a healing potion its own full turn, or combinable with a move/attack?~~ **RESOLVED: own full turn, mutually exclusive with move/attack.**
5. ~~Are potion dice single-use or reusable across turns?~~ **RESOLVED: single-use — 3 per player per round, then gone.**
6. ~~Can the d20 itself be healed, or only pawns?~~ **RESOLVED: yes, any piece including the d20.**
7. ~~Does healing require proximity/range, or can it target anywhere on the board?~~ **RESOLVED: no range requirement — target any piece on the board.**
8. Hit/miss mechanics — does an attack always land for at least some damage, or can a roll result in a total miss? **Deferred — liked as a future direction, not ready to add yet.**
9. ~~Occupied squares — can two pieces share a square?~~ **RESOLVED: no, two pieces can never share a square.** The d10-hops-over-pieces behavior (an artifact of how its movement is coded, inconsistent with the other dice which block fully) **is liked as a future direction but deferred for now** — not being changed or formalized yet.
10. ~~What happens to a destroyed die physically?~~ **RESOLVED: removed for the rest of the round — no respawn.**
11. ~~Starting positions — fixed or draft phase?~~ **RESOLVED: fixed formation** (the current default layout).
12. ~~Order of operations if a turn combines multiple actions~~ — **moot**, resolved by #4: healing and move/attack are mutually exclusive, so no turn ever combines them.
13. ~~Stalemate handling~~ — **RESOLVED: draws are allowed.** No turn limit or tiebreaker; if neither d20 reaches 0, the game can end in a draw.

## Future Ideas (post-prototype)
- **Ranged attacks** as a power-up/upgrade some dice can acquire, rather than a baseline trait
- **Hit/miss mechanics** on attack rolls (a roll could whiff entirely rather than always landing for some damage)
- **D10 hopping over occupied squares** as a deliberate, formalized trait (rather than the current side effect of how its movement happens to be coded)

---
*This is a living document — update as balancing decisions get made during playtesting.*
